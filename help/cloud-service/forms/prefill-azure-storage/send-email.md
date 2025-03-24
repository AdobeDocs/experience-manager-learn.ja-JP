---
title: SendGrid を使用したメールの送信
description: 保存されたフォームへのリンクを含んだメールをトリガーします
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-8474
duration: 25
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 100%

---

# メールの送信

フォームのデータが Azure Blob Storage に保存されると、保存されたフォームへのリンクが記載されたメールがユーザーに送信されます。このメールは、SendGrid REST API を使用して送信されます。

メールの送信に必要な Swagger ファイル、フォームデータモデルおよびクラウドサービス設定が、記事のアセットの一部として用意されています。

アダプティブフォームでキャプチャされたデータを取り込むための動的テンプレートである SendGrid アカウントを作成する必要があります。


動的テンプレートで使用される HTML コードのスニペットを次に示します。blobID パラメーターの値は、フォームデータモデルサービスの呼び出しを使用して渡されます。

```html
You can  <a href='https://forms.enablementadobe.com/content/dam/formsanddocuments/azureportalstorage/creditcardapplication/jcr:content?wcmmode=disabled&ampguid={{blobID}}'>access your application here</a> and complete it.
```


