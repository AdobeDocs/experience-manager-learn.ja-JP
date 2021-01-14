---
title: AEMヘッドレスの使用の手引き — GraphQL
description: AEM GraphQL APIを使用してコンテンツを構築し公開する方法を示す、エンドツーエンドのチュートリアルです。
sub-product: サイト
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6678
thumbnail: 328618.jpg
translation-type: tm+mt
source-git-commit: eb2b556c5947b15a31a74a86dadd525fb06bcf14
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---


# AEMヘッドレスの使用の手引き — GraphQL

>[!CAUTION]
>
> AEM GraphQL API for Content Fragments配信は、リクエストに応じて使用できます。
> お使いのAEM用のAPIをCloud Serviceプログラムとして有効にするには、Adobeサポートにお問い合わせください。

AEM GraphQL APIを使用してコンテンツを構築し、公開する方法、および外部アプリで使用されるヘッドレスCMSシナリオを例示する、エンドツーエンドのチュートリアルです。

このチュートリアルでは、AEM GraphQL APIとヘッドレス機能を使用して、外部アプリで表示されるエクスペリエンスをパワーにする方法を説明します。

このチュートリアルでは、次のトピックについて説明します。

* AEMで寄稿者をモデル化するためのコンテンツフラグメントモデルの作成
* 新しく作成されたコンテンツフラグメントモデルを使用した寄稿者コンテンツフラグメントの作成
* 統合GraphicQL開発ツールを使用して、AEMのコンテンツフラグメントをどのように照会できるかを調べます。
* サンプルのWKND GraphQL ReactアプリからAEM GraphQL APIを使用する
* フラグメント参照を使用した高度なデータモデリングの実行

## GraphQLの概要

次のビデオでは、AEMで実装されたGraphQL APIの概要を示します。 AEMのGraphQL APIは、主に、ヘッドレスデプロイメントの一環として、コンテンツのフラグメントデータをダウンストリームアプリケーションに配信するように設計されています。

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

## 始めよう！

「[クイックセットアップ](./setup.md)」の章にジャンプして、AEM GraphQLチュートリアルを開始します。

## GitHubプロジェクト

ソースコードとコンテンツパッケージは、[AEM Guides - WKND GraphQL GitHub Project](https://github.com/adobe/aem-guides-wknd-graphql)で入手できます。

チュートリアルまたはコードに問題がある場合は、[GitHubの問題](https://github.com/adobe/aem-guides-wknd-graphql/issues)を残してください。
