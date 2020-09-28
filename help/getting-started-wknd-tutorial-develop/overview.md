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
translation-type: tm+mt
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '970'
ht-degree: 28%

---


# AEM Sites の概要 - WKND チュートリアル {#introduction}

Adobe Experience Manager(AEM)を初めて使用する開発者向けのマルチパートチュートリアルへようこそ。 このチュートリアルでは、WKNDの架空のライフスタイルブランドに対するAEMサイトの導入方法を説明します。 このチュートリアルでは、プロジェクトの設定、コアコンポーネント、編集可能なテンプレート、クライアント側ライブラリ、およびAdobe Experience Manager Sitesでのコンポーネントの開発など、基本的なトピックについて説明します。

## 概要 {#wknd-tutorial-overview}

この複数パートから成るチュートリアルは、Adobe Experience Manager（AEM）の最新の標準やテクノロジーを使用して Web サイトを実装するための手順を開発者に教えることを目的としています。このチュートリアルを完了したら、開発者はプラットフォームの基本的な基盤とAEMの一般的なデザインパターンに関する知識を理解する必要があります。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

このチュートリアルは、 **AEMをCloud Serviceとして使用するように設計されており、** AEM 6.5+ **および** AEM 6.4.2+と下位互換性があり ****&#x200B;ます。 サイトは次を使用して実装されます。

* [Maven AEM プロジェクトアーキタイプ](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/archetype/overview.html)
* [コアコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/ja-JP/experience-manager-htl/using/getting-started/getting-started.html)
* Sling Model
* [編集可能なテンプレート](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [スタイルシステム](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*チュートリアルの各パートの所要時間は 1～2 時間です。*

## このチュートリアルについて {#about-tutorial}

WKND は、いくつかの海外の都市のナイトライフ、アクティビティ、およびイベントに焦点を当てた架空のオンラインマガジンとブログです。

### Adobe XDUIキット

To make this tutorial closer to a real-world scenario Adobe&#39;s talented UX designers created the mockups for the site using [Adobe XD](https://www.adobe.com/products/xd.html). チュートリアルの過程で、さまざまなデザインが完全にオーサリング可能なAEMサイトに実装されます。 Special thanks to **Lorenzo Buosi** and **Kilian Amendola** who created the beautiful design for the WKND site.

XD UIキットをダウンロードします。

* [AEMコアコンポーネントUIキット](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UIキット](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

WKNDという名前は、開発者が ***週末の大部分をチュートリアルに費やすのに適しているので*** 、適しています。

### GitHub {#github}

プロジェクトのすべてのコードは AEM Guide リポジトリの GitHub にあります。

**[GitHub：WKND サイトプロジェクト](https://github.com/adobe/aem-guides-wknd)**

さらに、チュートリアルの各章は、GitHub に独自のブランチを持っています。ユーザーは、前のパートに対応するブランチをチェックするだけで、どこからでもチュートリアルを開始できます。

>[!NOTE]
>
> このチュートリアルの前のバージョンで作業していた場合でも、 [ソリューションパッケージ](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1) と [コードはGitHubにあります](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1) 。

## ローカル開発環境 {#local-dev-environment}

このチュートリアルを完了するには、ローカルの開発環境が必要です。スクリーンショットとビデオは、Mac OS環境で実行しているCloud ServiceSDKとしてAEMを使用してキャプチャされます。 コマンドとコードは、特に断りのない限り、ローカルのオペレーティングシステムとは独立している必要があります。

**AEMを初めてCloud Serviceに？** AEMをCloud ServiceSDKとして使用してローカル開発環境を設定するには、 [次のガイドを参照してください](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。

**AEM 6.5を初めて使用する場合** ローカル開発環境を設定するには、 [次のガイドを参照してください](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

### 必要なソフトウェア

次のファイルをローカルにインストールする必要があります。

* [CLOUD SERVICESDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk) 、 [AEM 6.5](https://helpx.adobe.com/jp/experience-manager/6-5/sites/deploying/using/technical-requirements.html) 、 [AEM 6.4 + SP2としてのAEM](https://helpx.adobe.com/jp/experience-manager/6-4/release-notes/sp-release-notes.html)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) (AEM 6.5以降のみ)
* [Apache Maven](https://maven.apache.org/) （3.3.9以降）
* [Node.js v10 以降](https://nodejs.org/en/)
* [npm 6 以降](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

### 統合開発環境(IDE)

このチュートリアルでは、 [AEM Developer](https://www.eclipse.org/)[](https://eclipse.adobe.com/aem/dev-tools/) Tool PluginをIDEとして使用しますが、JavaおよびMavenプロジェクトをサポートするIDEはどれでも使用できます。 このチュートリアルでは、IDEの特定の機能への依存が最小限に抑えられます。

Eclipseまたは [Visual Studioコード](https://code.visualstudio.com/) 、 [IntelliJなどの他のIDEを使用する詳細な手順については、](https://www.jetbrains.com/idea/)次のガイドを [参照してください](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 参照サイト {#reference-site}

WKNDサイトの完成版も参考にしてください。 [https://wknd.site/](https://wknd.site/)

このチュートリアルでは、AEM開発者に必要な主な開発スキルについて説明しますが、サイト全体をエンドツーエンドに構築する *わけではありません* 。 完成したリファレンスサイトも、すぐに使えるAEMの機能をさらに詳しく調べ、確認するための大きなリソースです。

To test the latest code before jumping into the tutorial, download and install the **[latest release from GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Adobe Stock電力

WKNDリファレンスWebサイトの画像の多くは、 [Adobe Stockのもので、https://www.adobe.com/legal/terms.htmlの「Demo Asset Additional Terms」で定義されているサードパーティのマテリアルで](https://stock.adobe.com/) す [](https://www.adobe.com/jp/legal/terms.html)。 Adobe Stockの画像を、Webサイトやマーケティング資料など、このデモWebサイトを閲覧する以外の目的で使用する場合は、Adobe Stockでライセンスを購入できます。

Adobe Stockでは、1億4000万を超える高品質でロイヤリティフリーの画像にアクセスでき、写真、グラフィック、ビデオ、テンプレートなど、クリエイティブプロジェクトを急速に開始できます。

## 次の手順 {#next-steps}

何を待ってる！!「 [プロジェクト設定](project-setup.md) 」の章に進み、AEMプロジェクトアーキタイプを使用して新しいAdobe Experience Managerプロジェクトを生成する方法を学び、チュートリアルを開始します。
