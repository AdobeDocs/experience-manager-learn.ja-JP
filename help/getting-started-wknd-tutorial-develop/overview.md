---
title: AEM Sites の概要 - WKND チュートリアル
description: AEM Sites の概要 - WKND チュートリアル. WKNDチュートリアルは、Adobe Experience Managerを初めて使用する開発者向けのマルチパートチュートリアルです。 このチュートリアルでは、架空のライフスタイルブランドであるWKNDに対するAEMサイトの導入方法について説明します。 このチュートリアルでは、プロジェクトの設定、Mavenアーキタイプ、コアコンポーネント、編集可能なテンプレート、クライアントライブラリ、コンポーネントの開発など、基本的なトピックについて説明します。
sub-product: サイト
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: コアコンポーネント，ページエディタ，編集可能なテンプレート， AEMプロジェクトのアーキタイプ
topic: コンテンツ管理、開発
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 10%

---


# AEM Sites の概要 - WKND チュートリアル {#introduction}

Adobe Experience Manager(AEM)を初めて使用する開発者向けのマルチパートチュートリアルへようこそ。 このチュートリアルでは、WKNDの架空のライフスタイルブランドに対するAEMサイトの導入方法を説明します。 このチュートリアルでは、プロジェクトの設定、コアコンポーネント、編集可能なテンプレート、クライアント側ライブラリ、およびAdobe Experience Manager Sitesでのコンポーネントの開発など、基本的なトピックについて説明します。

## 概要 {#wknd-tutorial-overview}

この複数パートから成るチュートリアルは、Adobe Experience Manager（AEM）の最新の標準やテクノロジーを使用して Web サイトを実装するための手順を開発者に教えることを目的としています。このチュートリアルを完了したら、開発者はプラットフォームの基本的な基盤とAEMの一般的なデザインパターンに関する知識を理解する必要があります。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## サイトプロジェクトを開始するためのオプション

AEM Sites事業を始めるには2つの基本的なアプローチがある。

**AEMプロジェクトアーキタイプ** - Mavenテンプレートを使用して最小限のAEMプロジェクトを生成することで、AEM開発に対する従来のアプローチです。これは、大量のカスタマイズを予想するCloud Serviceプロジェクトとして、AEM 6.5/6.4のプロジェクトとAEMに推奨されるアプローチです。 このチュートリアルでは、AEMの開発について詳しく説明します。

[AEM Project Archetypeとのチュートリアルの開始](./project-archetype/overview.md)

**AEMサイトテンプレート**  — 定義済みのサイトテンプレートを使用してAEMサイトを生成する低コードのアプローチです。すぐに使用できるコンポーネントとテンプレートを使用して、サイトをすばやく起動し、実行します。 テーマ設定ワークフローを使用して、CSSとJavaScriptのみを使用してブランド固有のスタイルとカスタマイズを適用します。 新しいプロジェクトおよび開発者にお勧めします。 現在、Cloud ServiceとしてAEMでのみ使用できます。

[サイトテンプレートを使用したチュートリアルの開始](./site-template/create-site.md)

## Adobe XDUIキット

このチュートリアルを現実のシナリオに近づけるために、Adobeの有能なUXデザイナーが、[Adobe XD](https://www.adobe.com/products/xd.html)を使ってサイトのモックアップを作成しました。 チュートリアルの過程で、さまざまなデザインが完全にオーサリング可能なAEMサイトに実装されます。 WKND地域の美しいデザインを作った&#x200B;**ロレンツォ・ブオシ**&#x200B;と&#x200B;**キリアン・アメンデーション・オラ**&#x200B;に感謝。

XD UIキットをダウンロードします。

* [AEMコアコンポーネントUIキット](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UIキット](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## 参照サイト {#reference-site}

WKNDサイトの完成版も参考にしてください。[https://wknd.site/](https://wknd.site/)

このチュートリアルでは、AEM開発者に必要な主な開発スキルを習得しますが、**&#x200B;ではなく、サイト全体をエンドツーエンドに構築します。 完成したリファレンスサイトも、すぐに使えるAEMの機能をさらに詳しく調べ、確認するための大きなリソースです。

チュートリアルに進む前に最新のコードをテストするには、GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**から**[&#x200B;最新のリリースをダウンロードしてインストールします。

### Adobe Stock電力

WKNDリファレンスWebサイトの画像の多くは、[Adobe Stock](https://stock.adobe.com/)のもので、[https://www.adobe.com/legal/terms.html](https://www.adobe.com/jp/legal/terms.html)のDemo Asset Additional Termsで定義されているThird Party Materialです。 Adobe Stockの画像を、Webサイトやマーケティング資料など、このデモWebサイトを閲覧する以外の目的で使用する場合は、Adobe Stockでライセンスを購入できます。

Adobe Stockでは、1億4000万を超える高品質でロイヤリティフリーの画像にアクセスでき、写真、グラフィック、ビデオ、テンプレートなど、クリエイティブプロジェクトを急速に開始できます。

## 次の手順 {#next-steps}

何を待ってる！!AEMプロジェクトアーキタイプ](./project-archetype/overview.md)を使用して新しいAdobe Experience Managerプロジェクトを[生成する方法、または[サイトテンプレート](./site-template/create-site.md)を使用してサイトを作成する方法を説明します。
