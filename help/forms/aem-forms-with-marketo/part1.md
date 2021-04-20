---
title: AEM Formsとマーケット（パート1）
seo-title: AEM Formsとマーケット（パート1）
description: AEM Formsフォームデータモデルを使用したAEM Formsとマーケティングツールの統合に関するチュートリアルです。
seo-description: AEM Formsフォームデータモデルを使用したAEM Formsとマーケティングツールの統合に関するチュートリアルです。
feature: アダプティブForms、フォームデータモデル
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: 開発
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 1%

---


# AEM Formsとマーケット

Marketoは、Adobeの一部で、電子メール、モバイル、ソーシャル、デジタル広告、Web管理、分析など、アカウントベースのマーケティングに重点を置いたMarketing Automationソフトウェアを提供しています。

Form Data Model(AEM Formsのフォームデータモデル)を使用して、AEM FormをMarketoとシームレスに統合できるようになりました。

[フォームデータモデルの詳細](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

マーケティング担当者はREST APIを公開して、システムの多くの機能をリモートで実行できます。 プログラムを作成して一括リードインポートを行うことから、Marketorインスタンスを細かく制御できるオプションが多数あります。 フォームデータモデルを使用すると、AEM FormsをMarketoと統合するのは非常に簡単です。

このチュートリアルでは、フォームデータモデルを使用して、AEM Formsとマーケティングツールを統合する手順について説明します。 チュートリアルを完了すると、Marketoに対するカスタム認証を行うOSGiバンドルが作成されます。 また、指定したSwaggerファイルを使用してデータソースを設定します。

開始する前に、「前提条件」の節に示す次のトピックをよく読むことをお勧めします。

## 前提条件

1. [パッケージにAEM Formsがインストールされ追加ているAEMサーバ](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. ローカルAEM開発環境
1. Form Data Modelに精通していること
1. Swaggerファイルに関する基本的な知識
1. アダプティブFormsの作成

**クライアントシークレットIDとクライアントシークレットキー**

MarketoとAEM Formsの統合の最初の手順は、APIを使用したREST呼び出しを行うのに必要なAPI秘密鍵証明書を取得することです。 以下が必要です

1. client_id
1. client_secret
1. identity_endpoint
1. 認証URL。

[上記のプロパティを取得するには、Marketorの公式ドキュメントに従ってください。](https://developers.marketo.com/rest-api/) または、Marketorインスタンスの管理者に問い合わせることもできます。

**開始する前に**

[この記事に関連するアセットをダウンロードして解凍します。](assets/aemformsandmarketo.zip) zipファイルには次の内容が含まれます。

1. BlankTemplatePackage.zip — これはアダプティブフォームのテンプレートです。 パッケージマネージャーを使用して読み込みます。
1. marketo.json — これは、データソースの設定に使用されるSwaggerファイルです。
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar — これは、カスタム認証を行うバンドルです。 チュートリアルを完了できない場合や、バンドルが期待どおりに機能しない場合は、自由に使用してください。
