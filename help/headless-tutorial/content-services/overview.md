---
title: AEMヘッドレスの概要 — コンテンツサービス
description: AEM ヘッドレスを使用してコンテンツを構築および公開する方法を示す、エンドツーエンドのチュートリアルです。
feature: コンテンツフラグメント、API
topic: ヘッドレス、コンテンツ管理
role: Developer
level: Beginner
source-git-commit: 22829f532f7791af14919af24650b4593fe89ae8
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 4%

---


# AEMヘッドレスの概要 — コンテンツサービス

AEM Content Servicesは、従来のAEM Pagesを活用してヘッドレスなREST APIエンドポイントを構成し、これらのエンドポイントで公開するコンテンツをAEMコンポーネントが定義（参照）します。

AEM Content Servicesでは、AEM SitesでWebページのオーサリングに使用するのと同じコンテンツの抽象化を使用して、これらのHTTP APIのコンテンツとスキーマを定義できます。 AEMページとAEMコンポーネントを使用すると、マーケターはあらゆるアプリケーションを強化できる柔軟なJSON APIを迅速に作成および更新できます。

## コンテンツサービスチュートリアル

ヘッドレスなCMSシナリオで、AEMを使用してコンテンツを構築し、ネイティブモバイルアプリで利用する方法を示す、エンドツーエンドのチュートリアルです。

>[!VIDEO](https://video.tv.adobe.com/v/28315/?quality=12&learn=on)

このチュートリアルでは、AEM Content Servicesを使用して、イベント情報（音楽、パフォーマンス、アートなど）を表示するモバイルアプリのエクスペリエンスを強化する方法について説明します。 これはWKNDチームによってキュレーションされます。

このチュートリアルでは、次のトピックについて説明します。

* コンテンツフラグメントを使用したイベントを表すコンテンツの作成
* AEM Sitesのテンプレートとページを使用して、イベントデータをJSONとして公開するAEM Content Servicesエンドポイントを定義する
* AEM WCMコアコンポーネントを使用して、マーケターがJSONエンドポイントを作成できるようにする方法を確認する
* モバイルアプリからAEM Content Services JSONを使用する
   * Androidの使用は、このチュートリアルのすべてのユーザー（Windows、macOSおよびLinux）がネイティブアプリを実行する際に使用できるクロスプラットフォームエミュレーターがあるためです。

## GitHubプロジェクト

ソースコードとコンテンツパッケージは、[AEM Guides - WKND Mobile GitHub Project](https://github.com/adobe/aem-guides-wknd-mobile)で入手できます。

チュートリアルまたはコードに問題がある場合は、[GitHubの問題](https://github.com/adobe/aem-guides-wknd-mobile/issues)を残してください。

## AEM GraphQLとAEM Content Servicesの比較

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| スキーマ定義 | 構造化コンテンツフラグメントモデル | AEM Components |
| コンテンツ | コンテンツフラグメント | AEM Components |
| コンテンツ検出 | GraphQLクエリ別 | AEM Page |
| 配信形式 | GraphQL JSON | AEM ComponentExporter JSON |
