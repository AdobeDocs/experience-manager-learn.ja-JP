---
title: AEM でのコンテンツフラグメントのオーサリング
description: コンテンツフラグメントは、AEM のコンテンツの抽象化で、サポートするチャネルとは独立して、テキストベースのコンテンツをオーサリングおよび管理できます。
feature: Content Fragments
version: Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: d33c033a-9577-4d4e-99be-f3c7e2a4ce73
duration: 665
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '360'
ht-degree: 100%

---

# コンテンツフラグメントのオーサリング {#authoring-content-fragments}

コンテンツフラグメントは、AEM のコンテンツの抽象化で、サポートするチャネルとは独立して、テキストベースのコンテンツをオーサリングおよび管理できます。

AEM コンテンツフラグメントは、テキストベースのエディトリアルコンテンツで、関連付けられた構造化データ要素が含まれていても、デザイン情報やレイアウト情報のない純粋なコンテンツと見なされる場合があります。 コンテンツフラグメントは、通常、チャネルに依存しないコンテンツとして作成されます。これは、複数のチャネルで使用および再利用することを目的としており、コンテンツをコンテキスト固有のエクスペリエンスに含めます。

このビデオシリーズでは、AEM でのコンテンツフラグメントのオーサリングのライフサイクルを扱います。 コンテンツフラグメントの配信について詳しくは、[こちら](content-fragments-delivery-feature-video-use.md)を参照してください。

1. コンテンツフラグメントモデルの有効化と定義
2. コンテンツフラグメントのオーサリング
3. コンテンツフラグメントのダウンロード
4. エディトリアル機能

>[!CONTEXTUALHELP]
>id="aemcloud_sites_admin_content_fragments"
>title="フラグメントの管理"
>abstract="コンテンツフラグメントを使用することで、ページに依存しないコンテンツのデザイン、作成、キュレーションおよび使用を実現できる仕組みを説明します。"

## コンテンツフラグメントモデルの定義 {#defining-content-fragment-models}

>[!VIDEO](https://video.tv.adobe.com/v/38048?quality=12&learn=on&captions=jpn)

AEM コンテンツフラグメントモデル（コンテンツフラグメントのデータスキーマ）は、AEM の[[!UICONTROL 設定ブラウザー]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=ja)を介して有効にする必要があります。これは、コンテンツフラグメントモデルを設定ごとに定義できるようにします。

## コンテンツフラグメントの作成 {#creating-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/38053?quality=12&learn=on&captions=jpn)

AEM の設定は AEM Assets のフォルダー階層に適用され、コンテンツフラグメントモデルをコンテンツフラグメントとして作成できます。 コンテンツフラグメントは、豊富なフォームベースのオーサリングエクスペリエンスをサポートし、コンテンツを要素の集まりとしてモデル化できます。

コンテンツフラグメントには複数のバリアントを含めることができます。各バリアントは、コンテンツの異なるユースケース（チャネルとは限らない）に対応します。

*読み込み用のアスリート伝記の例：*\
**[sandra-sprient-bio.txt](assets/sandra-sprient-bio.txt)**

## コンテンツフラグメントのダウンロード {#downloading-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/38058?quality=12&learn=on&captions=jpn)

AEM コンテンツフラグメントは、バリアント、要素、メタデータを含む Zip ファイルとして AEM オーサーからダウンロードできます。

*サンプルのコンテンツフラグメントダウンロード Zip ファイル：*\
**[daniel_schreder.zip](assets/daniel_schreder.zip)**

## コンテンツフラグメントのエディトリアル機能 {#editorial-capabilities}

>[!VIDEO](https://video.tv.adobe.com/v/39652?quality=12&learn=on&captions=jpn)

>[!NOTE]
>
> コンテンツフラグメントの注釈とバージョン比較は、[AEM 6.4 サービスパック 2](https://helpx.adobe.com/jp/experience-manager/aem-releases-updates.html) および [AEM 6.3 サービスパック 3](https://helpx.adobe.com/jp/experience-manager/6-3/release-notes/sp3-release-notes.html) で導入されました。

## 次の手順

[コンテンツフラグメントの配信](content-fragments-delivery-feature-video-use.md)について学びます。

## その他のリソース {#additional-resources}

* [コンテンツフラグメントの配信](content-fragments-delivery-feature-video-use.md)
* [AEM WCM コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)
* [AEM WCM コアコンテンツフラグメントコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=ja)

ビデオシリーズから最終状態の AEM 6.4 以降のインスタンスに以下のパッケージをダウンロードしてインストールするには、次の手順を実行します。

**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
