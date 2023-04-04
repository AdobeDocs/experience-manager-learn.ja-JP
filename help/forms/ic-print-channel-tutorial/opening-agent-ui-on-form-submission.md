---
title: エージェント送信時にPOSTUI を開く
seo-title: Opening Agent UI On POST Submission
description: これは、印刷チャネル用の最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの第 11 部です。 ここでは、フォーム送信時にアドホック通信を作成するエージェント UI インターフェイスを起動します。
seo-description: This is part 11 of multistep tutorial for creating your first interactive communications document for the print channel. In this part, we will launch the agent ui interface for creating ad-hoc correspondence on form submission.
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 509b4d0d-9f3c-46cb-8ef7-07e831775086
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 2%

---

# エージェント送信時にPOSTUI を開く

ここでは、フォーム送信時にアドホック通信を作成するエージェント UI インターフェイスを起動します。

この記事では、フォームを送信する際に、エージェントの ui インターフェイスを開く手順について説明します。 一般的な使用例は、顧客サービスエージェントがフォームに入力パラメーターを入力し、フォーム送信エージェント ui で、フォームデータモデルの事前入力サービスから事前入力されたデータで開く場合です。

次のビデオでは、使用例を示します

>[!VIDEO](https://video.tv.adobe.com/v/40122?quality=12&learn=on)

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

行 1 :requestparameter からアカウント番号を取得する

行 2 ～ 8:パラメーター map を作成し、 documentId,Random を反映する適切なキーと値を設定します。

行 9-10:フォームデータモデルで定義された入力パラメータを格納する別の Map オブジェクトを作成します。

行 11:slingRequest 属性「paramMap」を設定します。

12～13 行目：リクエストをサーブレットに転送

ご使用のサーバーでこの機能をテストするには

* [パッケージマネージャーを使用して、この記事に関連するアセットを読み込んでインストールします。](assets/launch-agent-ui.zip)
* [configMgr にログイン](http://localhost:4502/system/console/configMgr)
* を検索 _AdobeGranite CSRF フィルタ_
* 追加 _/content/getprintchannel_ （除外されたパス内）
* 変更を保存します。
* [POST.jsp を開く](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). FormFieldRequestParameter に渡された文字列が有効な documentId であることを確認します。（19 行目）。
* [Web ページを開く](http://localhost:4502/content/OpenPrintChannel.html) 「 accountnumber 」と入力し、フォームを送信します。
* エージェント UI インターフェイスが開き、フォームに入力されたアカウント番号に応じたデータが事前に入力されます。

>[!NOTE]
>
>フォームデータモデルの「Get」操作の入力パラメーターが、これが機能するために、「accountnumber」と呼ばれる要求属性にバインドされていることを確認します。 binding の値の名前を他の名前に変更した場合は、変更がPOST.jsp の 25 行目に反映されていることを確認します。
