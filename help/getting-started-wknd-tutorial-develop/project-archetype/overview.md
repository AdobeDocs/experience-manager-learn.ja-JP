---
title: AEM Sites使用の手引き — プロジェクトアーキタイプ
description: 「AEM Sitesの使用の手引き — プロジェクトアーキタイプ」を参照してください。 WKNDチュートリアルは、Adobe Experience Managerを初めて使用する開発者向けのマルチパートチュートリアルです。 このチュートリアルでは、架空のライフスタイルブランドであるWKNDに対するAEMサイトの導入方法について説明します。 このチュートリアルでは、プロジェクトの設定、Mavenアーキタイプ、コアコンポーネント、編集可能なテンプレート、クライアントライブラリ、コンポーネントの開発など、基本的なトピックについて説明します。
sub-product: サイト
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: コアコンポーネント，ページエディタ，編集可能なテンプレート， AEMプロジェクトのアーキタイプ
topic: コンテンツ管理、開発
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 41%

---


# AEM Sitesの使用の手引き — プロジェクトアーキタイプ{#project-archetype}

Adobe Experience Manager(AEM)を初めて使用する開発者向けのマルチパートチュートリアルへようこそ。 このチュートリアルでは、WKNDの架空のライフスタイルブランドに対するAEMサイトの導入方法を説明します。

このチュートリアル開始では、[AEMプロジェクトアーキタイプ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ja)を使用して新しいプロジェクトを生成します。

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
> **AEM as a Cloud Service は初めてですか？** AEMをCloud ServiceSDKとして使用するローカル開発環境を設定するには、 [次のガイドを参照してください](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5を初めて使用する場合** ローカル開発環境を設定するには、 [次のガイドを参照してください](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## GitHub {#github}

プロジェクトのすべてのコードは AEM Guide リポジトリの GitHub にあります。

**[GitHub：WKND サイトプロジェクト](https://github.com/adobe/aem-guides-wknd)**

さらに、チュートリアルの各章は、GitHub に独自のブランチを持っています。ユーザーは、前のパートに対応するブランチをチェックするだけで、どこからでもチュートリアルを開始できます。

## 次の手順 {#next-steps}

何を待ってる！!「[プロジェクト設定](project-setup.md)」の章に進み、AEMプロジェクトアーキタイプを使用して新しいAdobe Experience Managerプロジェクトを生成する方法を学び、チュートリアルを開始します。
