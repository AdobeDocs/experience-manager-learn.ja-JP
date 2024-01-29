---
title: POST 送信時にエージェント UI を開く
description: これは、印刷チャネル用の最初のインタラクティブなコミュニケーションドキュメントを作成するためのマルチステップチュートリアルの第 11 部です。 ここでは、フォーム送信時にアドホック通信を作成するエージェント UI インターフェイスを起動します。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
jira: KT-6168
thumbnail: 40122.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 509b4d0d-9f3c-46cb-8ef7-07e831775086
duration: 183
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '322'
ht-degree: 100%

---

# POST 送信時にエージェント UI を開く

ここでは、フォーム送信時にアドホック通信を作成するエージェント UI インターフェイスを起動します。

この記事では、フォームを送信する際に、エージェント UI インターフェイスを開く手順について説明します。 一般的な使用例は、カスタマーサービスエージェントがフォームに入力パラメーターを入力し、フォーム送信時に、フォームデータモデル事前入力サービスから事前入力されたデータでエージェント UI が開かれることです。フォームデータモデル事前入力サービスへの入力パラメーターは、フォーム送信から抽出されます。

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

行 1：requestparameter から accountnumber を取得しします。

行 2～8：パラメーターマップを作成し、適切なキーと値を設定して documentId、Random を反映させます。

行 9～10：フォームデータモデルで定義された入力パラメーターを格納する別の Map オブジェクトを作成します。

行 11：slingRequest 属性「paramMap」を設定します。

行 12～13：リクエストをサーブレットに転送します。

ご使用のサーバーでこの機能をテストするには、次を行います。

* [パッケージマネージャーを使用して、この記事に関連するアセットを読み込んでインストールします。](assets/launch-agent-ui.zip)
* [configMgr にログイン](http://localhost:4502/system/console/configMgr)
* _Adobe Granite CSRF フィルター_&#x200B;を検索します。
* 除外パスに _/content/getprintchannel_ を追加します
* 変更を保存します。
* [POST.jsp を開きます](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp)。FormFieldRequestParameter に渡された文字列が有効な documentId であることを確認します。（行 19）。
* [Web ページを開いて](http://localhost:4502/content/OpenPrintChannel.html) 「 accountnumber 」と入力し、フォームを送信します。
* エージェント UI インターフェイスが開き、フォームに入力されたアカウント番号に応じたデータが事前に入力されます。

>[!NOTE]
>
>フォームデータモデルの Get 操作の入力パラメーターが、これが機能するために、「accountnumber」と呼ばれるリクエスト属性にバインドされていることを確認します。 bindingvalue の名前を他の名前に変更する場合は、その変更が POST.jsp の 25 行目に反映されていることを確認してください。
