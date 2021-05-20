---
title: AEM Sites の概要 - WKND チュートリアル
description: AEM Sites の概要 - WKND チュートリアル. WKNDチュートリアルは、Adobe Experience Managerを初めて使用する開発者向けに設計された、複数のパートから成るチュートリアルです。 このチュートリアルでは、架空のライフスタイルブランドであるWKND向けのAEMサイトの実装について説明します。 このチュートリアルでは、プロジェクトの設定、Mavenアーキタイプ、コアコンポーネント、編集可能テンプレート、クライアントライブラリ、コンポーネント開発などの基本的なトピックについて説明します。
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
feature: コアコンポーネント、ページエディター、編集可能テンプレート、AEMプロジェクトアーキタイプ
topic: コンテンツ管理、開発
role: Developer
level: Beginner
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 10%

---


# AEM Sites の概要 - WKND チュートリアル {#introduction}

このたびは、Adobe Experience Manager(AEM)を初めて使用する開発者向けに設計された、複数のパートから成るチュートリアルをご利用いただき、誠にありがとうございます。 このチュートリアルでは、WKNDの架空のライフスタイルブランド向けにAEMサイトを実装する方法について説明します。 このチュートリアルでは、プロジェクトの設定、コアコンポーネント、編集可能なテンプレート、クライアント側ライブラリ、Adobe Experience Manager Sitesを使用したコンポーネント開発など、基本的なトピックについて説明します。

## 概要 {#wknd-tutorial-overview}

この複数パートから成るチュートリアルは、Adobe Experience Manager（AEM）の最新の標準やテクノロジーを使用して Web サイトを実装するための手順を開発者に教えることを目的としています。このチュートリアルを完了した後、開発者は、プラットフォームの基本を理解し、AEMの一般的なデザインパターンに関する知識を持つ必要があります。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Sitesプロジェクトを開始するためのオプション

AEM Sitesプロジェクトを開始するには、2つの基本的なアプローチがあります。

**AEMプロジェクトアーキタイプ**  - Mavenテンプレートを使用して最小限のAEMプロジェクトを生成することで、AEM開発に対する従来のアプローチです。これは、大量のカスタマイズが予想されるCloud ServiceプロジェクトとしてのAEM 6.5/6.4プロジェクトおよびAEMに推奨されるアプローチです。 このチュートリアルでは、AEMの開発について詳しく説明します。

[AEMプロジェクトアーキタイプのチュートリアルを開始する](./project-archetype/overview.md)

**AEMサイトテンプレート**  — 事前に定義されたサイトテンプレートを使用してAEMサイトを生成する低コードのアプローチです。すぐに使用できるコンポーネントとテンプレートを使用して、サイトをすぐに起動および実行できます。 テーマの設定ワークフローを使用して、CSSとJavaScriptのみでブランド固有のスタイルとカスタマイズを適用します。 新しいプロジェクトおよび開発者に推奨されます。 現在、AEM as aCloud Serviceでのみ使用できます。

[サイトテンプレートを使用してチュートリアルを開始する](./site-template/create-site.md)

## Adobe XD UIキット

このチュートリアルを実際のシナリオに近づけるために、Adobeの才能あるUXデザイナーは、[Adobe XD](https://www.adobe.com/products/xd.html)を使用してサイトのモックアップを作成しました。 チュートリアルの過程で、様々なデザインが完全に作成可能なAEMサイトに実装されます。 WKNDサイトの美しいデザインを作った&#x200B;**Lorenzo Buosi**&#x200B;と&#x200B;**Kilian Amendola**&#x200B;に特に感謝します。

XD UIキットをダウンロードします。

* [AEMコアコンポーネントUIキット](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UIキット](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## 参照サイト {#reference-site}

WKNDサイトの完成版も参照用として利用できます。[https://wknd.site/](https://wknd.site/)

このチュートリアルでは、AEM開発者に必要な主な開発スキルについて説明しますが、サイト全体をエンドツーエンドで構築するのではなく、**&#x200B;構築します。 完成したリファレンスサイトは、すぐに使用できるAEMの機能をさらに詳しく調べ、確認するためのもう1つの優れたリソースです。

このチュートリアルに進む前に最新のコードをテストするには、GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**から**[&#x200B;最新リリースをダウンロードしてインストールします。

### Adobe Stockを利用

WKNDリファレンスWebサイトの画像の多くは、[Adobe Stock](https://stock.adobe.com/)のもので、[https://www.adobe.com/legal/terms.html](https://www.adobe.com/jp/legal/terms.html)にある「Demo Asset Additional Terms」で定義されているサードパーティの資料です。 Adobe Stockの画像をWebサイトやマーケティング資料などで取り扱うなど、このデモWebサイトを閲覧する以外の目的で使用する場合は、Adobe Stockでライセンスを購入できます。

Adobe Stockでは、写真、グラフィック、ビデオ、テンプレートなど、1億4000万を超える高品質でロイヤリティフリーの画像にアクセスして、クリエイティブなプロジェクトをすぐに開始できます。

## 次の手順 {#next-steps}

何を待ってる?!AEMプロジェクトアーキタイプ](./project-archetype/overview.md)または[を使用して新しいAdobe Experience Managerプロジェクトを生成する方法を説明します。また、サイトテンプレート](./site-template/create-site.md)を使用してサイトを作成する方法も説明します。[
