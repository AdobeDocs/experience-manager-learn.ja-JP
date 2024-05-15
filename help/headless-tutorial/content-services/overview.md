---
title: AEM ヘッドレスの概要 - コンテンツサービス
description: AEM ヘッドレスを使用してコンテンツを構築および公開する方法を示す、エンドツーエンドのチュートリアルです。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 311
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 100%

---

# AEM ヘッドレスの概要 - コンテンツサービス

AEM のコンテンツサービスは、従来の AEM ページを活用してヘッドレス REST API エンドポイントを構成し、AEM コンポーネントはこれらのエンドポイントで公開するコンテンツを定義、参照します。

AEM コンテンツサービスでは、AEM Sitesで web ページの作成に使用するのと同じコンテンツ抽象化が可能なため、これらの HTTP API のコンテンツとスキーマを定義できます。 AEM ページと AEM コンポーネントを使用すると、マーケターはあらゆるアプリケーションを稼働できる柔軟な JSON API を迅速に作成および更新できます。

## コンテンツサービスのチュートリアル

AEM を使用してコンテンツを構築および表示し、ネイティブモバイルアプリで使用する方法を、ヘッドレス CMS シナリオで示すエンドツーエンドのチュートリアルです。

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

このチュートリアルでは、AEM コンテンツサービスを使用して、イベント情報（音楽、パフォーマンス、アートなど）を表示するモバイルアプリのエクスペリエンスを強化する方法について説明します。 これは WKND チームによってキュレーションされます。

このチュートリアルでは、次のトピックについて説明します。

* コンテンツフラグメントを使用してイベントを表すコンテンツを作成する
* AEM Sites のテンプレートとページで、イベントデータを JSON として公開する AEM コンテンツサービスエンドポイントを定義する
* AEM WCM コアコンポーネントを使用して、マーケターが JSON エンドポイントを作成できるようにする方法を確認する
* モバイルアプリから AEM コンテンツサービスの JSON を使用する
   * Android を使用する理由は、このチュートリアルのすべてのユーザー（Windows、macOS、Linux）がネイティブアプリを実行する際に使用できるクロスプラットフォームエミュレーターがあるためです。

## GitHub プロジェクト

ソースコードとコンテンツパッケージは、[AEM Guides - WKND モバイル GitHub プロジェクト](https://github.com/adobe/aem-guides-wknd-mobile)で利用可能です。

チュートリアルまたはコードに問題がある場合は、[GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues) で報告してください。

## AEM GraphQL と AEM Content Services の比較

|                                | AEM GraphQL API | AEM コンテンツサービス |
|--------------------------------|:-----------------|:---------------------|
| スキーマ定義 | 構造化コンテンツフラグメントモデル | AEM コンポーネント |
| コンテンツ | コンテンツフラグメント | AEM コンポーネント |
| コンテンツ検出 | GraphQL クエリによる | AEM ページによる |
| 配信フォーマット | GraphQL JSON | AEM ComponentExporter JSON |
