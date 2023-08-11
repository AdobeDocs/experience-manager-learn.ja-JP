---
title: サンプルアセットのデプロイ
description: サンプルアセットをローカルクラウド対応のシステムにデプロイします。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: ht
source-wordcount: '198'
ht-degree: 100%

---

# サンプルアセットのデプロイ

このユースケースをシステムで動作させるには、ローカルクラウド対応のシステムに次のアセットをデプロイしてください。

* [概要ドキュメント](./introduction.md)に記載されている必要な設定／アカウントがすべて作成済みであることを確認してください。

* [カスタムアダプティブフォームテンプレートと、関連するページコンポーネントをインストールします。](./assets/azure-portal-template-page-component.zip)

* [SendGrid フォームデータモデルをインストールします](./assets/send-grid-form-data-model.zip)。このフォームデータモデルが機能するには、API キーと SendGrid の検証元アドレスを指定する必要があります。フォームデータモデルエディターでフォームデータモデルをテストします。

* [Azure でサポートされているフォームデータモデルをインストールします](./assets/azure-storage-fdm.zip)。このフォームデータモデルが機能するには、Azure ストレージアカウントの資格情報を指定する必要があります。フォームデータモデルエディターでフォームデータモデルをテストします。

* [サンプルアダプティブフォームを読み込みます。](./assets/credit-applications-af.zip)
* [クライアントライブラリを読み込みます。](./assets/client-lib.zip)
* [フォームをプレビューします](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/creditapplications/jcr:content?wcmmode=disabled)。有効なメールアドレスを入力し、「保存」ボタンをクリックします。フォームデータが Azure ストレージに保存され、保存されたフォームへのリンクが記載されたメールが、指定したメールアドレスに送信されます。


