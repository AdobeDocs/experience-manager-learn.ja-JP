---
title: SendGrid を使用したメールの送信
description: 保存されたフォームへのリンクを含んだメールをトリガーします
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 8474
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: ht
source-wordcount: '119'
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


