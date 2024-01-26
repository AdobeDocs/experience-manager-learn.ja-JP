---
title: AEM ヘッドレスの基本を学ぶ（実践チュートリアル）- GraphQL
description: AEM GraphQL API を使用してコンテンツを作成および公開する方法を説明するエンドツーエンドのチュートリアルです。
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
duration: 67
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 81%

---

# AEM ヘッドレスの基本を学ぶ - GraphQL

{{aem-headless-trials-promo}}

ヘッドレス CMS シナリオで AEM の GraphQL API を使用してコンテンツを作成および公開し、外部アプリで使用する方法を示すエンドツーエンドのチュートリアルです。

このチュートリアルでは、AEM の GraphQL API とヘッドレス機能を使用して、外部アプリで表示されるエクスペリエンスを強化する方法について説明します。

このチュートリアルでは、以下のトピックを扱います。

* プロジェクト設定の作成
* データをモデル化するコンテンツフラグメントモデルの作成
* 以前に作成したモデルに基づいて、コンテンツフラグメントを作成します。
* 統合 GraphiQL 開発ツールを使用して、AEM のコンテンツフラグメントをクエリする方法を調べます。
* GraphQL クエリを AEM に保存または永続化するには：
* サンプル React アプリからの永続 GraphQL クエリの使用

## 前提条件 {#prerequisites}

このチュートリアルに従うには、以下が必要です。

* HTML と JavaScript の基本的なスキル
* 以下のツールをローカルにインストールする必要があります。
   * [Node.js v18](https://nodejs.org/ja/)
   * [Git](https://git-scm.com/)
   * IDE（例：[Microsoft® Visual Studio Code](https://code.visualstudio.com/)）

### AEM 環境

このチュートリアルを完了するには、AEM管理者がAEM as a Cloud Service環境にアクセスできることをお勧めします。 AEMのas a Cloud Service環境にアクセスできない場合は、 [AEM Headless 体験版に登録する](https://commerce.adobe.com/business-trial/sign-up?items%5B0%5D%5Bid%5D=649A1AF5CBC5467A25E84F2561274821&amp;cli=headless_exl_banner_campaign&amp;co=US&amp;lang=ja) AEMヘッドレス機能の参照。

## それでは、始めましょう。

[コンテンツフラグメントモデルの定義](content-fragment-models.md)からチュートリアルを開始します。

## GitHub プロジェクト

ソースコードとコンテンツパッケージは、[AEM Guides - WKND GraphQL GitHub プロジェクト](https://github.com/adobe/aem-guides-wknd-graphql)で入手することができます。

チュートリアルまたはコードに問題が見つかった場合は、[GitHub イシュー](https://github.com/adobe/aem-guides-wknd-graphql/issues)を報告してください。
