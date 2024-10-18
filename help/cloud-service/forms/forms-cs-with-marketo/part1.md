---
title: AEM Formsas a Cloud ServiceとMarketoの統合
description: AEM Forms フォームデータモデルを使用して AEM Forms と Marketo を統合する方法を説明します
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: c3145149-bfa4-4dcb-acde-c359e9348f99
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 94%

---

# AEM Forms と Marketo の統合

アドビの一部である Marketo は、メール、モバイル、ソーシャル、デジタル広告、web 管理、分析などのアカウントベースのマーケティングに重点を置いたマーケティング自動処理ソフトウェアを提供しています。

AEM Forms のフォームデータモデルを使用して、AEM Form を Marketo とシームレスに統合できるようになりました。

[フォームデータモデルの詳細情報](https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/install-configure-pdf-generator.html)

Marketo は、システムの機能の多くをリモートで実行できる REST API を公開しています。プログラムの作成からリードの一括読み込みまで、Marketo インスタンスの詳細な制御を可能にするオプションが多数用意されています。フォームデータモデルを使用すれば、AEM Forms を Marketo と統合するのは非常に簡単です。

このチュートリアルでは、フォームデータモデルを使用して、AEM Forms を Marketo と統合する手順について説明します。チュートリアルを完了すると、Marketo に対してカスタム認証を行う OSGi バンドルが作成されます。また、提供された Swagger ファイルを使用してデータソースを設定することもできます。

開始するには、前提条件の節にリストされている次のトピックをよく理解しておくことを強くお勧めします。

## 前提条件

1. AEM Formsas a Cloud Serviceインスタンスへのアクセス
1. フォームデータモデルに精通している
1. Swagger ファイルの基本知識
1. アダプティブフォームの作成

**クライアント秘密鍵 ID とクライアント秘密鍵**

Marketo と AEM Forms の統合の最初の手順は、API を使用して REST 呼び出しを行うために必要な API 資格情報を取得することです。以下が必要です

1. client_id
1. client_secret
1. identity_endpoint

[上記のプロパティを取得するには、公式の Marketo ドキュメントに従ってください。](https://developers.marketo.com/rest-api/) または、Marketo インスタンスの管理者に問い合わせることもできます。

**始める前に**

* [このチュートリアルに関連するアセットをダウンロードして展開します。](assets/marketo.zip)

zip ファイルには以下が含まれます。

1. marketo.json - データソースの設定に使用する Swagger ファイルです。
1. marketo.json のホストプロパティを必ず変更して、marketo インスタンスを指すようにします。

## 次の手順

[データソースの作成](./part2.md)
