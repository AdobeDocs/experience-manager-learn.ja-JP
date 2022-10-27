---
title: アダプティブフォーム送信時の電子メールの送信
seo-title: Sending Email on Adaptive Form Submission
description: 電子メールを送信コンポーネントを使用して、アダプティブフォーム送信時に確認電子メールを送信する
seo-description: Send confirmation email on adaptive form submission using the send email component
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: Adaptive Forms
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 3%

---

# アダプティブフォーム送信時の電子メールの送信 {#sending-email-on-adaptive-form-submission}

一般的なアクションの 1 つは、アダプティブフォームの送信が成功したことを知らせる電子メールを送信者に送信することです。 これを達成するには、送信アクションとして「メールを送信」を選択します。

電子メールテンプレートを使用するか、電子メールの本文を入力します（下のスクリーンショットを参照）。

電子メールにフォームフィールドの値を挿入する構文に注意してください。また、設定プロパティで「添付ファイルを含める」チェックボックスをオンにすると、電子メールにフォーム添付ファイルを含めるオプションもあります。

アダプティブフォームが送信されると、受信者に電子メールが送信されます。

![SendEmail](assets/sendemailaction.gif)

## 必要な設定 {#configurations-needed}

Day CQ Mail サービスを設定する必要があります。 これは、次を指すようにブラウザーを設定できます。 [Felix Configuration Manager](http://localhost:4502/system/console/configMgr)

スクリーンショットには、アドビのメールサーバーの設定プロパティが表示されます。

![mailservice](assets/mailservice.png)

サーバーでこれを試すには、次の手順に従います。

* [アセットの読み込み](assets/timeoffrequest.zip) パッケージマネージャーを使用してAEMでこの記事に関連付けられています。

* を開きます。 [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* 詳細を入力します。電子メールフィールドに有効な電子メールアドレスを入力してください。

* フォームを送信.
