---
title: 監視フォルダーのメカニズムを使用した印刷チャネル用のインタラクティブなコミュニケーションドキュメントの生成
description: 監視フォルダーを使用して印刷チャネルドキュメントを生成する
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f5ab4801-cde5-426d-bfe4-ce0a985e25e8
last-substantial-update: 2019-07-07T00:00:00Z
duration: 115
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 100%

---

# 監視フォルダーのメカニズムを使用した印刷チャネル用のインタラクティブなコミュニケーションドキュメントの生成

印刷チャネルドキュメントの設計とテストが完了したら、通常は REST 呼び出しを行ってドキュメントを生成するか、監視フォルダーのメカニズムを使用して印刷ドキュメントを生成する必要があります。

この記事では、監視フォルダーのメカニズムを使用して印刷チャネルドキュメントを生成する使用例を説明します。

ファイルを監視フォルダーにドロップすると、監視フォルダーに関連付けられたスクリプトが実行されます。 このスクリプトについては、この記事の後で説明します。

監視フォルダーにドロップされたファイルの構造は次のとおりです。 このコードは、XML ドキュメントにリストされているすべての accountnumber に関するステートメントを生成します。

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

以下のコードでは、次の処理が行われます。

1 行目：InteractiveCommunicationsDocument へのパス

15～20 行目：監視フォルダーにドロップされた XML ドキュメントから accountnumber のリストを取得します

24～25 行目：ドキュメントに関連付けられた PrintChannelService と印刷チャネルを取得します。

30 行目：accountnumber をキー要素としてフォームデータモデルに渡します。

32～36 行目：生成するドキュメントのデータオプションを設定します。

38 行目：ドキュメントをレンダリングします。

39～40 行目：生成したドキュメントをファイルシステムに保存します。

フォームデータモデルの REST エンドポイントには、入力パラメーターとして ID が必要です。以下のスクリーンショットに示すように、この ID は accountnumber と呼ばれるリクエスト属性にマッピングされます。

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

* [こちらの記事にあるように、Tomcat を設定します。](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)Tomcat には、サンプルデータを生成する war ファイルがあります。
* [こちらの記事](/help/forms/adaptive-forms/service-user-tutorial-develop.md)の説明に従って、サービスユーザー（システムユーザー）を設定します。
このシステムユーザーが次のノードに対する読み取り権限を持っていることを確認します。 権限を付与するには、[ユーザー管理者](https://localhost:4502/useradmin)でログインしてシステムユーザーの「データ」を検索し、「権限」タブに移動して、次のノードで読み取り権限を付与します。
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* パッケージマネージャーを使用して、次のパッケージを AEM に読み込みます。 このパッケージには、次の内容が含まれます。


* [インタラクティブコミュニケーションドキュメントの例](assets/retirementstatementprint.zip)
* [監視フォルダースクリプト](assets/printchanneldocumentusingwatchedfolder.zip)
* [データソース設定](assets/datasource.zip)

* the /etc/fd/watchfolder/scripts/PrintPDF.ecma ファイルを開きます。 1 行目の interactiveCommunicationsDocument へのパスが、印刷する正しいドキュメントを指していることを確認します

* 2 行目の設定に従って saveLocation を変更します。

* 次の内容の accountnumbers.xml ファイルを作成します。

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


* accountnumbers.xml を C:\RenderPrintChannel\input フォルダーにドロップします。

* 生成された PDF ファイルは、ecma スクリプトで指定された saveLocation に書き込まれます。

>[!NOTE]
>
>Windows 以外のオペレーティングシステムで使用する場合は、/etc/fd/watchfolder /config/PrintChannelDocument に移動し、
>
>設定に応じて folderPath を変更します。
