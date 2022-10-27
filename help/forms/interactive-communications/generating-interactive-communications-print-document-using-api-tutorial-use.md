---
title: 監視フォルダーメカニズムを使用した印刷チャネル用のインタラクティブ通信ドキュメントの生成
seo-title: Generating Interactive Communications Document for print channel using watch folder mechanism
description: 監視フォルダーを使用して印刷チャネルドキュメントを生成する
seo-description: Use watched folder to generate print channel documents
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f5ab4801-cde5-426d-bfe4-ce0a985e25e8
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 2%

---

# 監視フォルダーメカニズムを使用した印刷チャネル用のインタラクティブ通信ドキュメントの生成

印刷チャネルドキュメントの設計とテストが完了したら、通常は REST 呼び出しを行ってドキュメントを生成するか、監視フォルダーメカニズムを使用して印刷ドキュメントを生成する必要があります。

この記事では、監視フォルダーのメカニズムを使用して印刷チャネルドキュメントを生成する使用例を説明します。

ファイルを監視フォルダーにドロップすると、監視フォルダーに関連付けられたスクリプトが実行されます。 このスクリプトは、以下の記事で説明します。

監視フォルダーにドロップされたファイルの構造は次のとおりです。 このコードは、XML ドキュメントにリストされているすべてのアカウント番号に関するステートメントを生成します。

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

以下のコードをリストすると、次の処理がおこなわれます。

行 1 - InteractiveCommunicationsDocument へのパス

行 15～20:監視フォルダーにドロップされた XML ドキュメントからアカウント番号のリストを取得します

24～25 行目：ドキュメントに関連付けられた PrintChannelService と Print Channel を取得します。

30 行目：accountnumber をキー要素としてフォームデータモデルに渡します。

32～36 行目：生成するドキュメントのデータオプションを設定します。

行 38:ドキュメントをレンダリングします。

39～40 行目 — 生成したドキュメントをファイルシステムに保存します。

フォームデータモデルの REST エンドポイントには、ID が入力パラメーターとして必要です。 この id は、以下のスクリーンショットに示すように、「accountnumber」と呼ばれる要求属性にマッピングされます。

![requestattribute](assets/requestattributeprintchannel.gif)

```java
var interactiveCommunicationsDocument = "/content/forms/af/retirementstatementprint/channels/print/";
var saveLocation =  new Packages.java.io.File("c:\\scrap\\loadtesting");

if(!saveLocation.exists())
{
 saveLocation.mkdirs();
}

var inputMap = processorContext.getInputMap();
var entry = inputMap.entrySet().iterator().next();
var inputDocument = inputMap.get(entry.getKey());
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
var resourceResolver = aemDemoListings.getServiceResolver();
var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var dbFactory = Packages.javax.xml.parsers.DocumentBuilderFactory.newInstance();
var dBuilder = dbFactory.newDocumentBuilder();
var xmlDoc = dBuilder.parse(inputDocument.getInputStream());
var nList = xmlDoc.getElementsByTagName("accountnumber");
for(var i=0;i<nList.getLength();i++)
{
 var accountnumber = nList.item(i).getTextContent();
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
   var printChannelService = sling.getService(Packages.com.adobe.fd.ccm.channels.print.api.service.PrintChannelService);
   var printChannel = printChannelService.getPrintChannel(interactiveCommunicationsDocument);
   var options = new Packages.com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
   options.setMergeDataOnServer(true);
   options.setRenderInteractive(false);
   var map = new Packages.java.util.HashMap();
   map.put("accountnumber",accountnumber);
    // Required Data Options
   var dataOptions = new Packages.com.adobe.forms.common.service.DataOptions(); 
   dataOptions.setServiceName(printChannel.getPrefillService()); 
   dataOptions.setExtras(map); 
   dataOptions.setContentType(Packages.com.adobe.forms.common.service.ContentType.JSON);
   dataOptions.setFormResource(resourceResolver.resolve(interactiveCommunicationsDocument));
            options.setDataOptions(dataOptions); 
    var printDocument = printChannel.render(options);
   var statement = new Packages.com.adobe.aemfd.docmanager.Document(printDocument.getInputStream());
            statement.copyToFile(new Packages.java.io.File(saveLocation+"\\"+accountnumber+".pdf"));

      }
   });
}
```


**ローカルシステムでこれをテストするには、次の手順に従ってください。**

* Tomcat を設定します。 [記事。](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat には、サンプルデータを生成する war ファイルがあります。
* この説明に従って、サービスユーザー（システムユーザー）を設定します。 [記事](/help/forms/adaptive-forms/service-user-tutorial-develop.md).
このシステムユーザーが次のノードに対する読み取り権限を持っていることを確認します。 権限をにログインさせるには、以下を実行します。 [ユーザー管理者](https://localhost:4502/useradmin) を検索し、Tab キーで「権限」タブに移動して、次のノードで読み取り権限を付与します。
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* パッケージマネージャーを使用して、次のパッケージをAEMにインポートします。 このパッケージには、次の内容が含まれます。


* [Interactive Communications ドキュメントの例](assets/retirementstatementprint.zip)
* [監視フォルダースクリプト](assets/printchanneldocumentusingwatchedfolder.zip)
* [データソース設定](assets/datasource.zip)

* /etc/fd/watchfolder/scripts/PrintPDF.ecmaファイルを開きます。 1 行目の interactiveCommunicationsDocument へのパスが、印刷する正しいドキュメントを指していることを確認します

* 行 2 の設定に従って saveLocation を変更します。

* 次の内容の accountnumbers.xml ファイルを作成します

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```


* accountnumbers.xml をC:\RenderPrintChannel\input folderディレクトリにドロップします。

* 生成されたPDFファイルは、ecma スクリプトで指定された saveLocation に書き込まれます。

>[!NOTE]
>
>Windows 以外のオペレーティングシステムで使用する場合は、次の URL に移動してください：
>
>/etc/fd/watchfolder /config/PrintChannelDocument を編集し、設定に従って folderPath を変更します。
