---
title: フォーム送信時に送信 ID を表示
seo-title: Display submission id on form submission
description: 「ありがとうございます」ページにフォームデータモデルの送信の応答を表示します
seo-description: Display the response of an form data model submission in thank you page
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13900
last-substantial-update: 2023-09-09T00:00:00Z
source-git-commit: adfb805615d2abe34458d5aea685ae47517c5548
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---

# 「ありがとうございます」ページのカスタマイズ

アダプティブフォームを REST エンドポイントに送信する際に、フォームの送信が成功したことをユーザーに知らせる確認メッセージを表示する必要があります。 POSTの応答には、送信 ID などの送信に関する詳細が含まれ、適切に設計された確認メッセージには、より優れたユーザーエクスペリエンスに貢献する送信 ID が含まれます。 この応答は、アダプティブフォームで設定した「ありがとうございます」ページに表示できます。

次のスクリーンショットは、「ありがとうございました」ページが設定されたフォームデータモデル送信アクションを使用して、フォームが送信されている様子を示しています

![ありがとうページ](./assets/thank-you-page-fdm-submit.png)

フォームデータモデルのPOSTは、常に応答で JSON オブジェクトを返します。 この JSON は、「ありがとうございます」ページの URL に、という名前のクエリパラメーターとして表示されます。 _fdmSubmitResult_. このクエリパラメーターを解析し、「ありがとうございました」ページに JSON 要素を表示できます。
次のサンプルコードは、JSON 応答を解析して数値フィールドの値を抽出します。 次に、適切な xml が構築され、slingRequest に渡されて、フォームに入力されます。 このコードは、通常、アダプティブフォームテンプレートに関連付けられたページコンポーネントの jsp で記述されます。

```java
if(request.getParameter("fdmSubmitResult")!=null)
{
    String fdmSubmitResult =  request.getParameter("fdmSubmitResult");
    String status = request.getParameter("status");
    com.google.gson.JsonObject jsonObject = com.google.gson.JsonParser.parseString(fdmSubmitResult).getAsJsonObject();
    String caseNumber = jsonObject.get("result").getAsJsonObject().get("number").getAsString();
    slingRequest.setAttribute("data","<afData><afUnboundData><data><caseNumber>"+caseNumber+"</caseNumber><status>"+status+"</status></data></afUnboundData></afData>");
}
```

「ありがとうございます」ページを新しいアダプティブフォームテンプレートに基づいて作成することをお勧めします。このテンプレートでは、クエリパラメーターからの応答を抽出するカスタムコードを記述できます。

## ソリューションのテスト

アダプティブフォームを作成し、フォームデータモデルの送信アクションを使用してフォームを送信するよう設定します。
[サンプルのアダプティブフォームテンプレートをデプロイする](assets/thank-you-page-template.zip)
このテンプレートに基づいて「ありがとうございます」フォームを作成するこの「ありがとうございます」ページをメインフォームに関連付けます。 [createXml.jsp](http://localhost:4502/apps/thank-you-page-template/component/page/thankyoupage/createxml.jsp) アダプティブフォームの事前入力に必要な xml を作成するために使用します。
アダプティブフォームをプレビューして送信します。
「ありがとうございます」ページが表示され、XML で指定されたデータが事前入力されます



