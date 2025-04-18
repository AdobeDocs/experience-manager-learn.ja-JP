---
title: サンプルアセットのデプロイ
description: サンプルアセットをローカルクラウド対応のシステムにデプロイします。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-13717
exl-id: ae8104fa-7af2-49c2-9e6b-704152d49149
duration: 38
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '188'
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
