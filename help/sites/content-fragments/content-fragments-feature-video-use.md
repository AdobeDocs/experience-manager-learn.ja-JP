---
title: AEMでのコンテンツフラグメントのオーサリング
description: 'コンテンツフラグメントはAEMのコンテンツ抽象です。この機能を使用すると、テキストベースのコンテンツを、サポートするチャネルとは独立して作成および管理できます。 '
sub-product: content-services
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: cloud-service
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 12%

---


# コンテンツフラグメントのオーサリング{#authoring-content-fragments}

コンテンツフラグメントはAEMのコンテンツ抽象です。この機能を使用すると、テキストベースのコンテンツを、サポートするチャネルとは独立して作成および管理できます。

AEMコンテンツフラグメントは、関連付けられた構造化データ要素の一部を含む場合があり、デザイン情報やレイアウト情報を持たない純粋なコンテンツと見なされる、テキストベースの編集コンテンツです。 コンテンツフラグメントは通常、チャネルにとらわれないコンテンツとして作成されます。これは複数のチャネルで使用および再利用され、その後コンテンツをコンテキスト固有のエクスペリエンスにまとめることを目的としています。

このビデオシリーズは、AEMのコンテンツフラグメントのオーサリングのライフサイクルをカバーしています。 [コンテンツフラグメントの配信について詳しくは、](content-fragments-delivery-feature-video-use.md)を参照してください。

1. コンテンツフラグメントモデルの有効化と定義
2. コンテンツフラグメントのオーサリング
3. コンテンツフラグメントのダウンロード
4. 編集機能

## コンテンツフラグメントモデルの定義{#defining-content-fragment-models}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

AEMコンテンツフラグメントモデル(コンテンツフラグメントのデータスキーマ)は、AEM [[!UICONTROL 設定ブラウザー]](https://docs.adobe.com/content/help/ja-JP/experience-manager-cloud-service/implementing/developing/configurations.html)を介して有効にする必要があります。これにより、コンテンツフラグメントモデルを構成ごとに定義できます。

## コンテンツフラグメントの作成 {#creating-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

AEMの設定は、AEM Assetsフォルダー階層に適用され、コンテンツフラグメントモデルをコンテンツフラグメントとして作成できます。 コンテンツフラグメントは、フォームベースの豊富なオーサリングエクスペリエンスをサポートしており、コンテンツを要素の集まりとしてモデル化できます。

コンテンツフラグメントには複数のバリアントを含めることができ、各バリアントはコンテンツの様々な使用例(考え方、必ずしもチャネルとは限りません)に対応しています。

*インポート用のアスリート伝記の例：*\
**[sandra-sprient-bio.txt](assets/sandra-sprient-bio.txt)**

## コンテンツフラグメントのダウンロード{#downloading-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22450/?quality=12&learn=on)

AEMコンテンツフラグメントは、バリアント、要素、メタデータを含むZipファイルとしてAEM Authorからダウンロードできます。

*コンテンツフラグメントのダウンロード用Zipファイルの例：*\
**[daniel_schreder.zip](assets/daniel_schreder.zip)**

## コンテンツフラグメント編集機能{#editorial-capabilities}

>[!VIDEO](https://video.tv.adobe.com/v/25891/?quality=12&learn=on)

>[!NOTE]
>
> コンテンツフラグメントの注釈とバージョンの比較が、[AEM 6.4 Service Pack 2](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=ja)と[AEM 6.3 Service Pack 3](https://helpx.adobe.com/jp/experience-manager/6-3/release-notes/sp3-release-notes.html)で導入されました。

## 次の手順

[コンテンツフラグメント](content-fragments-delivery-feature-video-use.md)の配信について説明します。

## その他のリソース {#additional-resources}

* [コンテンツフラグメントの配信](content-fragments-delivery-feature-video-use.md)
* [AEM WCMコアコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html)
* [AEM WCMコアコンテンツフラグメントコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/components/content-fragment-component.html)

ビデオシリーズから最終状態のAEM 6.4以降のインスタンスに対して、以下のパッケージをダウンロードしてインストールするには：

**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
