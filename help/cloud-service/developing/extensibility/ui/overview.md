---
title: AEM UI 拡張機能
description: App Builder を使用して拡張機能を作成する AEM UI 拡張機能について説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 73f5d90d-e007-41a0-9bb3-b8f36a9b1547
duration: 50
source-git-commit: 12d7f8f0afc1c19f289c847771cb9f4f965c650c
workflow-type: ht
source-wordcount: '275'
ht-degree: 100%

---

# AEM UI 拡張機能 {#aem-ui-extensibility}

Adobe Experience Manager（AEM）は、デジタルエクスペリエンスを作成するための強力なユーザーインターフェイス（UI）を備えています。UI をカスタマイズおよび拡張するために、アドビは App Builder を導入しました。このツールを使用すると、開発者は JavaScript と React を使用した複雑なコーディングを行わずにユーザーエクスペリエンスを向上できます。

App Builder は、AEM の拡張ポイントを適切に定義するためにバインドされた拡張機能を作成するための実装レイヤーを提供します。App Builder は、AEM とシームレスに統合され、リアルタイムのプレビューとテストを実行できます。AEM への変更のデプロイは、迅速かつ効率的です。App Builder を使用すると、開発者は時間と労力を節約し、迅速なプロトタイピングと関係者との共同作業が可能になります。

>[!CONTEXTUALHELP]
>id="aemcloud_learn_extensibility_app_builder"
>title="Adobe Developer App Builder と AEM ヘッドレスの基本を学ぶ"
>abstract="AEM App Builder を使用すると、開発者が JavaScript と React を使用して AEM UI を迅速にカスタマイズおよび拡張し、シームレスな統合と迅速なデプロイメントをサポートできる方法について説明します。"

## AEM UI 拡張機能の開発

AEM の様々な UI の拡張ポイントは異なりますが、基本概念は同じです。

以下にリンクされているビデオとウォークスルーでは、コンテンツフラグメントコンソール拡張機能の使用方法を紹介して、様々なアクティビティを説明します。ただし、ここで扱う概念はすべての AEM UI 拡張機能に適用できます。

1. [Adobe Developer Console プロジェクトの作成](./adobe-developer-console-project.md)
1. [拡張機能の初期化](./app-initialization.md)
1. [拡張機能の登録](./extension-registration.md)
1. 拡張ポイントの実装

   拡張ポイントとその実装は、拡張する UI に応じて異なります。

   + [コンテンツフラグメント UI 拡張機能の開発](./content-fragments/overview.md)

1. [モーダルの開発](./modal.md)
1. [Adobe I/O Runtime アクションの開発](./runtime-action.md)
1. [拡張機能の検証](./verify.md)
1. [拡張機能のデプロイ](./deploy.md)

## Adobe Developer ドキュメント

Adobe Developer には、AEM UI 拡張機能に関する開発者の詳細が含まれています。[技術的な詳細については、Adobe Developer コンテンツ](https://developer.adobe.com/uix/docs/)を参照してください。
