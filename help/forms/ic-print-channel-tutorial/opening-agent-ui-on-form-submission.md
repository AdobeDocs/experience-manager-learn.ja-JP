---
title: エージェント送信時にPOSTUIを開く
seo-title: エージェント送信時にPOSTUIを開く
description: これは、印刷チャネル用の最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの第11部です。 ここでは、フォーム送信時にアドホック通信を作成するエージェントuiインターフェイスを起動します。
seo-description: これは、印刷チャネル用の最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの第11部です。 ここでは、フォーム送信時にアドホック通信を作成するエージェントuiインターフェイスを起動します。
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122.jpg
topic: 開発
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 2%

---


# エージェント送信時にPOSTUIを開く

ここでは、フォーム送信時にアドホック通信を作成するエージェントuiインターフェイスを起動します。

この記事では、フォームを送信する際にエージェントuiインターフェイスを開く手順について説明します。 一般的な使用例は、顧客サービスエージェントがフォームに入力パラメーターを入力し、フォーム送信エージェントuiを開くときに、フォームデータモデルの事前入力サービスから事前入力されたデータが表示される場合です。

次のビデオでは、使用例を示します

>[!VIDEO](https://video.tv.adobe.com/v/40122/?quality=9&learn=on)

```java
String accountNumber = request.getParameter("accountnumber"))
ParameterMap parameterMap = new ParameterMap();
RequestParameter icLetterId[] = new RequestParameter[1];
icLetterId[0] = new FormFieldRequestParameter("/content/dam/formsanddocuments/retirementstatementprint");
parameterMap.put("documentId", icLetterId);
RequestParameter Random[] = new RequestParameter[1];
Random[0] = new FormFieldRequestParameter("209457");
parameterMap.put("Random", Random);
Map map = new HashMap();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,parameterMap,"GET");
wrapperRequest.getRequestDispatcher("/aem/forms/createcorrespondence.html").include(wrapperRequest, response);
```

1行目：requestparameterからアカウント番号を取得する

2～8行目：パラメーターマップを作成し、 documentId,Randomを反映する適切なキーと値を設定します。

9～10行目：別のMapオブジェクトを作成し、フォームデータモデルで定義された入力パラメータを保持します。

11行目：slingRequest属性「paramMap」を設定します。

12～13行目：要求をサーブレットに転送

この機能をサーバーでテストするには

* [パッケージマネージャーを使用して、この記事に関連するアセットを読み込み、インストールします。](assets/launch-agent-ui.zip)
* [configMgrにログインします。](http://localhost:4502/system/console/configMgr)
* _AdobeGranite CSRF Filter_&#x200B;を検索します。
* _/content/getprintchannel_&#x200B;をExcluded Pathsに追加します。
* 変更を保存します。
* [POST.jspを開きます](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp)。FormFieldRequestParameterに渡される文字列が有効なdocumentIdであることを確認します。（19行目）。
* [Webページを開](http://localhost:4502/content/OpenPrintChannel.html) き、 accountnumberと入力してフォームを送信します。
* エージェントUIインターフェイスが開き、フォームに入力されたアカウント番号に固有のデータが事前に設定されます。

>[!NOTE]
>
>フォームデータモデルのGet操作の入力パラメーターが、「accountnumber」と呼ばれる要求属性にバインドされていることを確認して、これが機能するようにします。 binding値の名前を他の名前に変更した場合は、その変更がPOST.jspの25行目に反映されていることを確認します。

