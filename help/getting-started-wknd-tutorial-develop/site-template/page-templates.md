---
title: ページテンプレート
description: ページテンプレートを作成および変更する方法について説明します。ページテンプレートとページとの関係を理解します。ページテンプレートのポリシーを設定することにより、コンテンツのきめ細かいガバナンスとブランドの一貫性を実現する方法を説明します。  Adobe XD で作成したモックアップをもとに、適切に構造化された雑誌記事テンプレートを作成しました。
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
recommendations: noDisplay, noCatalog
duration: 1561
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '628'
ht-degree: 100%

---

# ページテンプレート {#page-templates}

この章では、ページテンプレートとページの関係を探ります。スタイル設定されていない Magazine Article テンプレートを [AdobeXD](https://www.adobe.com/products/xd.html) のモックアップを基に作成します。テンプレートを作成するプロセスでは、コアコンポーネントと詳細ポリシー設定について説明します。

## 前提条件 {#prerequisites}

これはマルチパートチュートリアルであり、[コンテンツの作成と変更の公開](./author-content-publish.md)の章で大まかに説明されている手順が完了していることを前提としています。

## 目的

1. ページテンプレートの詳細と、ポリシーを使用してページコンテンツのきめ細かい制御を実施する方法を理解します。
1. テンプレートとページのリンク方法を学びます。
1. 新しいテンプレートを作成し、ページをオーサリングします。

## 作成する内容 {#what-you-will-build}

チュートリアルのこのパートでは、新しい雑誌記事の作成に使用でき共通の構造に沿う新しい Magazine Article Page テンプレートを作成します。このテンプレートは、AdobeXD で生成されたデザインと UI キットに基づいています。この章では、テンプレートの構造またはスケルトンの構築にのみ焦点を当てています。スタイルは実装されていませんが、テンプレートとページは機能しています。

## Magazine Article Page テンプレートの作成

ページを作成する際には、新しいページを作成するためのベースとして使用されるテンプレートを選択する必要があります。テンプレートでは、結果として作成されるページの構造、初期コンテンツおよび許可されたコンポーネントを定義します。

[ページテンプレート](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=ja)には、主な領域が次の 3 つあります。

1. **構造** - テンプレートの一部であるコンポーネントを定義します。コンテンツ作成者はこれらを編集できません。
1. **初期コンテンツ** - テンプレートの出発点となるコンポーネントを定義します。これは、コンテンツ作成者が編集または削除できます。
1. **ポリシー** - コンポーネントの動作、および作成者が利用可能なオプションに関する設定を定義します。

次に、モックアップの構造に合致する新しいテンプレートを AEM で作成します。これは、AEM のローカルインスタンスで行います。以下のビデオで示す手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/332915?quality=12&learn=on)

次のサムネールを使用して、テンプレートを特定できます（または独自のテンプレートをアップロードできます）。

![Article Page テンプレートサムネール](./assets/page-templates/article-page-template-thumbnail.png)


### ソリューションパッケージ

完成済みの [Magazine テンプレートソリューション](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip)をパッケージマネージャーでダウンロードしてインストールできます。

## エクスペリエンスフラグメントを使用したヘッダーとフッターの更新 {#experience-fragments}

ヘッダーやフッターなどのグローバルコンテンツを作成する際の一般的な方法は、[エクスペリエンスフラグメント](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=ja)を使用することです。エクスペリエンスフラグメントを使用すると、複数のコンポーネントを組み合わせて、1 つの参照可能なコンポーネントを作成できます。エクスペリエンスフラグメントには、マルチサイト管理と[ローカリゼーション](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=ja#localized-site-structure)をサポートするという利点があります。

Site テンプレートから、ヘッダーとフッターが生成されました。次に、モックアップと一致するようにエクスペリエンスフラグメントを更新します。以下のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/332916?quality=12&learn=on)

ビデオの大まかな手順は次のとおりです。

1. サンプルのコンテンツパッケージ **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)** をダウンロードします。
1. パッケージマネージャーを使用して、このコンテンツパッケージをアップロードしインストールします。
1. WKND ロゴを使用するように、ヘッダーとフッターのエクスペリエンスフラグメントを更新します。

## 雑誌記事ページの作成

次に、Magazine Article Page テンプレートを使用して新しいページを作成します。サイトのモックアップと一致するようにページのコンテンツを作成します。以下のビデオの手順に従います。

>[!VIDEO](https://video.tv.adobe.com/v/332917?quality=12&learn=on)

[用意されているテキスト](./assets/page-templates/la-skateparks-copy.txt)を記事の本文に入力します。

## おめでとうございます。 {#congratulations}

これで、Adobe Experience Manager Sites で新しいテンプレートとページを作成しました。

### 次の手順 {#next-steps}

現時点では、雑誌記事ページおよびサイトは WKND のブランドスタイルと一致しません。[テーマ設定](theming.md)チュートリアルに従って、サイトにグローバルスタイルを適用するための CSS と Javascript フロントエンドコードの更新に関するベストプラクティスを説明します。

### ソリューションパッケージ

この章のダウンロード可能なソリューションパッケージは、[WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip) です。
