---
title: AEMヘッドレス実践チュートリアルの概要 — GraphQL
description: AEM GraphQL API を使用してコンテンツを構築および公開する方法を示す、エンドツーエンドのチュートリアルです。
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
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 6%

---

# AEM ヘッドレスの概要 - GraphQL

AEM GraphQL API を使用し、外部アプリで使用したコンテンツをヘッドレス CMS シナリオで構築および公開する方法を示す、エンドツーエンドのチュートリアルです。

このチュートリアルでは、AEM GraphQL API とヘッドレス機能を使用して、外部アプリで表示されるエクスペリエンスを強化する方法について説明します。

このチュートリアルでは、次のトピックについて説明します。

* プロジェクト設定を作成する
* コンテンツフラグメントモデルを作成してデータをモデル化する
* 以前に作成したモデルに基づいてコンテンツフラグメントを作成します。
* 統合 GraphiQL 開発ツールを使用して、AEMのコンテンツフラグメントを照会する方法を確認します。
* GraphQLクエリをAEMに保存または永続化するには
* サンプル React アプリからの永続的なGraphQLクエリの使用

## 前提条件 {#prerequisites}

このチュートリアルに従うには、次の操作が必要です。

* 基本的なHTMLと JavaScript のスキル
* 以下のツールをローカルにインストールする必要があります。
   * [Node.js v18](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * IDE( 例： [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

### AEM 環境

このチュートリアルを完了するには、 AEM as a Cloud Service環境へのAEM管理者アクセス権をお勧めします。 AEMas a Cloud Service環境にアクセスできない場合は、 [ローカルAEMas a Cloud ServiceQuickstart SDK](/help/cloud-service/local-development-environment/aem-runtime.md). ただし、コンテンツフラグメントのナビゲーションなど、一部の製品 UI 画面は異なることに注意する必要があります。

## さあ始めましょう！

チュートリアルを開始する前に [コンテンツフラグメントモデルの定義](content-fragment-models.md).

## GitHub プロジェクト

ソースコードとコンテンツパッケージは、 [AEMガイド — WKND GraphQL GitHub プロジェクト](https://github.com/adobe/aem-guides-wknd-graphql).

チュートリアルまたはコードに問題がある場合は、 [GitHub の問題](https://github.com/adobe/aem-guides-wknd-graphql/issues).
