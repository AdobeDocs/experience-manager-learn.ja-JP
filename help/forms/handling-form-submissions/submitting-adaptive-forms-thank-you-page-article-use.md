---
title: 「ありがとうございます」ページへの送信
seo-title: 「ありがとうございます」ページへの送信
description: アダプティブフォームの送信時に「ありがとうございます」ページを表示する
seo-description: アダプティブフォームの送信時に「ありがとうございます」ページを表示する
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: アダプティブフォーム
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: 開発
role: デベロッパー
level: 初心者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 7%

---


# ありがとうございますページへの送信{#submitting-to-thank-you-page}

「RESTエンドポイントへの送信」オプションでは、フォームに入力されたGETを、HTTPデータリクエストの一環として設定済みの確認ページに渡します。 フィールド名をリクエストに追加することができます。リクエストのフォーマットは次のとおりです。

\{fieldName\} = \{parameterName\}. 例えば、submitterNameはアダプティブフォームフィールドの名前、submitterはパラメータの名前です。 「ありがとうございます」ページでは、request.getParameter(&quot;submitter&quot;)を使用してsubmitterパラメーターにアクセスし、submitter nameフィールドの値を取得できます。

submitterName=submitter

以下のスクリーンショットでは、/content/thankyouにある「ありがとうございます」ページにアダプティブフォームを送信しています。 この「ありがとうございます」ページには、フォームフィールドの値を保持する3つのリクエスト属性を渡しています。

![thank](assets/thankyoupage.gif)

POSTを介して外部エンドポイントに送信することもできます。 そのためには、「ポストリクエストを有効にする」チェックボックスを選択し、外部エンドポイントのURLを指定するだけです。 フォームを送信すると、「ありがとうございます」ページが表示され、POSTエンドポイントが同時に呼び出されます。

![capture](assets/capture.gif)


ご使用のサーバーでこの機能をテストするには、次の手順に従ってください。

* パッケージマネージャー](assets/submittingtorestendpoint.zip)を使用して、この記事に関連付けられた[アセットファイルをAEMに読み込みます
* ブラウザーで[Time Off Request Form](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)を指定します。
* 必須フィールドに入力し、フォームを送信します
* ページに情報を入力した「ありがとうございます」ページが表示されます。

