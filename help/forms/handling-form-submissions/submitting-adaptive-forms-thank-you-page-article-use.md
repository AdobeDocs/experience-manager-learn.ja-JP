---
title: 「ありがとうございます」ページへの送信
seo-title: Submitting To Thank You Page
description: アダプティブフォームの送信時に「ありがとうございます」ページを表示する
seo-description: Display a thank you page on submitting Adaptive Form
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 6%

---

# 「ありがとうございます」ページへの送信 {#submitting-to-thank-you-page}

「 REST エンドポイントに送信」オプションでは、HTTPGETリクエストの一環として、フォームに入力されたデータを設定済みの確認ページに渡します。 リクエストにフィールド名を追加できます。リクエストのフォーマットを以下に示します。

\{fieldName\} = \{parameterName\}。 例えば、 submitterName はアダプティブフォームフィールドの名前で、 submitter はパラメーターの名前です。 「ありがとうございます」ページでは、 request.getParameter(&quot;submitter&quot;) を使用して submitter パラメーターにアクセスし、submitter 名フィールドの値を取得できます。

`submitterName=submitter`

以下のスクリーンショットでは、/content/thankyou にある「ありがとうございます」ページにアダプティブフォームを送信しています。 この「ありがとうございます」ページには、フォームフィールドの値を保持する 3 つの要求属性を渡しています。

![「ありがとうございます」ページ](assets/thankyoupage.gif)

また、「 」を使用して外部エンドポイントに送信することもできます。POST これを達成するには、「post リクエストを有効にする」チェックボックスを選択して、外部エンドポイントの URL を指定するだけです。 フォームを送信すると、「ありがとうございます」ページが表示され、POSTエンドポイントが同時に呼び出されます。

![設定をキャプチャ](assets/capture.gif)

お使いのサーバーでこの機能をテストするには、次の手順に従ってください。

* 次をインポート： [パッケージマネージャーを使用して、この記事に関連付けられたアセットファイルをAEMに取り込みます。](assets/submittingtorestendpoint.zip)
* ブラウザーで [タイムオフリクエストフォーム](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 必須フィールドに入力し、フォームを送信します
* ページに情報が入力された「ありがとうございます」ページが表示されます
