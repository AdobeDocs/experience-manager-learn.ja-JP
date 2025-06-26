---
title: AEM Sites の基本を学ぶ - WKND チュートリアル
description: WKND という架空のライフスタイルブランド向けに AEM Sites を実装する方法を説明します。プロジェクトの設定、Maven アーキタイプ、コアコンポーネント、編集可能なテンプレート、クライアントライブラリ、コンポーネント開発など、Experience Manager の基本的なトピックについて順を追って説明します。
version: Experience Manager as a Cloud Service
jira: KT-13565
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
doc-type: Catalog
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: dbf63f30ccfd06e4f4d7883c2f7bc4ac78245364
workflow-type: ht
source-wordcount: '577'
ht-degree: 100%

---

# AEM Sites の基本を学ぶ - WKND チュートリアル {#introduction}

{{traditional-aem}}

これは、Adobe Experience Manager（AEM）を初めて使用する開発者向けに設計された、複数のパートから成るチュートリアルです。このチュートリアルでは、架空のライフスタイルブランド WKND の AEM Sites の実装について説明します。このチュートリアルでは、Adobe Experience Manager Sites を使用したプロジェクトの設定、コアコンポーネント、編集可能なテンプレート、クライアントサイドライブラリ、コンポーネント開発などの基本的なトピックについて説明します。

## 概要 {#wknd-tutorial-overview}

このマルチパートチュートリアルの目的は、Adobe Experience Manager(AEM) の最新の標準とテクノロジーを使用して Web サイトを実装する方法を開発者に教えることです。 このチュートリアルを修了すると、開発者はプラットフォームの基本的な基礎について理解し、AEM の一般的な設計パターンに関する知識を得ることができます。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Sites プロジェクトを開始するためのオプション

AEM Sites プロジェクトを開始するには、2 つの基本的な方法があります。

**AEM プロジェクトアーキタイプ** - Maven テンプレートを使用して最小限の AEM プロジェクトを生成する、AEM 開発に対する従来のアプローチ。これは、大量のカスタマイズが予想される AEM 6.5/6.4 プロジェクトおよび AEM as a Cloud Service プロジェクトで推奨されるアプローチです。このチュートリアルでは、AEM の開発について詳しく説明します。

[AEM プロジェクトアーキタイプを使用したチュートリアルの開始](./project-archetype/overview.md)

**AEM サイトテンプレート** - クイックサイト作成とも呼ばれ、事前定義されたサイトテンプレートを使用して AEM サイトを生成するローコードアプローチです。すぐに使用できるコンポーネントとテンプレートを使用して、サイトを素早く実行できます。テーマの設定ワークフローを使用して、CSS および JavaScript のみでブランド固有のスタイルとカスタマイズを適用します。新規プロジェクトやデベロッパーにお勧めです。AEM as a Cloud Service でのみ使用できます。

[サイトテンプレートを使用してチュートリアルを開始する](./site-template/create-site.md)

## Adobe XD UI キット

このチュートリアルを実際のシナリオに近づけるため、Adobe の UX デザイナーは [Adobe XD](https://helpx.adobe.com/jp/support/xd.html) を使ってサイトのモックアップを作成しました。このチュートリアルを通じて、モックアップの様々な部分を、完全に作成可能な AEM Sites に実装します。WKND サイトの美しいデザインを作成した **Lorenzo Buosi** と **Kilian Amendola** に謝意を表明したいと思います。

XD UI キットをダウンロードします。

* [AEM コアコンポーネント UI キット](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI キット](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## 参照サイト {#reference-site}

WKND サイトの最終バージョン（[https://wknd.site/](https://wknd.site/)）も参照できます。

このチュートリアルでは、AEM デベロッパーに必要な主要な開発スキルをカバーしていますが、サイト全体をエンドツーエンドで構築することは&#x200B;*ありません*。完成したリファレンスサイトは、すぐに使用できる AEM の機能を参照して確認するためのもう 1 つの優れたリソースです。

チュートリアルに入る前に最新のコードをテストするには、**[GitHub から最新リリース](https://github.com/adobe/aem-guides-wknd/releases/latest)をダウンロードし、インストールしてください**。

### Powered by Adobe Stock

WKND リファレンスサイトの画像の多くは [Adobe Stock](https://stock.adobe.com/jp/) のもので、[https://www.adobe.com/legal/terms.html](https://www.adobe.com/jp/legal/terms.html) のデモアセット追加条項で定義されたサードパーティーマテリアルです。Adobe Stock の画像を web サイトやマーケティング資料など、このデモ web サイトを閲覧した後に他の目的で使用する場合は、Adobe Stock でライセンスを購入できます。

Adobe Stockでは、写真、グラフィック、ビデオ、テンプレートなど、1 億 4000 万を超える高品質のロイヤリティフリー画像を利用して、クリエイティブなプロジェクトをすぐに開始できます。

## 次の手順 {#next-steps}

[AEM プロジェクトアーキタイプを使用して新しい Adobe Experience Manager プロジェクトを生成する](./project-archetype/overview.md)または[サイトテンプレートを使用してサイトを作成する](./site-template/create-site.md)で方法を説明します。
