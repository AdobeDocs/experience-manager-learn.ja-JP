---
title: AEM Sites の概要 - WKND チュートリアル
description: AEM Sites の概要 - WKND チュートリアル. WKND チュートリアルは、Adobe Experience Managerを初めて使用する開発者向けに設計された、複数のパートから成るチュートリアルです。 このチュートリアルでは、架空のライフスタイルブランドである WKND 向けのAEMサイトの実装に関する手順を説明します。 このチュートリアルでは、プロジェクトの設定、Maven アーキタイプ、コアコンポーネント、編集可能テンプレート、クライアントライブラリ、コンポーネント開発などの基本的なトピックについて説明します。
sub-product: sites
topics: development
version: Cloud Service
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: 04096fe3c99cdcce2d43b2b29899c2bbe37ac056
workflow-type: tm+mt
source-wordcount: '624'
ht-degree: 10%

---

# AEM Sites の概要 - WKND チュートリアル {#introduction}

Adobe Experience Manager(AEM) を初めて使用する開発者向けに設計された、複数のパートから成るチュートリアルへようこそ。 このチュートリアルでは、架空のライフスタイルブランド WKND 向けのAEMサイトの実装について説明します。 このチュートリアルでは、プロジェクトの設定、コアコンポーネント、編集可能テンプレート、クライアント側ライブラリ、Adobe Experience Manager Sitesでのコンポーネント開発など、基本的なトピックについて説明します。

## 概要 {#wknd-tutorial-overview}

この複数パートから成るチュートリアルは、Adobe Experience Manager（AEM）の最新の標準やテクノロジーを使用して Web サイトを実装するための手順を開発者に教えることを目的としています。このチュートリアルを完了した後、開発者は、プラットフォームの基本を理解し、AEMの一般的なデザインパターンに関する知識を持つ必要があります。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Sites プロジェクトを開始するためのオプション

AEM Sitesプロジェクトを開始するには、2 つの基本的な方法があります。

**AEM Project Archetype** - Maven テンプレートを使用して最小限のAEMプロジェクトを生成する、AEM開発に対する従来のアプローチ。 これは、大量のカスタマイズが予想されるAEM 6.5/6.4 プロジェクトおよびAEMas a Cloud Serviceプロジェクトで推奨されるアプローチです。 このチュートリアルでは、AEMの開発について詳しく説明します。

[AEMプロジェクトアーキタイプを使用したチュートリアルの開始](./project-archetype/overview.md)

**AEM Site Templates**  — クイックサイト作成とも呼ばれ、事前定義されたサイトテンプレートを使用してAEMサイトを生成する低コードのアプローチです。 すぐに使用できるコンポーネントとテンプレートを使用して、サイトをすぐに使い始めます。 テーマの設定ワークフローを使用して、CSS と JavaScript のみでブランド固有のスタイルとカスタマイズを適用します。 新規プロジェクトおよび開発者に推奨されます。 AEM as a Cloud Serviceでのみ使用できます。

[サイトテンプレートを使用してチュートリアルを開始する](./site-template/create-site.md)

## Adobe XD UI キット

このチュートリアルを実際のシナリオに近づけるために、Adobeの才能ある UX デザイナーは、 [Adobe XD](https://www.adobe.com/products/xd.html). チュートリアルの過程で、様々なデザインが完全に作成可能なAEMサイトに実装されます。 特別感謝 **ロレンツォブオシ** および **キリアン・アメンドーラ** WKND サイト用の美しいデザインを作り上げたのは

XD UI キットをダウンロードします。

* [AEMコアコンポーネント UI キット](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI キット](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## 参照サイト {#reference-site}

WKND サイトの最終バージョンも参照できます。 [https://wknd.site/](https://wknd.site/)

このチュートリアルでは、AEM開発者に必要な主要な開発スキルについて説明しますが、次の操作をおこないます *not* エンドツーエンドでサイト全体を構築します。 完成したリファレンスサイトは、すぐに使用できるAEMの機能を参照して確認するためのもう 1 つの優れたリソースです。

このチュートリアルに進む前に最新のコードをテストするには、 **[GitHub からの最新リリース](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Powered by Adobe Stock

WKND リファレンス Web サイトの画像の多くは、 [Adobe Stock](https://stock.adobe.com/) とは、 [https://www.adobe.com/legal/terms.html](https://www.adobe.com/jp/legal/terms.html). Adobe Stockの画像を、Web サイトやマーケティング資料など、このデモ Web サイトを閲覧した後に、他の目的で使用する場合は、Adobe Stockでライセンスを購入できます。

Adobe Stockでは、写真、グラフィック、ビデオ、テンプレートなど、1 億 4000 万を超える高品質のロイヤリティフリー画像を利用して、クリエイティブなプロジェクトをすぐに開始できます。

## 次の手順 {#next-steps}

何を待ってる?!方法を学ぶ [AEMプロジェクトアーキタイプを使用して新しいAdobe Experience Managerプロジェクトを生成します](./project-archetype/overview.md) または [サイトテンプレートを使用してサイトを作成する](./site-template/create-site.md).
