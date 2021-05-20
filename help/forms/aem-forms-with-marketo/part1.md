---
title: AEM FormsとMarketo（パート1）
seo-title: AEM FormsとMarketo（パート1）
description: AEM Formsフォームデータモデルを使用したAEM FormsとMarketoの統合に関するチュートリアルです。
seo-description: AEM Formsフォームデータモデルを使用したAEM FormsとMarketoの統合に関するチュートリアルです。
feature: アダプティブForms、フォームデータモデル
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 1%

---


# AEM FormsとMarketo

Marketoは、Adobeの一部で、電子メール、モバイル、ソーシャル、デジタル広告、Web管理、分析など、アカウントベースのマーケティングに重点を置いたマーケティング自動化ソフトウェアを提供します。

AEM Formsのフォームデータモデルを使用すると、AEM FormsをMarketoとシームレスに統合できます。

[フォームデータモデルの詳細](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketoは、多くのシステム機能をリモートで実行できるREST APIを公開しています。 プログラムの作成から一括リード読み込みまで、Marketoインスタンスの詳細な制御を可能にする多くのオプションが用意されています。 フォームデータモデルを使用すると、AEM FormsとMarketoを統合するのは非常に簡単です。

このチュートリアルでは、フォームデータモデルを使用してAEM FormsとMarketoを統合する手順について説明します。 チュートリアルを完了すると、Marketoに対するカスタム認証をおこなうOSGiバンドルが作成されます。 また、提供されたSwaggerファイルを使用してデータソースを設定します。

最初に、前提条件の節に示す次のトピックに精通していることを強くお勧めします。

## 前提条件

1. [AEM FormsアドオンパッケージがインストールされたAEM server](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. ローカルAEM開発環境
1. フォームデータモデルについて
1. Swaggerファイルに関する基本的な知識
1. アダプティブFormsの作成

**クライアント秘密鍵IDとクライアント秘密鍵**

MarketoとAEM Formsを統合する最初の手順は、APIを使用してREST呼び出しをおこなうために必要なAPI資格情報を取得することです。 以下が必要です。

1. client_id
1. client_secret
1. identity_endpoint
1. 認証URL。

[上記のプロパティを取得するには、公式のMarketoドキュメントに従ってください。](https://developers.marketo.com/rest-api/) または、Marketoインスタンスの管理者に問い合わせることもできます。

**開始する前に**

[この記事に関連するアセットをダウンロードして解凍します。](assets/aemformsandmarketo.zip) zipファイルには次の内容が含まれます。

1. BlankTemplatePackage.zip — アダプティブフォームテンプレートです。 パッケージマネージャーを使用して読み込みます。
1. marketo.json — データソースの設定に使用するSwaggerファイル。
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar — カスタム認証をおこなうバンドルです。 チュートリアルを完了できない場合や、バンドルが期待どおりに動作しない場合は、これを自由に使用してください。
