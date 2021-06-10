---
title: ソリューションのテスト
description: フォームに添付ファイルを追加し、ワークフローをトリガーして電子メールを送信し、ソリューションをテストします。
sub-product: フォーム[ふぉーむ]
feature: ワークフロー
topics: adaptive forms
audience: developer
doc-type: article
activity: develop
version: 6.5
topic: 開発
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 9%

---


# ソリューションのテスト


* AEM Formsサーバーから電子メールを送信するように[Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service)を設定します
* [felix Webコンソール](http://localhost:4502/system/console/bundles)を使用して[formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar)バンドルをデプロイします。
* [SendFormAttachmentsViaEmailワークフローをデプロイします。](assets/zipped-form-attachments-model.zip) このワークフローでは、送信電子メールコンポーネントを使用して、カスタムプロセスステップによってpayloadフォルダーに保存されたzipped_attachments.zipファイルを送信します。必要に応じて、送信者と受信者のEメールアドレスを設定します。
* [サンプルフォーム](assets/zip-form-attachments-form.zip)を[FormsとドキュメントのUI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)から読み込みます。
* [フォームをプ](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) レビューし、添付ファイルを2つ追加してフォームを送信します。
* ワークフローがトリガーされ、zipファイルを含む電子メール通知が送信されます。

