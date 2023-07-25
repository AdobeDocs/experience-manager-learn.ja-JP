---
title: サンプルアセットのデプロイ
description: サンプルアセットをローカルクラウド対応システムにデプロイします。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 7%

---

# サンプルアセットのデプロイ

このユースケースをシステムで動作させるには、ローカルクラウド対応システムに次のアセットをデプロイしてください。

* 必要な設定/アカウントがすべて、[概要ドキュメント](./introduction.md)

* [カスタムアダプティブフォームテンプレートと、関連するページコンポーネントをインストールします。](./assets/azure-portal-template-page-component.zip)

* [SendGrid フォームデータモデルのインストール](./assets/send-grid-form-data-model.zip). このフォームデータモデルが機能するには、アドレスから検証された API キーと SendGrid を提供する必要があります。 フォームデータモデルエディターでフォームデータモデルをテストする

* [Azure ベースのフォームデータモデルのインストール](./assets/azure-storage-fdm.zip). フォームデータモデルが機能するには、Azure ストレージアカウントの資格情報を指定する必要があります。 フォームデータモデルエディターでフォームデータモデルをテストします。

* [サンプルのアダプティブフォームを読み込む](./assets/credit-applications-af.zip)
* [クライアントライブラリを読み込みます。](./assets/client-lib.zip)
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/creditapplications/jcr:content?wcmmode=disabled). 有効な電子メールアドレスを入力し、「保存」ボタンをクリックします。 フォームデータは Azure ストレージに保存され、保存されたフォームへのリンクが記載された電子メールが、指定された電子メールアドレスに送信されます。


