---
title: AEMヘッドレスの概要 — コンテンツサービス
description: AEM ヘッドレスを使用してコンテンツを構築および公開する方法を示す、エンドツーエンドのチュートリアルです。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 5aa32791-861a-48e3-913c-36028373b788
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 4%

---

# AEMヘッドレスの概要 — コンテンツサービス

AEM Content Services は、従来のAEM Pages を活用してヘッドレス REST API エンドポイントを構成し、AEMコンポーネントは、これらのエンドポイントで公開するコンテンツを定義（参照）します。

AEM Content Services を使用すると、AEM Sitesで Web ページのオーサリングに使用するのと同じコンテンツの抽象概念を使用して、これらの HTTP API のコンテンツとスキーマを定義できます。 AEMページとAEMコンポーネントを使用すると、マーケターはあらゆるアプリケーションを強化できる柔軟な JSON API を迅速に作成および更新できます。

## コンテンツサービスチュートリアル

AEMを使用してコンテンツを構築し、ネイティブモバイルアプリで使用する方法を、ヘッドレス CMS シナリオで示すエンドツーエンドのチュートリアルです。

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

このチュートリアルでは、AEM Content Services を使用して、イベント情報（音楽、パフォーマンス、アートなど）を表示するモバイルアプリのエクスペリエンスを強化する方法について説明します。 これは WKND チームによってキュレーションされます。

このチュートリアルでは、次のトピックについて説明します。

* コンテンツフラグメントを使用してイベントを表すコンテンツを作成
* AEM Sites のテンプレートとページで、イベントデータを JSON として公開するAEM Content Services エンドポイントを定義する
* AEM WCM コアコンポーネントを使用して、マーケターが JSON エンドポイントを作成できるようにする方法を確認する
* モバイルアプリからAEM Content Services JSON を使用
   * Android の使用は、このチュートリアルのすべてのユーザー (Windows、macOS、Linux) がネイティブアプリを実行する際に使用できる、クロスプラットフォームエミュレーターがあるためです。

## GitHub プロジェクト

ソースコードとコンテンツパッケージは、 [AEMガイド — WKND Mobile GitHub プロジェクト](https://github.com/adobe/aem-guides-wknd-mobile).

チュートリアルまたはコードに問題がある場合は、 [GitHub の問題](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM GraphQLとAEM Content Services の比較

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| スキーマ定義 | 構造化コンテンツフラグメントモデル | AEM Components |
| コンテンツ | コンテンツフラグメント | AEM Components |
| コンテンツ検出 | GraphQLクエリ | AEM Page |
| 配信フォーマット | GraphQL JSON | AEM ComponentExporter JSON |
