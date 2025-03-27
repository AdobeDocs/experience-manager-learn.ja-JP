---
title: ワークフローのテーマ設定 | AEM クイックサイト作成
description: Adobe Experience Manager サイトのテーマソースを更新して、ブランド固有のスタイルを適用する方法を説明します。 プロキシサーバーを使用して、CSS と JavaScript のアップデートのライブプレビューを表示する方法を説明します。 このチュートリアルでは、Adobe Cloud Manager のフロントエンドパイプラインを使用して AEM サイトにテーマのアップデートをデプロイする方法についても説明します。
version: Experience Manager as a Cloud Service
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
duration: 1275
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '458'
ht-degree: 100%

---

# ワークフローのテーマ設定 {#theming}

この章では、Adobe Experience Manager サイトのテーマソースを更新して、ブランド固有のスタイルを適用します。 プロキシサーバーを使用して、ライブサイトのコードを作成する際に CSS と JavaScript のアップデートのプレビューを表示する方法について説明します。 このチュートリアルでは、Adobe Cloud Manager のフロントエンドパイプラインを使用して AEM サイトにテーマのアップデートをデプロイする方法についても説明します。

最後に、WKND ブランドに合致するスタイルを組み込むようにサイトを更新します。

## 前提条件 {#prerequisites}

これは複数のパートから成るチュートリアルで、[ページテンプレート](./page-templates.md)の章で説明した手順を既に完了していることを想定しています。

## 目的

1. サイトのテーマソースをダウンロードして変更する方法を説明します。
1. リアルタイムプレビューに対応するライブサイトのコードを作成する方法を説明します。
1. Adobe Cloud Manager のフロントエンドパイプラインを使用して、CSS と JavaScript のコンパイル済みアップデートをテーマの一部として配信するエンドツーエンドのワークフローを理解します。

## テーマの更新 {#theme-update}

次に、WKND ブランドのルックアンドフィールに合わせて、サイトのテーマソースを変更します。

>[!VIDEO](https://video.tv.adobe.com/v/332918?quality=12&learn=on)

ビデオの大まかな手順は次のとおりです。

1. プロキシ開発サーバーで使用するローカルユーザーを AEM に作成します。
1. テーマのソースを AEM からダウンロードし、VSCode などのローカル IDE を使用して開きます。
1. テーマのソースを変更し、プロキシ開発サーバーを使用して、CSS と JavaScript の変更をリアルタイムでプレビューします。
1. 雑誌記事が WKND ブランドのスタイルとモックアップに一致するように、テーマのソースを更新します。

### ソリューションファイル

[WKND サンプルテーマ](assets/theming/WKND-THEME-src-1.1.zip)の完成したスタイルをダウンロードします。

## フロントエンドパイプラインを使用したテーマのデプロイ {#deploy-theme}

Cloud Manager のフロントエンドパイプラインを使用して、テーマの更新を AEM 環境にデプロイします。

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

ビデオの大まかな手順は次のとおりです。

1. [Cloud Manager で Git リポジトリ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html?lang=ja)を新規作成します。
1. テーマソースプロジェクトを Cloud Manager の Git リポジトリに追加します。

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. フロントエンドコードをデプロイする[フロントエンドパイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=ja)を Cloud Manager で設定します。
1. フロントエンドパイプラインを実行して、アップデートをターゲット AEM 環境にデプロイします。

### リポジトリの例

GitHub リポジトリの例をいくつか参考として使用できます。

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-standard-theme-e2e) -「実際の」プロジェクトの例として使用します。

## おめでとうございます。 {#congratulations}

おめでとうございます。これで、テーマを更新して AEM にデプロイしました。

### 次の手順 {#next-steps}

AEM の開発を深く掘り下げ、基盤となるテクノロジーをさらに理解するには、[AEM プロジェクトアーキタイプ](../project-archetype/overview.md)を使用してサイトを作成します。
