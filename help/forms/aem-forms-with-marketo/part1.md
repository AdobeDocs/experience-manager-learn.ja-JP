---
title: AEM Forms と Marketo の統合
description: AEM Forms フォームデータモデルを使用して AEM Forms と Marketo を統合する方法を説明します
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 77
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 100%

---

# AEM Forms と Marketo の統合

アドビの一部である Marketo は、メール、モバイル、ソーシャル、デジタル広告、web 管理、分析などのアカウントベースのマーケティングに重点を置いたマーケティング自動処理ソフトウェアを提供しています。

AEM Forms のフォームデータモデルを使用して、AEM Form を Marketo とシームレスに統合できるようになりました。

[フォームデータモデルの詳細情報](https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/install-configure-pdf-generator.html)

Marketo は、システムの機能の多くをリモートで実行できる REST API を公開しています。プログラムの作成からリードの一括読み込みまで、Marketo インスタンスの詳細な制御を可能にするオプションが多数用意されています。フォームデータモデルを使用すれば、AEM Forms を Marketo と統合するのは非常に簡単です。

このチュートリアルでは、フォームデータモデルを使用して、AEM Forms を Marketo と統合する手順について説明します。チュートリアルを完了すると、Marketo に対してカスタム認証を行う OSGi バンドルが作成されます。また、提供された Swagger ファイルを使用してデータソースを設定することもできます。

開始するには、前提条件の節にリストされている次のトピックをよく理解しておくことを強くお勧めします。

## 前提条件

1. [AEM Forms アドオンパッケージがインストールされた AEM サーバー](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. ローカル AEM 開発環境
1. フォームデータモデルに精通している
1. Swagger ファイルの基本知識
1. アダプティブフォームの作成

**クライアント秘密鍵 ID とクライアント秘密鍵**

Marketo と AEM Forms の統合の最初の手順は、API を使用して REST 呼び出しを行うために必要な API 資格情報を取得することです。以下が必要です

1. client_id
1. client_secret
1. identity_endpoint
1. 認証 URL

[上記のプロパティを取得するには、公式の Marketo ドキュメントに従ってください。](https://developers.marketo.com/rest-api/) または、Marketo インスタンスの管理者に問い合わせることもできます。

**始める前に**

[この記事に関連するアセットをダウンロードして展開します。](assets/aemformsandmarketo.zip)zip ファイルには以下が含まれます。

1. BlankTemplatePackage.zip - アダプティブフォームテンプレートです。パッケージマネージャーを使用して読み込みます。
1. marketo.json - データソースの設定に使用する Swagger ファイルです。
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar - カスタム認証を実行するバンドルです。チュートリアルを完了できない場合や、バンドルが期待どおりに動作しない場合に、自由に使用できます。

## 次の手順

[カスタム認証の作成](./part2.md)
