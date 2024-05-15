---
title: アダプティブフォーム送信時のメール送信
description: メール送信コンポーネントを使用して、アダプティブフォーム送信時に確認メールを送信します。
feature: Adaptive Forms
doc-type: article
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
duration: 40
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 100%

---

# アダプティブフォーム送信時のメール送信 {#sending-email-on-adaptive-form-submission}

アダプティブフォームが正常に送信されたことを送信者にメールで知らせることは、一般的によく行われます。これを実現するには、送信アクションとして「メールを送信」を選択します。

メールテンプレートを使用するか、メールの本文を以下のスクリーンショットのように入力します。

メールにフォームフィールドの値を挿入する構文に注意してください。また、設定プロパティで「添付ファイルを含める」チェックボックスをオンにすると、メールにフォームの添付ファイルを含めることもできます。

アダプティブフォームが送信されると、受信者にメールが届きます。

![SendEmail](assets/sendemailaction.gif)

## 必要な設定 {#configurations-needed}

Day CQ Mail サービスを設定する必要があります。この設定は、ブラウザーで [Felix Configuration Manager](http://localhost:4502/system/console/configMgr) にアクセスすることで行うことができます。

スクリーンショットには、アドビのメールサーバーの設定プロパティが表示されています。

![mailservice](assets/mailservice.png)

お使いのサーバーでこれを試すには、次の手順に従います。

* パッケージ マネージャーを使用して、この記事に関連付けられている[アセットを AEM に読み込み](assets/timeoffrequest.zip)ます。

* [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled) を開きます。

* 詳細を入力します。メールフィールドに有効なメールアドレスを入力してください。

* フォームを送信します。
