---
title: AEM Sites の基本を学ぶ - プロジェクトアーキタイプ
description: AEM Sites の基本を学ぶ - プロジェクトアーキタイプ。WKND チュートリアルは、Adobe Experience Manager を初めて使用する開発者向けに設計された、複数のパートから成るチュートリアルです。このチュートリアルでは、架空のライフスタイルブランド「WKND」の AEM サイトの実装について順を追って説明します。プロジェクトの設定、コアコンポーネント、編集可能テンプレート、クライアントライブラリおよびコンポーネント開発などの基本的なトピックが含まれます。
version: 6.5, Cloud Service
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
doc-type: Tutorial
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
duration: 74
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '414'
ht-degree: 100%

---

# AEM Sites の基本を学ぶ - プロジェクトアーキタイプ {#project-archetype}

{{edge-delivery-services-and-page-editor}}

これは、Adobe Experience Manager（AEM）を初めて使用する開発者向けに設計された、複数のパートから成るチュートリアルです。このチュートリアルでは、架空のライフスタイルブランド WKND の AEM サイトの実装について順を追って説明します。

このチュートリアルでは、まず [AEM プロジェクトアーキタイプ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ja)を使用して、新しいプロジェクトを生成します。

このチュートリアルは **AEM as a Cloud Service** で動作するように設計されており、**AEM 6.5.14 以降**&#x200B;と下位互換性があります。サイトの実装には、以下を使用します。

* [Maven AEM プロジェクトアーキタイプ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ja)
* [コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=ja)
* [Sling Model](https://sling.apache.org/documentation/bundles/models.html)
* [編集可能なテンプレート](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=ja)
* [スタイルシステム](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=ja)

*チュートリアルの各パートの推定所要時間は 1～2 時間です。*

## ローカル開発環境 {#local-dev-environment}

このチュートリアルを完了するには、ローカル開発環境が必要です。スクリーンショットとビデオは、[Visual Studio Code](https://code.visualstudio.com/) を IDE とする、macOS 環境で動作する AEM as a Cloud Service SDK を使用してキャプチャしています。コマンドとコードは、特に明記されていない限り、ローカルオペレーティングシステムから独立している必要があります。

### 必要なソフトウェア

以下をローカルにインストールしておく必要があります。

* [ローカル AEM **オーサー**&#x200B;インスタンス](https://experience.adobe.com/#/downloads)（Cloud Service SDK または 6.5.14 以降）
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)（3.3.9 以降）
* [Node.js](https://nodejs.org/ja/)（LTS - 長期サポート）
* [npm 6 以降](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/) または同等の IDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - チュートリアル全体を通して使用するツール

>[!NOTE]
>
> **AEM as a Cloud Service を初めて使用する場合は、**[AEM as a Cloud Service SDK を使用してローカル開発環境をセットアップするためのガイド](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja)を確認してください。
>
> **AEM 6.5 を初めて使用する場合は、**[ローカル開発環境のセットアップについてのガイド](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja)を参照してください。

## GitHub {#github}

このチュートリアルのコードは、AEM ガイドリポジトリの GitHub にあります。

**[GitHub：WKND サイトプロジェクト](https://github.com/adobe/aem-guides-wknd)**

さらに、チュートリアルの各パートは、GitHub に独自のブランチを持っています。ユーザーは、前のパートに対応するブランチをチェックするだけで、どこからでもチュートリアルを開始できます。

## 次の手順 {#next-steps}

すぐに取りかかりましょう。[プロジェクトのセットアップ](project-setup.md)の章に移動してチュートリアルを開始し、AEM プロジェクトアーキタイプを使用して新しい Adobe Experience Manager プロジェクトを生成する方法について学びます。
