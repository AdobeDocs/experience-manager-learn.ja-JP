---
title: AEM Forms と Marketo（第 1 部）
description: AEM Forms フォームデータモデルを使用した AEM Forms と Marketo の統合に関するチュートリアル
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 9%

---

# AEM Forms と Marketo

Marketoは、Adobeの一部で、電子メール、モバイル、ソーシャル、デジタル広告、Web 管理、分析など、アカウントベースのマーケティングに重点を置いたマーケティングオートメーションソフトウェアを提供します。

AEM Formsのフォームデータモデルを使用すると、AEM Forms をMarketoとシームレスに統合できます。

[フォームデータモデルの詳細](https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/install-configure-pdf-generator.html)

Marketoは、多くのシステム機能をリモートで実行できる REST API を公開しています。 プログラムの作成からリードの一括読み込みまで、Marketoインスタンスの詳細な制御を可能にする多くのオプションが用意されています。 フォームデータモデルを使用すると、AEM FormsとMarketoを統合するのが非常に簡単です。

このチュートリアルでは、フォームデータモデルを使用したAEM FormsとMarketoの統合に関する手順について説明します。 チュートリアルを完了すると、Marketoに対するカスタム認証を実行する OSGi バンドルが用意されます。 また、提供された Swagger ファイルを使用してデータソースを設定します。

最初に、前提条件の節に示す以下のトピックに精通していることを強くお勧めします。

## 前提条件

1. [AEM FormsアドオンパッケージがインストールされたAEM server](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. ローカルAEM開発環境
1. フォームデータモデルに精通している
1. Swagger ファイルに関する基本的な知識
1. アダプティブFormsの作成

**クライアント秘密鍵 ID とクライアント秘密鍵**

MarketoとAEM Formsを統合する最初の手順は、API を使用して REST 呼び出しをおこなうために必要な API 資格情報を取得することです。 以下が必要です

1. client_id
1. client_secret
1. identity_endpoint
1. 認証 URL

[上記のプロパティを取得するには、公式のMarketoドキュメントに従ってください。](https://developers.marketo.com/rest-api/) または、Marketoインスタンスの管理者に問い合わせることもできます。

**始める前に**

[この記事に関連するアセットをダウンロードして展開します。](assets/aemformsandmarketo.zip) zip ファイルには次の内容が含まれます。

1. BlankTemplatePackage.zip — アダプティブフォームテンプレートです。 パッケージマネージャーを使用して読み込みます。
1. marketo.json — データソースの設定に使用する Swagger ファイル。
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar — これはカスタム認証を実行するバンドルです。 チュートリアルを完了できない場合や、バンドルが期待どおりに動作しない場合は、これを自由に使用できます。

## 次の手順

[カスタム認証の作成](./part2.md)
