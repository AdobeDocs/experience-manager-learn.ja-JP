---
title: SendGrid を使用して電子メールを送信
description: トリガー済みフォームへのリンクを含む電子メールを保存する
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 8474
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# 電子メールを送信

フォームのデータが Azure BLOB ストレージに保存されると、保存されたフォームへのリンクが記載された電子メールがユーザーに送信されます。 この電子メールは、SendGrid REST API を使用して送信されます。

電子メールの送信に必要な Swagger ファイル、フォームデータモデル、クラウドサービス設定が、記事アセットの一部として提供されます。

アダプティブフォームで取り込まれたデータを取り込むための動的テンプレートである SendGrid アカウントを作成する必要があります。


以下は、動的テンプレートで使用される html コードのスニペットです。 blobID パラメーターの値は、フォームデータモデル呼び出しサービスを使用して渡されます。

```html
You can  <a href='https://forms.enablementadobe.com/content/dam/formsanddocuments/azureportalstorage/creditcardapplication/jcr:content?wcmmode=disabled&ampguid={{blobID}}'>access your application here</a> and complete it.
```


