---
title: 「ありがとうございます」ページへの送信
seo-title: 「ありがとうございます」ページへの送信
description: アダプティブフォーム送信時に「ありがとうございます」ページを表示する
seo-description: アダプティブフォーム送信時に「ありがとうございます」ページを表示する
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: アダプティブフォーム
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 6%

---


# ありがとうページ{#submitting-to-thank-you-page}への送信

「 RESTエンドポイントへの送信」オプションでは、フォームに入力されたGETを、HTTPデータリクエストの一環として設定済みの確認ページに渡します。 リクエストにフィールド名を追加できます。リクエストのフォーマットを以下に示します。

\{fieldName\} = \{parameterName\}。 例えば、 submitterNameはアダプティブフォームフィールドの名前で、 submitterはパラメーターの名前です。 ありがとうページで、 request.getParameter(&quot;submitter&quot;)を使用してsubmitterパラメーターにアクセスし、submitter名フィールドの値を取得できます。

submitterName=submitter

以下のスクリーンショットでは、/content/thankyouにある「ありがとうございます」ページにアダプティブフォームを送信します。 このありがとうページに、フォームフィールドの値を保持する3つの要求属性を渡しています。

![thank](assets/thankyoupage.gif)

また、「 」を介して外部エンドポイントに送信することもできます。POST これを実現するには、「postリクエストを有効にする」チェックボックスを選択して、外部エンドポイントのURLを指定するだけです。 フォームを送信すると、「ありがとうございます」ページが表示され、POSTエンドポイントが同時に呼び出されます。

![キャプチャ](assets/capture.gif)


ご使用のサーバーでこの機能をテストするには、次の手順に従ってください。

* パッケージマネージャー](assets/submittingtorestendpoint.zip)を使用して、この記事に関連付けられた[アセットファイルをAEMに読み込みます。
* ブラウザーで[リクエストフォームのタイムオフ](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)を参照します。
* 必須フィールドに入力し、フォームを送信します
* ページに情報が入力された「ありがとうございます」ページが表示されます

