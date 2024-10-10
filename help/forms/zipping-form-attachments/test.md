---
title: ソリューションのテスト - 2 つのアプローチをテストする際に必要な手順
description: フォームに添付ファイルを追加し、ワークフローをトリガーしてメールを送信することで、ソリューションをテストします。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '216'
ht-degree: 100%

---

# 2 つのアプローチをテストするために必要な手順

* [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=ja#configuring-the-mail-service) を設定して、AEM Forms サーバーからメールを送信します。
*   [Felix web コンソールを使用して、](http://localhost:4502/system/console/bundles)[formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) バンドルをデプロイします。

## Zip ファイルをメールの添付ファイルとして送信



* [SendFormAttachmentsViaEmail ワークフローをデプロイします。 ](assets/zipped-form-attachments-model.zip) このワークフローでは、送信メールコンポーネントを使用して、カスタムプロセスステップでペイロードフォルダーに保存された zipped_attachments.zip ファイルを送信します。 必要に応じて、送信者と受信者のメールアドレスを設定します。
* 「[Forms とドキュメント UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)」から[サンプルフォーム](assets/zip-form-attachments-form.zip)を読み込みます。
* [フォームをプレビュー](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled)し、添付ファイルを 2 つ追加してフォームを送信します。
* ワークフローがトリガーされ、zip ファイルを含むメール通知が送信されます。

## 個別のファイルとしての添付ファイルの送信

* [SendForm ワークフローをデプロイします。](assets/send-form-attachments-model.zip) このワークフローでは、送信メールをコンポーネントを使用して、フォームの添付ファイルを個々のファイルとして送信します。 必要に応じて、送信者と受信者のメールアドレスを設定します。
* 「[Forms とドキュメント UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)」から[サンプルフォーム](assets/send-list-attachments-form.zip)を読み込みます。
* [フォームをプレビュー](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled)し、添付ファイルを 2 つ追加してフォームを送信します。
* ワークフローがトリガーされ、フォームの添付ファイルを含むメール通知が送信されます。
