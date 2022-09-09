---
title: AEMでのコンテンツフラグメントのオーサリング
description: コンテンツフラグメントは、AEMのコンテンツの抽象化で、サポートするチャネルとは独立して、テキストベースのコンテンツをオーサリングおよび管理できます。
sub-product: content-services
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: Cloud Service
topic: Content Management
role: User
level: Beginner
exl-id: d33c033a-9577-4d4e-99be-f3c7e2a4ce73
source-git-commit: 9aae58c3301a7067baca374d6499f1afc3c95b06
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 12%

---

# コンテンツフラグメントのオーサリング {#authoring-content-fragments}

コンテンツフラグメントは、AEMのコンテンツの抽象化で、サポートするチャネルとは独立して、テキストベースのコンテンツをオーサリングおよび管理できます。

AEMコンテンツフラグメントは、テキストベースの編集コンテンツで、関連付けられた構造化されたデータ要素が含まれていても、デザイン情報やレイアウト情報のない純粋なコンテンツと見なされる場合があります。 コンテンツフラグメントは、通常、チャネルに依存しないコンテンツとして作成されます。これは、複数のチャネルで使用および再利用することを目的としており、コンテンツをコンテキスト固有のエクスペリエンスに含めます。

このビデオシリーズでは、AEMでのコンテンツフラグメントのオーサリングのライフサイクルを扱います。 詳細： [コンテンツフラグメントの配信は、こちらを参照してください。](content-fragments-delivery-feature-video-use.md).

1. コンテンツフラグメントモデルの有効化と定義
2. コンテンツフラグメントのオーサリング
3. コンテンツフラグメントのダウンロード
4. 編集機能

>[!CONTEXTUALHELP]
>id="aemcloud_sites_admin_content_fragments"
>title="フラグメントを管理"
>abstract="コンテンツフラグメントを使用して、ページに依存しないコンテンツの設計、作成、キュレーション、使用を行う方法を説明します。"

## コンテンツフラグメントモデルの定義 {#defining-content-fragment-models}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

AEMコンテンツフラグメントモデル（コンテンツフラグメントのデータスキーマ）は、AEMを介して有効にする必要があります [[!UICONTROL 設定ブラウザー]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=ja)：コンテンツフラグメントモデルを設定ごとに定義できます。

## コンテンツフラグメントの作成 {#creating-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

AEMの設定はAEM Assetsのフォルダー階層に適用され、コンテンツフラグメントモデルをコンテンツフラグメントとして作成できます。 コンテンツフラグメントは、豊富なフォームベースのオーサリングエクスペリエンスをサポートし、コンテンツを要素の集まりとしてモデル化できます。

コンテンツフラグメントには複数のバリアントを含めることができます。各バリアントは、コンテンツの異なるユースケース（チャネルとは限りません）に対応します。

*読み込み用のアスリート伝記の例：*\
**[sandra-sprient-bio.txt](assets/sandra-sprient-bio.txt)**

## コンテンツフラグメントのダウンロード {#downloading-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22450/?quality=12&learn=on)

AEMコンテンツフラグメントは、バリアント、要素、メタデータを含む Zip ファイルとして AEM オーサーからダウンロードできます。

*サンプルのコンテンツフラグメントダウンロード Zip ファイル：*\
**[daniel_schreder.zip](assets/daniel_schreder.zip)**

## コンテンツフラグメントの編集機能 {#editorial-capabilities}

>[!VIDEO](https://video.tv.adobe.com/v/25891/?quality=12&learn=on)

>[!NOTE]
>
> コンテンツフラグメントの注釈とバージョン比較については、 [AEM 6.4 Service Pack 2](https://helpx.adobe.com/jp/experience-manager/aem-releases-updates.html) および [AEM 6.3 Service Pack 3](https://helpx.adobe.com/jp/experience-manager/6-3/release-notes/sp3-release-notes.html).

## 次の手順

詳細 [コンテンツフラグメントの配信](content-fragments-delivery-feature-video-use.md).

## その他のリソース {#additional-resources}

* [コンテンツフラグメントの配信](content-fragments-delivery-feature-video-use.md)
* [AEM WCM コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)
* [AEM WCM コアコンテンツフラグメントコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=ja)

ビデオシリーズから最終状態のAEM 6.4 以降のインスタンスに以下のパッケージをダウンロードしてインストールするには、次の手順を実行します。

**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
