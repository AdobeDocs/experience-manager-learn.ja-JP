---
title: テーマ設定ワークフロー | AEMクイックサイト作成
description: Adobe Experience Manager Site のテーマソースを更新して、ブランド固有のスタイルを適用する方法を説明します。 プロキシサーバーを使用して、CSS と JavaScript の更新のライブプレビューを表示する方法を説明します。 このチュートリアルでは、Cloud Manager のフロントエンドパイプラインを使用して、Adobeの更新をAEMサイトにデプロイする方法についても説明します。
version: Cloud Service
type: Tutorial
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 1%

---

# テーマ設定ワークフロー {#theming}

この章では、Adobe Experience Manager Site のテーマソースを更新して、ブランド固有のスタイルを適用します。 プロキシサーバーを使用して、ライブサイトに対してコードを作成する際に、CSS と JavaScript の更新のプレビューを表示する方法を学びます。 このチュートリアルでは、Cloud Manager のフロントエンドパイプラインを使用して、Adobeの更新をAEMサイトにデプロイする方法についても説明します。

最後に、WKND ブランドに合致するスタイルを含むようにサイトを更新します。

## 前提条件 {#prerequisites}

これは複数のパートから成るチュートリアルで、 [ページテンプレート](./page-templates.md) チャプターが完了しました。

## 目的

1. サイトのテーマソースをダウンロードして変更する方法を説明します。
1. リアルタイムプレビューのためにライブサイトに対するコードを説明します。
1. AdobeCloud Manager のフロントエンドパイプラインを使用して、コンパイル済みの CSS と JavaScript の更新をテーマの一部として配信する、エンドツーエンドのワークフローを理解します。

## テーマの更新 {#theme-update}

次に、WKND ブランドの外観と操作性に合わせて、テーマのソースを変更します。

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

ビデオの大まかな手順：

1. プロキシ開発サーバーで使用するローカルユーザーをAEMで作成します。
1. テーマのソースをAEMからダウンロードし、VSCode などのローカル IDE を使用して開きます。
1. テーマのソースを変更し、プロキシ開発サーバーを使用して、CSS と JavaScript の変更をリアルタイムでプレビューします。
1. 雑誌記事が WKND ブランドのスタイルとモックアップと一致するように、テーマのソースを更新します。

### ソリューションファイル

の完成したスタイルをダウンロード [WKND サンプルテーマ](assets/theming/WKND-THEME-src-1.1.zip)

## フロントエンドパイプラインを使用したテーマのデプロイ {#deploy-theme}

Cloud Manager のフロントエンドパイプラインを使用して、テーマの更新をAEM環境にデプロイします。

>[!VIDEO](https://video.tv.adobe.com/v/338722/?quality=12&learn=on)

ビデオの大まかな手順：

1. 新しい Git の作成 [Cloud Manager のリポジトリ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. Cloud Manager の Git リポジトリにテーマソースプロジェクトを追加します。

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. の設定 [フロントエンドパイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) Cloud Manager で、フロントエンドコードをデプロイします。
1. フロントエンドパイプラインを実行して、ターゲットAEM環境にアップデートをデプロイします。

### リポジトリの例

GitHub リポジトリの例をいくつか参照として使用できます。

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - 「実際の」プロジェクトの例として使用します。

## おめでとうございます。 {#congratulations}

おめでとうございます。テーマを更新し、AEMにデプロイしました。

### 次の手順 {#next-steps}

AEM開発に深く掘り下げ、 [AEM Project Archetype](../project-archetype/overview.md).
