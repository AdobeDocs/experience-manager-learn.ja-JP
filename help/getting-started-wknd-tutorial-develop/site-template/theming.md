---
title: 主題設定ワークフロー
seo-title: はじめに —AEM Sites — テーマワークフロー
description: Adobe Experience Managerサイトのテーマソースを更新して、ブランド固有のスタイルを適用する方法を説明します。 プロキシサーバーを使用してCSSとJavaScriptの更新のライブプレビューを表示する方法について説明します。 このチュートリアルでは、GitHubの操作を使用してAEMサイトにテーマの更新を展開する方法も説明します。
sub-product: サイト
version: Cloud Service
type: Tutorial
feature: コアコンポーネント
topic: コンテンツ管理、開発
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 1%

---


# 主題設定ワークフロー{#theming}

>[!CAUTION]
>
> ここで紹介するクイックサイト作成機能は、2021年下半期にリリースされます。 関連ドキュメントは、プレビュー目的で利用できます。

この章では、Adobe Experience Managerサイトのテーマソースを更新し、ブランド固有のスタイルを適用します。 本番用サイトに対してコードを作成する際に、プロキシサーバーを使用してCSSとJavaScriptの更新のプレビューを表示する方法を習得します。 このチュートリアルでは、GitHubの操作を使用してAEMサイトにテーマの更新を展開する方法も説明します。

最終的に、WKNDのブランドに合わせたスタイルを含むようにサイトが更新されます。

## 前提条件 {#prerequisites}

これはマルチパート形式のチュートリアルで、[ページテンプレート](./page-templates.md)の章で概要を説明した手順が完了していることを前提としています。

## 目的

1. サイトのテーマソースをダウンロードおよび変更する方法について説明します。
1. リアルタイムプレビューのライブサイトに対するコードを学習します。
1. GitHubアクションを使用して、テーマの一部としてコンパイルされたCSSとJavaScriptの更新を配信する、エンドツーエンドのワークフローを理解します。

## テーマの更新{#theme-update}

次に、サイトがWKNDブランドのルック&amp;フィールと一致するように、テーマのソースを変更します。

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

ビデオの高レベルの手順：

1. プロキシ開発サーバーで使用するローカルユーザーをAEMで作成します。
1. AEMからテーマソースをダウンロードし、VSCodeなどのローカルIDEを使用して開きます。
1. テーマのソースを変更し、プロキシ開発サーバーを使用してCSSとJavaScriptの変更をリアルタイムでプレビューします。
1. 雑誌の記事がWKNDのブランドスタイルおよびモックアップと一致するように、テーマのソースを更新します。

### ソリューションファイル

[WKNDテーマ](assets/theming/WKND-THEME-src.zip)の完成したスタイルをダウンロードします

## テーマを展開{#deploy-theme}

GitHubの操作を使用して、AEM環境にテーマの更新を展開します。

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

ビデオの高レベルの手順：

1. テーマ追加のソースは、新しいリポジトリ](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)として[GitHubに投影されます。
1. GitHub](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)に[個人アクセストークンを作成し、安全な場所に保存します。
1. AEM環境のGitHub設定を構成して、GitHubリポジトリを指し示し、個人アクセストークンを含めます。
1. GitHubリポジトリに次の[暗号化されたシークレット](https://docs.github.com/en/actions/reference/encrypted-secrets)を作成します。
   * **AEM_SITE** - AEMサイトのルート(例 `wknd`)
   * **AEM_URL** - AEM作成者環境のURL
   * **GIT_TOKEN**  — 手順2の個人アクセストークン。
1. GitHubアクションを実行します。**Githubアーティファクト**&#x200B;を構築し、展開します。 これは、アクションを実行する際のダウンストリームの効果を持ちます。**アーティファクトid**&#x200B;を持つAEMのテーマ設定を更新します。これにより、テーマソースがAEM環境に展開され、`AEM_URL`と`AEM_SITE`で指定されたとおりになります。

### レポートの例

参照として使用できるGitHubリポジトリの例がいくつかあります。

* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e)  — 「real-world」プロジェクトの例として使用します。
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme)  — チュートリアルの次の例として使用します。

## これで完了です! {#congratulations}

新しいテーマを作成し、AEMに展開しました。

### 次の手順 {#next-steps}

AEMの開発について詳しく調べ、[AEMプロジェクトのアーキタイプ](../project-archetype/overview.md)を使用してサイトを作成し、基盤となるテクノロジーの詳細を理解します。
