---
title: 監視フォルダーメカニズムを使用した印刷チャネル用のインタラクティブ通信ドキュメントの生成
seo-title: 監視フォルダーメカニズムを使用した印刷チャネル用のインタラクティブ通信ドキュメントの生成
description: 監視フォルダーを使用した印刷チャネルドキュメントの生成
seo-description: 監視フォルダーを使用した印刷チャネルドキュメントの生成
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---


# 監視フォルダーメカニズムを使用した印刷チャネル用のインタラクティブ通信ドキュメントの生成

印刷チャネルのドキュメントを設計およびテストした後は、通常、REST呼び出しを行ってドキュメントを生成するか、監視フォルダーのメカニズムを使用して印刷ドキュメントを生成する必要があります。

この記事では、監視フォルダーのメカニズムを使用して印刷チャネルードキュメントを生成する場合の使用例を説明します。

ファイルを監視フォルダーにドロップすると、監視フォルダーに関連付けられたスクリプトが実行されます。 このスクリプトについては、下の記事で説明します。

監視フォルダーに配置されるファイルの構造は次のとおりです。 このコードは、XMLドキュメントにリストされているすべてのアカウント番号に関するステートメントを生成します。

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

以下のコードは次の処理を行います。

行1 - InteractiveCommunicationsDocumentへのパス

行15 ～ 20:監視フォルダーにドロップされたXMLドキュメントーから、accountnumbersのリストを取得します

24-25行目：ドキュメントに関連付けられたPrintChannelServiceおよびPrintチャネルを取得します。

30行目：accountnumberをキー要素としてフォームデータモデルに渡します。

32～36行目：生成するドキュメントの「Data Options」を設定します。

38行目：ドキュメントをレンダリングします。

39 ～ 40行：生成されたドキュメントをファイルシステムに保存します。

フォームデータモデルのRESTエンドポイントでは、IDが入力パラメーターとして必要です。 このidは、以下のスクリーンショットに示すように、「accountnumber」というリクエスト属性にマップされます。

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


**ローカルシステムでテストするには、次の手順に従ってください。**

* この[記事の説明に従って、Tomcatを設定します。](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcatには、サンプルデータを生成するwarファイルがあります。
* この[記事](/help/forms/adaptive-forms/service-user-tutorial-develop.md)で説明するように、サービス（システムユーザー）を設定します。
このシステムユーザーが次のノードで読み取り権限を持っていることを確認してください。 [ユーザーadmin](https://localhost:4502/useradmin)に権限ログインを与え、システムユーザー「data」を検索し、次のノードでTabキーで「権限」タブに移動して読み取り権限を付与するには
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* パッケージマネージャーを使用して、次のパッケージをAEMに読み込みます。 このパッケージには、次の内容が含まれています。


* [対話型通信ドキュメントの例](assets/retirementstatementprint.zip)
* [監視フォルダースクリプト](assets/printchanneldocumentusingwatchedfolder.zip)
* [データソース設定](assets/datasource.zip)

* /etc/fd/watchfolder/scripts/PrintPDF.ecmaファイルを開きます。 1行目のinteractiveCommunicationsDocumentのパスが、印刷する正しいドキュメントを指していることを確認してください

* 2行目の環境設定に従ってsaveLocationを変更します

* 次の内容のaccountnumbers.xmlファイルを作成します

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


* accountnumbers.xmlをC:\RenderPrintChannel\input folderフォルダーにドロップします。

* 生成されたPDFファイルは、ecmaスクリプトで指定されたsaveLocationに書き込まれます。

>[!NOTE]
>
>Windows以外のオペレーティングシステムで使用する場合は、
>
>/etc/fd/watchfolder /config/PrintChannelDocumentに置き換え、環境設定に従ってfolderPathを変更します。

