---
title: ソリューションのテスト — 2 つのアプローチのテストに必要な手順
description: フォームに添付ファイルを追加し、ワークフローをトリガーして電子メールを送信し、ソリューションをテストします。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 7%

---

# 2 つのアプローチをテストするために必要な手順

* 設定 [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) AEM Formsサーバーから電子メールを送信するには
* をデプロイします。 [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) ～を用いたバンドル [felix web コンソール](http://localhost:4502/system/console/bundles)

## Zip ファイルを電子メールの添付ファイルとして送信



* をデプロイします。 [SendFormAttachmentsViaEmail ワークフロー。](assets/zipped-form-attachments-model.zip) このワークフローでは、電子メールの送信コンポーネントを使用して、カスタムプロセスステップで payload フォルダーに保存された zipped_attachments.zip ファイルを送信します。 必要に応じて、送信者と受信者の E メールアドレスを設定します。
* 次をインポート： [サンプルフォーム](assets/zip-form-attachments-form.zip) から [Formsとドキュメント UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) 添付ファイルを 2 つ追加し、フォームを送信します。
* ワークフローがトリガーされ、zip ファイルを含む電子メール通知が送信されます。

## フォームの添付ファイルを個々のファイルとして送信する

* をデプロイします。 [SendForm ワークフロー。](assets/send-form-attachments-model.zip) このワークフローでは、「電子メールを送信」コンポーネントを使用して、フォームの添付ファイルを個々のファイルとして送信します。 必要に応じて、送信者と受信者の E メールアドレスを設定します。
* 次をインポート： [サンプルフォーム](assets/send-list-attachments-form.zip) から [Formsとドキュメント UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) 添付ファイルを 2 つ追加し、フォームを送信します。
* ワークフローがトリガーされ、フォームの添付ファイルを含む電子メール通知が送信されます。
