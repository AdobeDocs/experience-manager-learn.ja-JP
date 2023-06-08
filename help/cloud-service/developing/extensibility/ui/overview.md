---
title: AEM UI 拡張機能
description: 拡張機能を作成するための App Builder を使用したAEM UI の拡張機能について説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
thumbnail: null
last-substantial-update: 2023-06-02T00:00:00Z
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 2%

---


# AEM UI 拡張機能

Adobe Experience Manager(AEM) は、デジタルエクスペリエンスを作成するための強力なユーザーインターフェイス (UI) を備えています。 UI をカスタマイズおよび拡張するために、Adobeで App Builder が導入されました。 このツールを使用すると、JavaScript や React を使用した複雑なコーディングをおこなわずに、開発者がユーザーエクスペリエンスを強化できます。

App Builder は、AEMの拡張ポイントを適切に定義するためにバインドされた拡張機能を作成するための実装レイヤーを提供します。 App Builder はAEMとシームレスに統合され、リアルタイムでプレビューとテストを実行できます。 変更のAEMへのデプロイは、迅速かつ効率的です。 App Builder を使用すると、開発者は時間と労力を節約でき、関係者との迅速なプロトタイピングとコラボレーションを可能にします。

## AEM UI 拡張機能の開発

AEMの様々な UI の拡張ポイントは異なりますが、基本概念は同じです。

以下にリンクされているビデオおよびウォークスルーでは、様々なアクティビティを説明するためのコンテンツフラグメントコンソール拡張機能の使用方法を紹介しています。 ただし、扱う概念はすべてのAEM UI 拡張機能に適用できることに注意してください。

1. [Adobe Developer Console プロジェクトの作成](./adobe-developer-console-project.md)
1. [拡張機能の初期化](./app-initialization.md)
1. [拡張機能の登録](./extension-registration.md)
1. 拡張ポイントの実装

   拡張ポイントとその実装は、拡張される UI に応じて異なります。

   + [コンテンツフラグメント UI 拡張機能の開発](./content-fragments/overview.md)

1. [モーダルの作成](./modal.md)
1. [Adobe I/O Runtimeアクションの作成](./runtime-action.md)
1. [拡張機能の検証](./verify.md)
1. [拡張機能のデプロイ](./deploy.md)

## Adobe Developerドキュメント

Adobe Developerには、AEM UI の拡張機能に関する開発者の詳細が含まれています。 次を確認してください： [Adobe Developerの詳細な技術的詳細のコンテンツ](https://developer.adobe.com/uix/docs/).
