---
title: AEM ヘッドレスの最初のチュートリアル
description: AEM ヘッドレスの最初のアプリケーションにする方法を説明します。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: b0ac4b50-5fe5-41a1-9530-8e593d7000c9
duration: 89
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 100%

---

# AEM ヘッドレスの最初のチュートリアル

{{aem-headless-trials-promo}}

このチュートリアルでは、AEM ヘッドレス API と GraphQL を完全に活用した React を使用して web エクスペリエンスを作成する方法について説明します。このチュートリアルでは、React、Adobe Experience Manager（AEM）ヘッドレス API、GraphQL の機能を組み合わせて、動的でインタラクティブな web アプリケーションを作成するプロセスを説明します。

React は、ユーザーインターフェイスを作成するための一般的な JavaScript ライブラリであり、そのシンプルさ、再利用性、コンポーネントベースのアーキテクチャで知られています。AEM は、堅牢なコンテンツ管理機能を提供し、開発者が様々なチャネルやアプリケーションを通じて AEM に保存されているコンテンツやデータへのアクセスを可能にするヘッドレス API を公開します。

AEM のヘッドレス API を活用すると、AEM インスタンスからコンテンツ、アセット、データを取得し、これらを使用して React アプリケーションを強化できます。API 用の柔軟なクエリ言語である GraphQL は、AEM インスタンスから特定のデータをリクエストする効率的かつ正確な方法を提供し、React と AEM 間のシームレスな統合を可能にします。

![AEM ヘッドレスの最初のチュートリアル](./assets/overview/overview.png)

このチュートリアルでは、GraphQL を使用した React および AEM のヘッドレス API を使用して web エクスペリエンスを作成するプロセスを段階的に説明します。開発環境を設定し、React と AEM 間の接続を確立し、GraphQL クエリを使用してコンテンツを取得し、web アプリケーションで動的にレンダリングする方法を学びます。

ここでは、React プロジェクトの設定、AEM による認証の確立、GraphQL を使用した AEM からのコンテンツのクエリ、React コンポーネント内のデータの処理、キャッシュとページネーションを利用したパフォーマンスの最適化などのトピックについて説明します。

このチュートリアルを終了すると、React、AEM のヘッドレス API、GraphQL を活用して強力で魅力的な web エクスペリエンスを作成する方法について明確な理解が得られます。次の web アプリケーションの作成に取り組みましょう。

## 前提条件

### スキル

+ React に精通している
+ GraphQL に精通している
+ AEM as a Cloud Service の基本知識

### AEM as a Cloud Service

このチュートリアルでは、AEM as a Cloud Service 環境への管理者アクセス権が必要です。

### ソフトウェア

+ [Node.js v16 以降](https://nodejs.org/ja/)
   + コマンドラインから `node -v` を実行して、Node のバージョンを確認します。
+ [npm 6 以降](https://www.npmjs.com/)
   + コマンドラインから `npm -v` を実行して、npm のバージョンを確認します。
+ [Git](https://git-scm.com/)
   + コマンドラインから `git -v` を実行して、Git のバージョンを確認します。

[ノードバージョンマネージャー（nvm）](https://github.com/nvm-sh/nvm)を使用して、同じマシン上に複数のバージョンの Node.js を存在させるようにします。

コンピューターにソフトウェアをグローバルにインストールする権限があることを確認します。

## 次の手順

環境が設定されたら、[AEM as a Cloud Service でのコンテンツの設定と作成](./1-content-modeling.md)の手順に進みます。
