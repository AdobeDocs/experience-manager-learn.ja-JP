---
title: AEMでのコンテンツフラグメントのオーサリング
description: 'コンテンツフラグメントはAEMのコンテンツ抽象です。この機能を使用すると、テキストベースのコンテンツを、サポートするチャネルとは独立して作成および管理できます。 '
sub-product: content-services
feature: content-fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 8%

---


# コンテンツフラグメントのオーサリング {#authoring-content-fragments}

コンテンツフラグメントはAEMのコンテンツ抽象です。この機能を使用すると、テキストベースのコンテンツを、サポートするチャネルとは独立して作成および管理できます。

>[!NOTE]
>
>これらのビデオで扱うAEMコンテンツフラグメント機能は、 [AEM 6.3 + FP 19008およびFP19614で初めて導入されました](https://helpx.adobe.com/experience-manager/6-3/release-notes/content-services-fragments-featurepack.html)。


AEMコンテンツフラグメントは、関連付けられた構造化データ要素の一部を含む場合があり、デザイン情報やレイアウト情報を持たない純粋なコンテンツと見なされる、テキストベースの編集コンテンツです。 コンテンツフラグメントは通常、チャネルにとらわれないコンテンツとして作成されます。これは複数のチャネルで使用および再利用され、その後コンテンツをコンテキスト固有のエクスペリエンスにまとめることを目的としています。

このビデオシリーズは、AEMのコンテンツフラグメントのオーサリングのライフサイクルをカバーしています。 コンテンツフラグメントの [配信について詳しくは、こちらを参照してください](content-fragments-delivery-feature-video-use.md)。

1. コンテンツフラグメントモデルの有効化と定義
2. コンテンツフラグメントのオーサリング
3. コンテンツフラグメントのダウンロード
4. 編集機能

## Defining Content Fragment Models {#defining-content-fragment-models}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

AEMコンテンツフラグメントモデル(コンテンツフラグメントのデータスキーマ)は、AEM [!UICONTROL 設定ブラウザー]( Configuration Browser)を使用して有効にする必要があります。これにより、コンテンツフラグメントモデルを設定ごとに定義できます。

## コンテンツフラグメントの作成 {#creating-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

AEMの設定は、AEM Assetsフォルダー階層に適用され、コンテンツフラグメントモデルをコンテンツフラグメントとして作成できます。 コンテンツフラグメントは、フォームベースの豊富なオーサリングエクスペリエンスをサポートしており、コンテンツを要素の集まりとしてモデル化できます。

コンテンツフラグメントには複数のバリアントを含めることができ、各バリアントはコンテンツの様々な使用例(考え方、必ずしもチャネルとは限りません)に対応しています。

*インポート用のアスリート伝記の例：*\
**[sandra-sprient-bio.txt](assets/sandra-sprient-bio.txt)**

## コンテンツフラグメントのダウンロード {#downloading-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22450/?quality=12&learn=on)

AEMコンテンツフラグメントは、バリアント、要素、メタデータを含むZipファイルとしてAEM Authorからダウンロードできます。

*コンテンツフラグメントのダウンロード用Zipファイルの例：*\
**[daniel_schreder.zip](assets/daniel_schreder.zip)**

## コンテンツフラグメント編集機能 {#editorial-capabilities}

>[!VIDEO](https://video.tv.adobe.com/v/25891/?quality=12&learn=on)

>[!NOTE]
>
> コンテンツフラグメントに対する注釈とバージョンの比較は、 [AEM 6.4 Service Pack 2](https://helpx.adobe.com/jp/experience-manager/aem-releases-updates.html) および [AEM 6.3 Service Pack 3で導入されました](https://helpx.adobe.com/jp/experience-manager/6-3/release-notes/sp3-release-notes.html)。

## 次の手順

コンテンツフラグメントの [配信について説明します](content-fragments-delivery-feature-video-use.md)。

## その他のリソース {#additional-resources}

* [コンテンツフラグメントの配信](content-fragments-delivery-feature-video-use.md)
* [AEM WCMコアコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html)
* [AEM WCMコアコンテンツフラグメントコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/components/content-fragment-component.html)

ビデオシリーズから最終状態のAEM 6.4以降のインスタンスに対して、以下のパッケージをダウンロードしてインストールするには：

**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
