---
title: AEM ヘッドレスの基本を学ぶ（実践チュートリアル）- GraphQL
description: AEM GraphQL API を使用してコンテンツを作成および公開する方法を説明するエンドツーエンドのチュートリアルです。
doc-type: tutorial
mini-toc-levels: 1
kt: 6678
thumbnail: KT-6678.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
source-git-commit: 4c0770eafbbbb90bfc00ab49be02e84c41d63057
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 100%

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

このチュートリアルを完了するには、 AEM as a Cloud Service 環境への AEM 管理者アクセスをお勧めします。AEM as a Cloud Service 環境にアクセスできない場合は、[ローカル AEM as a Cloud Service クイックスタート SDK](/help/cloud-service/local-development-environment/aem-runtime.md) を使用することができます。ただし、コンテンツフラグメントのナビゲーションなど、一部の製品の UI 画面が異なることに注意する必要があります。

## それでは、始めましょう。

[コンテンツフラグメントモデルの定義](content-fragment-models.md)からチュートリアルを開始します。

## GitHub プロジェクト

ソースコードとコンテンツパッケージは、[AEM Guides - WKND GraphQL GitHub プロジェクト](https://github.com/adobe/aem-guides-wknd-graphql)で入手することができます。

チュートリアルまたはコードに問題が見つかった場合は、[GitHub イシュー](https://github.com/adobe/aem-guides-wknd-graphql/issues)を報告してください。
