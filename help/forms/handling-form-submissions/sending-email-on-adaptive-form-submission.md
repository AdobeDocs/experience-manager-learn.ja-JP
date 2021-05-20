---
title: アダプティブフォーム送信時の電子メールの送信
seo-title: アダプティブフォーム送信時の電子メールの送信
description: 電子メールの送信コンポーネントを使用して、アダプティブフォーム送信時に確認電子メールを送信する
seo-description: 電子メールの送信コンポーネントを使用して、アダプティブフォーム送信時に確認電子メールを送信する
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: アダプティブフォーム
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 3%

---


# アダプティブフォーム送信時の電子メールの送信{#sending-email-on-adaptive-form-submission}

一般的なアクションの1つは、アダプティブフォームの送信が成功したことを確認する電子メールを送信者に送信することです。 これを実現するには、送信アクションとして「Eメールを送信」を選択します。

Eメールテンプレートを使用するか、以下のスクリーンショットに示すように、Eメールの本文を入力します。

電子メールにフォームフィールドの値を挿入する構文に注意してください。また、設定プロパティの「添付ファイルを含める」チェックボックスをオンにすると、電子メールにフォーム添付ファイルを含めるオプションもあります。

アダプティブフォームが送信されると、受信者に電子メールが送信されます。

![SendEmail](assets/sendemailaction.gif)

## 必要な構成{#configurations-needed}

Day CQ Mailサービスを設定する必要があります。 これは、[Felix Configuration Manager](http://localhost:4502/system/console/configMgr)を指すことで設定できます

このスクリーンショットは、アドビのメールサーバーの設定プロパティを示しています。

![mailservice](assets/mailservice.png)

サーバーでこれを試すには、次の手順に従います。

* [パッケージマネ](assets/timeoffrequest.zip) ージャーを使用して、この記事に関連付けられたアセットをAEMにインポートします。

* [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)を開きます。

* 詳細を入力します。電子メールフィールドに有効な電子メールアドレスを入力してください。

* フォームを送信します。
