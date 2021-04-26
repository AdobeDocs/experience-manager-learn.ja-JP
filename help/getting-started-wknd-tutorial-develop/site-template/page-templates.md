---
title: ページテンプレート
seo-title: AEM Sitesの概要 — ページテンプレート
description: ページテンプレートの作成および変更方法を説明します。 ページテンプレートとページの関係を理解します。 ページテンプレートのポリシーを設定して、コンテンツの詳細なガバナンスとブランドの一貫性を提供する方法について説明します。  Adobe XDのモックアップに基づいて、構造の整った雑誌記事のテンプレートが作成されます。
sub-product: サイト
version: Cloud Service
type: Tutorial
topic: コンテンツ管理
feature: コアコンポーネント，編集可能なテンプレート，ページエディター
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 5%

---


# ページテンプレート{#page-templates}

>[!CAUTION]
>
> ここで紹介するクイックサイト作成機能は、2021年下半期にリリースされます。 関連ドキュメントは、プレビュー目的で利用できます。

この章では、ページテンプレートとページの関係を調べます。 [AdobeXD](https://www.adobe.com/products/xd.html)のモックアップに基づいて、スタイル設定されていない雑誌記事テンプレートを作成します。 テンプレートの構築の過程で、コアコンポーネントと高度なポリシー設定について説明します。

## 前提条件 {#prerequisites}

これはマルチパートのチュートリアルで、[コンテンツを作成し、変更をパブリッシュする](./author-content-publish.md)の章で説明されている手順が完了していることを前提としています。

## 目的

1. Inspectは、Adobe XDで作成されたページデザインで、コアコンポーネントにマッピングします。
1. ページテンプレートの詳細と、ページコンテンツの詳細な制御を実施するためのポリシーの使用方法を理解します。
1. テンプレートとページのリンク方法を説明します。

## 作成する内容 {#what-you-will-build}

チュートリアルのこの部分では、新しい雑誌記事を作成し、共通の構造に合わせて調整するために使用できる新しい雑誌記事ページテンプレートを作成します。 テンプレートは、AdobeXDで作成されたデザインとUIキットに基づいて作成されます。 この章では、テンプレートの構造またはスケルトンの作成にのみ焦点を当てます。 スタイルは実装されませんが、テンプレートとページは機能します。

## Adobe XDとのUIの計画{#adobexd}

ほとんどの場合、モックアップと静的デザインを含む新しいWebサイト開始の作成を計画します。 [Adobe](https://www.adobe.com/products/xd.html) XDは、ユーザーエクスペリエンスを構築するデザインツールです。次に、UIキットとモックアップを調べ、記事ページテンプレートの構造の計画に役立ちます。

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**WKND記事デザインファイル [をダウンロードします](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**。

>[!NOTE]
>
> 汎用の[AEMコアコンポーネントUIキットも、カスタムプロジェクトの起点として](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd)提供されています。

## 雑誌記事ページテンプレートの作成

ページを作成するとき、テンプレートを選択する必要があります。これは新しいページを作成するための基本として使用されます。テンプレートは、結果のページ、初期コンテンツ、許可されるコンポーネントの構造を定義します。

[ページテンプレート](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html)には主に3つの領域があります。

1. **構造**  — テンプレートの一部であるコンポーネントを定義します。コンテンツ作成者はこれらを編集できません。
1. **初期コンテンツ**  — テンプレートを開始するコンポーネントを定義します。コンテンツ作成者はこれらを編集または削除できます。
1. **ポリシー**  — コンポーネントの動作方法と作成者が使用できるオプションに関する設定を定義します。

次に、モックアップの構造と一致するAEMで新しいテンプレートを作成します。 これは、AEMのローカルインスタンスで発生します。 次のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

### ソリューションパッケージ

完成した[ソリューションのマガジンテンプレート](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)は、Package Managerを使ってダウンロードし、インストールできます。

## ヘッダーとフッターをエクスペリエンスフラグメントに更新{#experience-fragments}

ヘッダーやフッターなどのグローバルコンテンツを作成する場合、一般に、[エクスペリエンスフラグメント](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)を使用します。 エクスペリエンスフラグメントを使用すると、複数のコンポーネントを組み合わせて、1つの参照可能なコンポーネントを作成できます。 エクスペリエンスフラグメントには、マルチサイト管理と[ローカライゼーション](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)をサポートする利点があります。

サイトテンプレートで、ヘッダーとフッターが生成されました。 次に、エクスペリエンスフラグメントを更新して、モックアップと一致させます。 次のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

以下のビデオの高レベルの手順を示します。

1. サンプルコンテンツパッケージ&#x200B;**[WKND-Starter-Assets-Skate-Article-1.0.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.0.zip)**&#x200B;をダウンロードします。
1. Package Managerを使用して、コンテンツパッケージをアップロードしてインストールします。
1. WKNDロゴを使用するためのヘッダーおよびフッターエクスペリエンスフラグメントの更新

## 雑誌記事ページの作成

次に、雑誌記事ページテンプレートを使用して新しいページを作成します。 ページのコンテンツを作成し、サイトのモックアップと一致させます。 次のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

## これで完了です! {#congratulations}

新しいテンプレートとページがAdobe Experience Manager Sitesに作成されました。

### 次の手順 {#next-steps}

この時点で、雑誌記事ページとサイトはWKNDのブランドスタイルと一致しません。 [テーマ設定](theming.md)のチュートリアルに従って、サイトにグローバルスタイルを適用するために使用されるCSSおよびJavaScriptのフロントエンドコードを更新するためのベストプラクティスを学習します。

### ソリューションパッケージ

この章のソリューションパッケージは以下の場所からダウンロードできます。[WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
