---
title: テーマ設定ワークフロー
seo-title: AEM Sites使用の手引き — テーマのワークフロー
description: Adobe Experience Manager Siteのテーマソースを更新して、ブランド固有のスタイルを適用する方法を説明します。 プロキシサーバーを使用して、CSSとJavaScriptの更新のライブプレビューを表示する方法を説明します。 このチュートリアルでは、GitHubアクションを使用してAEM Siteにテーマの更新をデプロイする方法についても説明します。
sub-product: サイト
version: Cloud Service
type: Tutorial
feature: コアコンポーネント
topic: コンテンツ管理、開発
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 1%

---


# テーマ設定ワークフロー {#theming}

>[!CAUTION]
>
> ここで紹介するクイックサイト作成機能は、2021年後半にリリースされます。 関連ドキュメントは、プレビュー用に提供されています。

この章では、Adobe Experience Manager Siteのテーマソースを更新し、ブランド固有のスタイルを適用します。 ライブサイトに対してコードを作成する際に、プロキシサーバーを使用してCSSとJavaScriptの更新のプレビューを表示する方法を説明します。 このチュートリアルでは、GitHubアクションを使用してAEM Siteにテーマの更新をデプロイする方法についても説明します。

最後に、WKNDブランドに合致するスタイルを含むようにサイトを更新します。

## 前提条件 {#prerequisites}

これは複数のパートから成るチュートリアルで、[ページテンプレート](./page-templates.md)の章で説明した手順が完了していることを前提としています。

## 目的

1. サイトのテーマソースをダウンロードして変更する方法を説明します。
1. リアルタイムプレビューのために、ライブサイトに対するコードを説明します。
1. GitHubアクションを使用して、コンパイル済みのCSSとJavaScriptの更新をテーマの一部として配信する、エンドツーエンドのワークフローを理解します。

## テーマ{#theme-update}の更新

次に、テーマのソースを変更し、サイトがWKNDブランドのルックアンドフィールに一致するようにします。

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

ビデオの大まかな手順：

1. プロキシ開発サーバーで使用するローカルユーザーをAEMで作成します。
1. テーマのソースをAEMからダウンロードし、VSCodeなどのローカルIDEを使用して開きます。
1. テーマのソースを変更し、プロキシ開発サーバーを使用して、CSSとJavaScriptの変更をリアルタイムでプレビューします。
1. 雑誌記事がWKNDブランドのスタイルとモックアップと一致するようにテーマのソースを更新します。

### ソリューションファイル

[WKNDテーマ](assets/theming/WKND-THEME-src.zip)の完成したスタイルをダウンロードします。

## テーマ{#deploy-theme}を展開する

GitHubアクションを使用して、テーマの更新をAEM環境にデプロイします。

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

ビデオの大まかな手順：

1. テーマソースプロジェクトを新しいリポジトリとして[GitHubに追加します。](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)
1. GitHub](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)に[個人用アクセストークンを作成し、安全な場所に保存します。
1. AEM環境でGitHub設定を指定し、自分のGitHubリポジトリを指すように設定し、自分の個人用アクセストークンを含めます。
1. 次の[暗号化されたシークレット](https://docs.github.com/en/actions/reference/encrypted-secrets)をGitHubリポジトリに作成します。
   * **AEM_SITE**  - AEMサイトのルート( `wknd`)
   * **AEM_URL**  - AEMオーサー環境のURL
   * **GIT_TOKEN**  — 手順2の個人用アクセストークン。
1. GitHubアクションを実行します。**GitHubアーティファクト**&#x200B;を構築してデプロイします。 これは、アクションを実行すると、ダウンストリームの効果を持ちます。**AEMのテーマ設定をアーティファクトID**&#x200B;で更新します。これにより、`AEM_URL`および`AEM_SITE`で指定されたとおりに、テーマソースがAEM環境にデプロイされます。

### reposの例

参照として使用できるGitHubリポジトリの例をいくつか示します。

* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e)  - 「リアルワールド」プロジェクトの例として使用します。
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme)  — チュートリアルに続く例として使用します。

## バリデーターが {#congratulations}

これで、テーマを作成し、AEMに展開しました。

### 次の手順 {#next-steps}

[AEMプロジェクトアーキタイプ](../project-archetype/overview.md)を使用してサイトを作成し、AEMの開発に深く掘り下げ、基盤となるテクノロジーの詳細を理解します。
