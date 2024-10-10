---
title: 監視フォルダーでの 出力サービスでのフラグメントの使用
description: crx リポジトリにあるフラグメントを使用して PDF ドキュメントを生成します。
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
exl-id: 6b0bd2f1-b8ee-4f96-9813-8c11aedd3621
duration: 84
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '347'
ht-degree: 100%

---

# ECMA スクリプトを使用したフラグメントを含む PDF ドキュメントの生成{#developing-with-output-and-forms-services-in-aem-forms}


この記事では、xdp フラグメントを使用して PDF ファイルを生成する際に、出力サービスを使用します。 メインの xdp とフラグメントは crx リポジトリに存在します。 AEM のファイルシステムフォルダー構造を模倣することが重要です。 例えば、xdp の fragments フォルダーでフラグメントを使用する場合は、AEM のベースフォルダーに **fragments** というフォルダーを作成する必要があります。ベースフォルダーにはベース xdp テンプレートが格納されます。例えば、ファイルシステムに次の構造がある場合
* c:\xdptemplates - これには、ベースの xdp テンプレートが含まれます
* c:\xdptemplates\fragments - このフォルダーにはフラグメントが含まれ、メインテンプレートは以下に示すようにフラグメントを参照します。
  ![fragment-xdp](assets/survey-fragment.png)。
* フォルダー xdpdocuments には、ベーステンプレートと **fragments** フォルダー内のフラグメントが含まれます。

[フォームとドキュメント UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) を使用して、必要な構造を作成できます。

次に、2 つのフラグメントを使用するサンプル xdp のフォルダー構造を示します
![フォームとドキュメント](assets/fragment-folder-structure-ui.png)


* Output サービス - 通常、 xml データを xdp テンプレートまたは PDF と結合して、統合された PDF を生成するために使用されます。詳細については、出力サービスの [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) を参照してください。このサンプルでは、crx リポジトリ内のフラグメントを使用します。


PDF の生成には次の ECMA スクリプトが使用されました。コードでは ResourceResolver と ResourceResolverHelper が使用されていることに注意してください。ResourceRelover は、このコードがユーザーコンテキストの外部で実行されるので必要です。

```java
var inputMap = processorContext.getInputMap();
var itr = inputMap.entrySet().iterator();
var entry = inputMap.entrySet().iterator().next();
var xmlData = inputMap.get(entry.getKey());
log.info("Got XML Data File");

var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
log.info("Got service resolver");
var resourceResolver = aemDemoListings.getFormsServiceResolver();
//The ResourceResolverHelper execute's the following code within the context of the resourceResolver 
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
             //var statement = new Packages.com.adobe.aemfd.docmanager.Document("/content/dam/formsanddocuments/xdpdocuments/main.xdp",resourceResolver);
               var outputService = sling.getService(Packages.com.adobe.fd.output.api.OutputService);
            var pdfOutputOptions = new Packages.com.adobe.fd.output.api.PDFOutputOptions();
            pdfOutputOptions.setContentRoot("crx:///content/dam/formsanddocuments/xdpdocuments");
            pdfOutputOptions.setAcrobatVersion(Packages.com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
            var dataMergedDocument = outputService.generatePDFOutput("main.xdp",xmlData,pdfOutputOptions);
               //var dataMergedDocument = outputService.generatePDFOutput(statement,xmlData,pdfOutputOptions);
            processorContext.setResult("mergeddocument.pdf",dataMergedDocument);
            log.info("Generated the pdf document with fragments");
      }

 });
```

**システム上のサンプルパッケージをテストするには：**
* [DevelopingWithServiceUSer バンドルをデプロイします。](assets/DevelopingWithServiceUser.jar)
* 以下のスクリーンショットに示すように、ユーザーマッパーサービス修正にエントリ **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** を追加します。
  ![ユーザーマッパーの修正](assets/user-mapper-service-amendment.png)
* [サンプル xdp ファイルと ECMA スクリプトをダウンロードして読み込みます。](assets/watched-folder-fragments-ecma.zip)
これにより、c:/fragmentsandoutputservice フォルダーに監視フォルダー構造が作成されます。

* [サンプルデータファイルを抽出](assets/usingFragmentsSampleData.zip)して、監視フォルダーのインストールフォルダー（c:\fragmentsandoutputservice\install）に配置します。

* 生成された PDF ファイルの監視フォルダー設定の結果フォルダーを確認します。
