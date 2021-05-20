---
title: ページテンプレート
seo-title: AEM Sitesの概要 — ページテンプレート
description: ページテンプレートの作成および変更方法について説明します。 ページテンプレートとページの関係を理解します。 コンテンツに対して詳細なガバナンスとブランドの一貫性を提供するために、ページテンプレートのポリシーを設定する方法を説明します。  Adobe XDのモックアップに基づいて、適切に構造化されたMagazine記事テンプレートが作成されます。
sub-product: サイト
version: Cloud Service
type: Tutorial
topic: コンテンツ管理
feature: コアコンポーネント、編集可能なテンプレート、ページエディター
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 7%

---


# ページテンプレート{#page-templates}

>[!CAUTION]
>
> ここで紹介するクイックサイト作成機能は、2021年後半にリリースされます。 関連ドキュメントは、プレビュー用に提供されています。

この章では、ページテンプレートとページの関係を調べます。 [AdobeXD](https://www.adobe.com/products/xd.html)のモックアップに基づいて、スタイルが設定されていないMagazine記事テンプレートを作成します。 テンプレートの作成プロセスでは、コアコンポーネントと高度なポリシー設定について説明します。

## 前提条件 {#prerequisites}

これは複数のパートから成るチュートリアルで、[コンテンツのオーサーとパブリッシュの変更](./author-content-publish.md)の章で説明されている手順が完了していることを前提としています。

## 目的

1. Inspectは、Adobe XDで作成されたページデザインで、コアコンポーネントにマッピングします。
1. ページテンプレートの詳細と、ポリシーを使用してページコンテンツをきめ細かく制御する方法を理解します。
1. テンプレートとページのリンク方法を説明します。

## 作成する内容 {#what-you-will-build}

このチュートリアルのこのパートでは、新しい雑誌記事の作成に使用でき、共通の構造に合わせて使用できる新しい雑誌記事ページテンプレートを作成します。 このテンプレートは、AdobeXDで作成されたデザインとUIキットに基づいています。 この章では、テンプレートの構造またはスケルトンの構築にのみ焦点を当てます。 スタイルは実装されませんが、テンプレートとページは機能します。

## Adobe XDでのUI計画 {#adobexd}

ほとんどの場合、新しいWebサイトの計画は、モックアップと静的デザインから始まります。 [AdobeXD](https://www.adobe.com/products/xd.html) は、ユーザーエクスペリエンスを構築するデザインツールです。次に、UIキットとモックアップを調べて、記事ページテンプレートの構造を計画します。

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**WKND記事デザイン [ファイルをダウンロードします](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**。

>[!NOTE]
>
> 汎用の[AEMコアコンポーネントUIキットも、カスタムプロジェクトの出発点として](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd)利用できます。

## マガジン記事ページテンプレートの作成

ページを作成するとき、テンプレートを選択する必要があります。これは新しいページを作成するための基本として使用されます。テンプレートは、作成されるページの構造、初期コンテンツ、許可されるコンポーネントを定義します。

[ページテンプレート](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html)には、主に次の3つの領域があります。

1. **構造**  — テンプレートの一部であるコンポーネントを定義します。コンテンツ作成者はこれらを編集できません。
1. **初期コンテンツ**  — テンプレートの開始時に使用するコンポーネントを定義します。コンテンツ作成者はこれらのコンポーネントを編集または削除できます
1. **ポリシー**  — コンポーネントの動作方法と、作成者が使用できるオプションに関する設定を定義します。

次に、モックアップの構造に合った新しいテンプレートをAEMで作成します。 これは、AEMのローカルインスタンスで発生します。 次のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

### ソリューションパッケージ

完成した[マガジンテンプレート](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)のソリューションは、パッケージマネージャーを通じてダウンロードし、インストールできます。

## ヘッダーとフッターをエクスペリエンスフラグメントで更新{#experience-fragments}

ヘッダーやフッターなどのグローバルコンテンツを作成する場合、一般的な方法は、[エクスペリエンスフラグメント](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)を使用することです。 エクスペリエンスフラグメントを使用すると、複数のコンポーネントを組み合わせて、単一の参照可能なコンポーネントを作成できます。 エクスペリエンスフラグメントには、マルチサイト管理と[ローカライゼーション](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)をサポートする利点があります。

Siteテンプレートでヘッダーとフッターが生成された。 次に、モックアップと一致するようにエクスペリエンスフラグメントを更新します。 次のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

以下のビデオの大まかな手順：

1. サンプルコンテンツパッケージ&#x200B;**[WKND-Starter-Assets-Skate-Article-1.0.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.0.zip)**&#x200B;をダウンロードします。
1. パッケージマネージャーを使用して、コンテンツパッケージをアップロードしてインストールします。
1. WKNDロゴを使用するようにヘッダーおよびフッターエクスペリエンスフラグメントを更新する

## 雑誌記事ページの作成

次に、「雑誌記事ページ」テンプレートを使用して新しいページを作成します。 サイトのモックアップに合わせてページのコンテンツを作成します。 次のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

## バリデーターが {#congratulations}

これで、Adobe Experience Manager Sitesで新しいテンプレートとページが作成されました。

### 次の手順 {#next-steps}

この時点で、雑誌記事ページとサイトがWKNDのブランドスタイルと一致しません。 [テーマ](theming.md)のチュートリアルに従って、サイトにグローバルスタイルを適用する際に使用するCSSおよびJavaScriptフロントエンドコードを更新する際のベストプラクティスを学びます。

### ソリューションパッケージ

この章のソリューションパッケージをダウンロードできます。[WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)。
