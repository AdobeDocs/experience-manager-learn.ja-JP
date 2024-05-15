---
title: 「ありがとうございます」ページへの送信
description: アダプティブフォーム送信時の「ありがとうございます」ページの表示
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
last-substantial-update: 2020-07-07T00:00:00Z
duration: 51
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 100%

---

# 「ありがとうございます」ページへの送信 {#submitting-to-thank-you-page}

「REST エンドポイントに送信」オプションでは、フォームに入力されたデータが HTTP GET リクエストの一環として設定済みの確認ページに渡されます。リクエストにフィールド名を追加できます。リクエストのフォーマットを以下に示します。

\{fieldName\} = \{parameterName\}例えば、submitterName はアダプティブフォームフィールドの名前で、submitter はパラメーターの名前です。「ありがとうございます」ページでは、request.getParameter(&quot;submitter&quot;) を使用して submitter パラメーターにアクセスし、送信者名フィールドの値を取得できます。

`submitterName=submitter`

次のスクリーンショットでは、/content/thankyou にある「ありがとうございます」ページにアダプティブフォームを送信しています。この「ありがとうございます」ページには、フォームフィールド値を保持する 3 つのリクエスト属性を渡しています。

![「ありがとうございます」ページ](assets/thankyoupage.gif)

また、POST を使用して外部エンドポイントに送信することもできます。それには、「POST リクエストを有効にする」チェックボックスを選択して、外部エンドポイントの URL を指定します。フォームを送信すると、「ありがとうございます」ページが表示され、POST エンドポイントが同時に呼び出されます。

![設定の取得](assets/capture.gif)

お使いのサーバーでこの機能をテストするには、次の手順に従ってください。

* [パッケージマネージャーを使用して、この記事に関連付けられているアセットファイルを AEM に](assets/submittingtorestendpoint.zip)読み込みます。
* ブラウザーで [Time Off Request Form](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled) にアクセスします。
* 必須フィールドに入力し、フォームを送信します。
* ページに情報が入力された「ありがとうございます」ページが表示されます。
