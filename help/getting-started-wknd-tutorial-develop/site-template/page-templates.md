---
title: ページテンプレート
description: ページテンプレートを作成および変更する方法について説明します。 ページテンプレートとページとの関係を理解します。 コンテンツの詳細なガバナンスとブランドの一貫性を提供するために、ページテンプレートのポリシーを設定する方法について説明します。  Adobe XDのモックアップに基づいて、適切に構造化された Magazine 記事テンプレートが作成されます。
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
source-git-commit: 04096fe3c99cdcce2d43b2b29899c2bbe37ac056
workflow-type: tm+mt
source-wordcount: '691'
ht-degree: 6%

---

# ページテンプレート {#page-templates}

>[!CAUTION]
>
> 現在、クイックサイト作成ツールはテクニカルプレビューです。 テストおよび評価の目的で使用できるようになり、Adobeサポートに同意しない限り、実稼動での使用は意図されません。

この章では、ページテンプレートとページの関係を調べます。 以下のモックアップに基づいて、スタイル設定されていない Magazine 記事テンプレートを作成します。 [AdobeXD](https://www.adobe.com/products/xd.html). テンプレートの構築プロセスでは、コアコンポーネントと高度なポリシー設定について説明します。

## 前提条件 {#prerequisites}

これは複数のパートから成るチュートリアルで、 [コンテンツのオーサリングと変更の公開](./author-content-publish.md) チャプターが完了しました。

## 目的

1. ページテンプレートの詳細と、ページコンテンツを詳細に制御するためにポリシーを使用する方法について説明します。
1. テンプレートとページがリンクされている方法を説明します。
1. 新しいテンプレートを作成し、ページを作成します。

## 作成する内容 {#what-you-will-build}

このチュートリアルのこの部分では、新しい雑誌記事の作成に使用でき、共通の構造に合わせて調整できる新しい雑誌記事ページテンプレートを作成します。 このテンプレートは、AdobeXD で作成されたデザインと UI キットに基づいて作成されます。 この章では、テンプレートの構造またはスケルトンの構築にのみ焦点を当てます。 スタイルは実装されませんが、テンプレートとページは機能します。

## マガジン記事ページテンプレートの作成

ページを作成するとき、テンプレートを選択する必要があります。これは新しいページを作成するための基本として使用されます。テンプレートは、作成されるページの構造、初期コンテンツ、許可されるコンポーネントを定義します。

主に次の 3 つの領域があります。 [ページテンプレート](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=ja):

1. **構造**  — テンプレートの一部であるコンポーネントを定義します。 コンテンツ作成者はこれらを編集できません。
1. **初期コンテンツ**  — テンプレートの開始点となるコンポーネントを定義します。コンテンツ作成者は、これらのコンポーネントを編集または削除できます
1. **ポリシー**  — コンポーネントの動作方法と作成者が使用できるオプションに関する設定を定義します。

次に、モックアップの構造に合った新しいテンプレートをAEMで作成します。 これは、AEMのローカルインスタンスで発生します。 次のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

次のサムネールを使用して、テンプレートを特定できます（または独自のをアップロードできます）。

![記事ページテンプレートのサムネール](./assets/page-templates/article-page-template-thumbnail.png)


### ソリューションパッケージ

完了 [マガジンテンプレートの解決方法](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) は、パッケージマネージャーからダウンロードしてインストールできます。

## エクスペリエンスフラグメントを使用したヘッダーとフッターの更新 {#experience-fragments}

ヘッダーやフッターなどのグローバルコンテンツを作成する場合の一般的な方法は、 [エクスペリエンスフラグメント](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). エクスペリエンスフラグメントを使用すると、複数のコンポーネントを組み合わせて、1 つの参照可能なコンポーネントを作成できます。 エクスペリエンスフラグメントには、複数サイトの管理と [局在](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

Site テンプレートにより、ヘッダーとフッターが生成されました。 次に、モックアップと一致するようにエクスペリエンスフラグメントを更新します。 次のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

以下のビデオの概要手順：

1. サンプルコンテンツパッケージをダウンロードします。 **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**.
1. パッケージマネージャーを使用して、コンテンツパッケージをアップロードしインストールします。
1. WKND ロゴを使用するようにヘッダーおよびフッターエクスペリエンスフラグメントを更新する

## 雑誌記事ページの作成

次に、「雑誌記事ページ」テンプレートを使用して新しいページを作成します。 サイトのモックアップと一致するようにページのコンテンツを作成します。 次のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

以下を使用： [提供されたテキスト](./assets/page-templates/la-skateparks-copy.txt) を使用して、記事の本文を入力します。

## おめでとうございます。 {#congratulations}

これで、Adobe Experience Manager Sitesで新しいテンプレートとページが作成されました。

### 次の手順 {#next-steps}

この時点で、雑誌記事ページとサイトが WKND のブランドスタイルと一致しません。 フォロー： [テーマ設定](theming.md) グローバルスタイルをサイトに適用する際に使用する CSS および JavaScript フロントエンドコードの更新のベストプラクティスについて説明するチュートリアルです。

### ソリューションパッケージ

この章のソリューションパッケージをダウンロードできます。 [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
