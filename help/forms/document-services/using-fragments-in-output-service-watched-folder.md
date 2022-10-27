---
title: 出力サービスでのフラグメントの使用
description: crx リポジトリにあるフラグメントを使用して pdf ドキュメントを生成します
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 1%

---

# ECMA スクリプトを使用したフラグメントを含む PDF ドキュメントの生成{#developing-with-output-and-forms-services-in-aem-forms}


この記事では、xdp フラグメントを使用して pdf ファイルを生成する際に、出力サービスを使用します。 メインの xdp とフラグメントは crx リポジトリに存在します。 AEMのファイルシステムフォルダー構造を模倣することが重要です。 例えば、xdp の fragments フォルダーでフラグメントを使用する場合は、 **フラグメント** をAEMのベースフォルダーに追加します。 基本フォルダーには基本 xdp テンプレートが格納されます。 たとえば、ファイルシステムに次の構造がある場合、
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* フォルダー xdpdocuments には、基本テンプレートとフラグメントが **フラグメント** フォルダー

必要な構造は、 [フォームとドキュメント ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

次に、2 つのフラグメントを使用するサンプル xdp のフォルダー構造を示します
![forms&amp;document](assets/fragment-folder-structure-ui.png)


* Output Service — 通常、このサービスは、xdp テンプレートまたは pdf と xml データを結合して統合された pdf を生成するために使用されます。 詳しくは、 [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) Output サービスの このサンプルでは、crx リポジトリ内のフラグメントを使用します。


次の ECMA スクリプトは、generatePDFを使用しました。 コード内で ResourceResolver と ResourceResolverHelper が使用されていることに注意してください。 ResourceRelover は、このコードがユーザーコンテキストの外部で実行されるので必要です。

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

**システム上のサンプルパッケージをテストするには**
* [DevelopingWithServiceUSer バンドルのデプロイ](assets/DevelopingWithServiceUser.jar)
* エントリを追加 **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** 次のスクリーンショットに示すように、ユーザーマッパーサービスの修正内容。
   ![ユーザーマッパーの修正](assets/user-mapper-service-amendment.png)
* [サンプル xdp ファイルと ECMA スクリプトをダウンロードして読み込みます](assets/watched-folder-fragments-ecma.zip).
これにより、c:/fragmentsandoutputservice フォルダーに監視フォルダー構造が作成されます

* [サンプルデータファイルを抽出します。](assets/usingFragmentsSampleData.zip) 監視フォルダーの install フォルダー (c:\fragmentsandoutputservice\install) に配置します。

* 生成された PDF ファイルの監視フォルダー設定の結果フォルダーを確認します。
