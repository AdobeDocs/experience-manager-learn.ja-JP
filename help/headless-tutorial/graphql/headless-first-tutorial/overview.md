---
title: AEMヘッドレス最初のチュートリアル
description: AEMヘッドレスファーストアプリケーションになる方法を説明します。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 4%

---


# AEMヘッドレス最初のチュートリアル

![AEMヘッドレスファーストチュートリアル](./assets/overview/overview.png)

このたびは、AEMヘッドレス API とGraphQLを駆使した、React を使用した Web エクスペリエンスの構築に関するチュートリアルをご利用いただき、誠にありがとうございます。 このチュートリアルでは、React、Adobe Experience Manager(AEM) ヘッドレス API、GraphQLの機能を組み合わせて、動的でインタラクティブな Web アプリケーションを作成するプロセスを順を追って説明します。

React は、シンプルさ、再利用性、コンポーネントベースのアーキテクチャで知られる、ユーザーインターフェイスを構築するための一般的な JavaScript ライブラリです。 AEMは、堅牢なコンテンツ管理機能を提供し、ヘッドレス API を提供します。開発者は、様々なチャネルやアプリケーションを通じて、AEMに保存されたコンテンツやデータにアクセスできます。

AEMヘッドレス API を活用することで、AEMインスタンスからコンテンツ、アセットおよびデータを取得し、それらを使用して React アプリケーションを強化できます。 API 用の柔軟なクエリ言語であるGraphQLを使用すると、AEMインスタンスから特定のデータを効率的かつ正確にリクエストし、React とAEMをシームレスに統合できます。

このチュートリアル全体で、GraphQLで React およびAEMヘッドレス API を使用して Web エクスペリエンスを構築する手順を順を追って説明します。 開発環境を設定し、React とAEM間の接続を確立し、GraphQLクエリを使用してコンテンツを取得し、Web アプリケーションで動的にレンダリングする方法について説明します。

ここでは、React プロジェクトの設定、AEMでの認証の確立、GraphQLを使用したAEMからのコンテンツのクエリ、React コンポーネントでのデータの処理、キャッシュとページネーションを使用したパフォーマンスの最適化などのトピックについて説明します。

このチュートリアルを最後までに、React、AEMヘッドレス API、GraphQLを活用して、強力で魅力的な Web エクスペリエンスを構築する方法について明確に理解できます。 次の Web アプリケーションの構築に取り組みましょう。

## 前提条件

### スキル

+ React の能力
+ GraphQL能力
+ AEM as a Cloud Serviceの基本知識

### AEM as a Cloud Service

このチュートリアルでは、AEMas a Cloud Service環境に管理者がアクセスできる必要があります。

### ソフトウェア

+ [Node.js v16 以降](https://nodejs.org/ja/)
   + を実行して、ノードのバージョンを確認します。 `node -v` コマンドラインから
+ [npm 6 以降](https://www.npmjs.com/)
   + を実行して npm のバージョンを確認する `npm -v` コマンドラインから
+ [Git](https://git-scm.com/)
   + を実行して Git のバージョンを確認する `git -v` コマンドラインから

用途 [ノードバージョンマネージャ (nvm)](https://github.com/nvm-sh/nvm) を使用して、同じマシンに複数のバージョンの node.js が存在することに対処できます。

コンピュータにソフトウェアをグローバルにインストールする権限を持っていることを確認します。

## 次の手順

環境が設定されたら、次の手順に進みます。 [AEM as a Cloud Serviceでのコンテンツのセットアップとオーサリング](./1-content-modeling.md)
