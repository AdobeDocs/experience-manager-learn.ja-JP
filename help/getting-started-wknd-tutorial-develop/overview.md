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
feature: 「コアコンポーネント，ページエディタ，編集可能テンプレート， AEMプロジェクトのアーキタイプ」
topic: 「コンテンツ管理、開発」
role: デベロッパー
level: 初心者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 32%

---


# AEM Sites の概要 - WKND チュートリアル {#introduction}

Adobe Experience Manager(AEM)を初めて使用する開発者向けのマルチパートチュートリアルへようこそ。 このチュートリアルでは、WKNDの架空のライフスタイルブランドに対するAEMサイトの導入方法を説明します。 このチュートリアルでは、プロジェクトの設定、コアコンポーネント、編集可能なテンプレート、クライアント側ライブラリ、およびAdobe Experience Manager Sitesでのコンポーネントの開発など、基本的なトピックについて説明します。

## 概要 {#wknd-tutorial-overview}

この複数パートから成るチュートリアルは、Adobe Experience Manager（AEM）の最新の標準やテクノロジーを使用して Web サイトを実装するための手順を開発者に教えることを目的としています。このチュートリアルを完了したら、開発者はプラットフォームの基本的な基盤とAEMの一般的なデザインパターンに関する知識を理解する必要があります。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

このチュートリアルは、**AEMをCloud Service**&#x200B;として使用するように設計されており、**AEM 6.5.5.0+**&#x200B;および&#x200B;**AEM 6.4.8.1+**&#x200B;と下位互換性があります。 サイトは次を使用して実装されます。

* [Maven AEM プロジェクトアーキタイプ](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/archetype/overview.html)
* [コアコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/ja-JP/experience-manager-htl/using/getting-started/getting-started.html)
* Sling Model
* [編集可能なテンプレート](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [スタイルシステム](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*チュートリアルの各パートの所要時間は 1～2 時間です。*

## ローカル開発環境 {#local-dev-environment}

このチュートリアルを完了するには、ローカルの開発環境が必要です。スクリーンショットとビデオは、Mac OS環境上で[Visual Studio Code](https://code.visualstudio.com/)をIDEとして実行するCloud ServiceSDKとしてAEMを使用してキャプチャされます。 コマンドとコードは、特に断りのない限り、ローカルのオペレーティングシステムとは独立している必要があります。

### 必要なソフトウェア

以下をローカルにインストールしておく必要があります。

* ローカルAEM **作成者**&#x200B;インスタンス(Cloud ServiceSDK、6.5.5以上、6.4.8.1以上)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)（3.3.9 以降）
* [Node.js](https://nodejs.org/ja/) （LTS — 長期サポート）
* [npm 6 以降](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio](https://code.visualstudio.com/) コードまたは同等のIDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  — チュートリアル全体で使用されるツール

>[!NOTE]
>
> **AEM as a Cloud Service は初めてですか？** AEMをCloud ServiceSDKとして使用してローカル開発環境を設定するには、 [次のガイドを参照してください](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5を初めて使用する場合** ローカル開発環境を設定するには、 [次のガイドを参照してください](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## このチュートリアルについて {#about-tutorial}

WKND は、いくつかの海外の都市のナイトライフ、アクティビティ、およびイベントに焦点を当てた架空のオンラインマガジンとブログです。

### Adobe XDUIキット

このチュートリアルを現実のシナリオに近づけるために、Adobeの有能なUXデザイナーが、[Adobe XD](https://www.adobe.com/products/xd.html)を使ってサイトのモックアップを作成しました。 チュートリアルの過程で、さまざまなデザインが完全にオーサリング可能なAEMサイトに実装されます。 WKND地域の美しいデザインを作った&#x200B;**ロレンツォ・ブオシ**&#x200B;と&#x200B;**キリアン・アメンデーション・オラ**&#x200B;に感謝。

XD UIキットをダウンロードします。

* [AEMコアコンポーネントUIキット](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UIキット](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

WKNDという名前は、開発者がチュートリアルを完了するために&#x200B;***週末***&#x200B;の方が良いと思うので、適切です。

### GitHub {#github}

プロジェクトのすべてのコードは AEM Guide リポジトリの GitHub にあります。

**[GitHub：WKND サイトプロジェクト](https://github.com/adobe/aem-guides-wknd)**

さらに、チュートリアルの各章は、GitHub に独自のブランチを持っています。ユーザーは、前のパートに対応するブランチをチェックするだけで、どこからでもチュートリアルを開始できます。

>[!NOTE]
>
> このチュートリアルの前のバージョンで作業していた場合、GitHubには[ソリューションパッケージ](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1)と[コード](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1)が残っています。

## 参照サイト {#reference-site}

WKNDサイトの完成版も参考にしてください。[https://wknd.site/](https://wknd.site/)

このチュートリアルでは、AEM開発者に必要な主な開発スキルを習得しますが、**&#x200B;ではなく、サイト全体をエンドツーエンドに構築します。 完成したリファレンスサイトも、すぐに使えるAEMの機能をさらに詳しく調べ、確認するための大きなリソースです。

チュートリアルに進む前に最新のコードをテストするには、GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**から**[&#x200B;最新のリリースをダウンロードしてインストールします。

### Adobe Stock電力

WKNDリファレンスWebサイトの画像の多くは、[Adobe Stock](https://stock.adobe.com/)のもので、[https://www.adobe.com/legal/terms.html](https://www.adobe.com/jp/legal/terms.html)のDemo Asset Additional Termsで定義されているThird Party Materialです。 Adobe Stockの画像を、Webサイトやマーケティング資料など、このデモWebサイトを閲覧する以外の目的で使用する場合は、Adobe Stockでライセンスを購入できます。

Adobe Stockでは、1億4000万を超える高品質でロイヤリティフリーの画像にアクセスでき、写真、グラフィック、ビデオ、テンプレートなど、クリエイティブプロジェクトを急速に開始できます。

## 次の手順 {#next-steps}

何を待ってる！!「[プロジェクト設定](project-setup.md)」の章に進み、AEMプロジェクトアーキタイプを使用して新しいAdobe Experience Managerプロジェクトを生成する方法を学び、チュートリアルを開始します。
