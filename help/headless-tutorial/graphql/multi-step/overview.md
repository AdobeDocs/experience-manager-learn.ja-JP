---
title: AEMヘッドレス実践チュートリアルの概要 — GraphQL
description: AEM GraphQL APIを使用したコンテンツの構築と公開の方法を示す、エンドツーエンドのチュートリアルです。
sub-product: サイト
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6678
thumbnail: 328618.jpg
feature: コンテンツフラグメント、GraphQL API
topic: ヘッドレス、コンテンツ管理
role: Developer
level: Beginner
source-git-commit: 81626b8d853f3f43d9c51130acf02561f91536ac
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---


# AEMヘッドレスの概要 — GraphQL

ヘッドレスなCMSシナリオで、AEM GraphQL APIを使用してコンテンツを構築し、外部アプリで使用する方法を示す、エンドツーエンドのチュートリアルです。

このチュートリアルでは、AEM GraphQL APIとヘッドレス機能を使用して、外部アプリで表示されるエクスペリエンスを強化する方法を説明します。

このチュートリアルでは、次のトピックについて説明します。

* コンテンツフラグメントモデルを作成して、AEMでコントリビューターをモデル化する
* 新しく作成されたコンテンツフラグメントモデルを使用してコントリビューターコンテンツフラグメントを作成する
* 統合GraphiQL開発ツールを使用して、AEMのコンテンツフラグメントを照会する方法を確認します。
* サンプルWKND GraphQL ReactアプリからAEM GraphQL APIを使用する
* フラグメント参照を使用した高度なデータモデリングの実行

## 始めましょう！

AEM GraphQLチュートリアルを開始するには、[クイックセットアップ](./setup.md)の章を参照してください。

## GitHubプロジェクト

ソースコードとコンテンツパッケージは、[AEM Guides - WKND GraphQL GitHub Project](https://github.com/adobe/aem-guides-wknd-graphql)で入手できます。

チュートリアルまたはコードに問題がある場合は、[GitHubの問題](https://github.com/adobe/aem-guides-wknd-graphql/issues)を残してください。
