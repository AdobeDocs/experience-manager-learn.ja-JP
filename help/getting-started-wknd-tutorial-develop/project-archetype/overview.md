---
title: AEM Sitesの概要 — プロジェクトアーキタイプ
description: AEM Sitesの概要 — プロジェクトアーキタイプ。 WKND チュートリアルは、Adobe Experience Managerを初めて使用する開発者向けに設計された、複数のパートから成るチュートリアルです。 このチュートリアルでは、架空のライフスタイルブランドである WKND 向けのAEMサイトの実装に関する手順を説明します。 このチュートリアルでは、プロジェクトの設定、Maven アーキタイプ、コアコンポーネント、編集可能テンプレート、クライアントライブラリ、コンポーネント開発などの基本的なトピックについて説明します。
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
source-git-commit: bbdb045edf5f2c68eec5094e55c1688e725378dc
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 40%

---

# AEM Sitesの概要 — プロジェクトアーキタイプ {#project-archetype}

Adobe Experience Manager(AEM) を初めて使用する開発者向けに設計された、複数のパートから成るチュートリアルへようこそ。 このチュートリアルでは、架空のライフスタイルブランド WKND の AEM Sites の実装について説明します。

このチュートリアルでは、最初に [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) をクリックして、新しいプロジェクトを生成します。

このチュートリアルは、 **AEMas a Cloud Service** との後方互換性がある **AEM 6.5.14 以降**. サイトは次を使用して実装されます。

* [Maven AEM プロジェクトアーキタイプ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ja)
* [コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)
* [Sling Model](https://sling.apache.org/documentation/bundles/models.html)
* [編集可能なテンプレート](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=ja)
* [スタイルシステム](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=ja)

*チュートリアルの各パートの所要時間は 1～2 時間です。*

## ローカル開発環境 {#local-dev-environment}

このチュートリアルを完了するには、ローカルの開発環境が必要です。スクリーンショットとビデオは、macOS環境で動作しているAEMas a Cloud ServiceSDK ( [Visual Studio Code](https://code.visualstudio.com/) を IDE として使用します。 特に断りのない限り、コマンドとコードはローカルのオペレーティングシステムから独立している必要があります。

### 必要なソフトウェア

以下をローカルにインストールしておく必要があります。

* [ローカルAEM **作成者** インスタンス](https://experience.adobe.com/#/downloads) (Cloud ServiceSDK または 6.5.14 以降 )
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)（3.3.9 以降）
* [Node.js](https://nodejs.org/ja/) （LTS — 長期サポート）
* [npm 6 以降](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/) または同等の IDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  — チュートリアル全体で使用されるツール

>[!NOTE]
>
> **AEM as a Cloud Service は初めてですか？** 以下を確認します。 [AEM as a Cloud Service SDK を使用したローカル開発環境の設定に関する以下のガイド](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja).
>
> **AEM 6.5 を初めて使用する場合** 以下を確認します。 [次のガイドでは、ローカル開発環境を設定します](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja).

## GitHub {#github}

このチュートリアルのコードは、AEM Guide リポジトリの GitHub にあります。

**[GitHub：WKND サイトプロジェクト](https://github.com/adobe/aem-guides-wknd)**

さらに、チュートリアルの各章は、GitHub に独自のブランチを持っています。ユーザーは、前のパートに対応するブランチをチェックするだけで、どこからでもチュートリアルを開始できます。

## 次のステップ {#next-steps}

何を待ってるの？ チュートリアルを開始するには、 [プロジェクト設定](project-setup.md) この章では、AEMプロジェクトアーキタイプを使用して新しいAdobe Experience Managerプロジェクトを生成する方法について説明します。
