---
title: AEMでのコンテンツフラグメントのオーサリング
description: コンテンツフラグメントは、AEMのコンテンツを抽象化したもので、サポートするチャネルとは独立して、テキストベースのコンテンツを作成および管理できます。
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 6%

---

# コンテンツフラグメントのオーサリング {#authoring-content-fragments}

コンテンツフラグメントは、AEMのコンテンツを抽象化したもので、サポートするチャネルとは独立して、テキストベースのコンテンツを作成および管理できます。

AEMコンテンツフラグメントは、テキストベースの編集コンテンツで、関連付けられた構造化されたデータ要素が含まれていても、デザイン情報やレイアウト情報のない純粋なコンテンツと見なされる場合があります。 コンテンツフラグメントは、通常、チャネルに依存しないコンテンツとして作成され、チャネル間で使用および再使用することを目的としています。その後、コンテンツをコンテキスト固有のエクスペリエンスにラップします。

このビデオシリーズでは、AEMでのコンテンツフラグメントのオーサリングのライフサイクルを紹介します。 [コンテンツフラグメントの配信について詳しくは、](content-fragments-delivery-feature-video-use.md)を参照してください。

1. コンテンツフラグメントモデルの有効化と定義
2. コンテンツフラグメントのオーサリング
3. コンテンツフラグメントのダウンロード
4. 編集機能

## コンテンツフラグメントモデルの定義 {#defining-content-fragment-models}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

AEMコンテンツフラグメントモデル（コンテンツフラグメントのデータスキーマ）は、AEM [[!UICONTROL 設定ブラウザー]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)を使用して有効にする必要があります。これにより、設定ごとにコンテンツフラグメントモデルを定義できます。

## コンテンツフラグメントの作成 {#creating-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

AEMの設定は、AEM Assetsのフォルダー階層に適用され、コンテンツフラグメントモデルをコンテンツフラグメントとして作成できます。 コンテンツフラグメントは、要素のコレクションとしてコンテンツをモデル化できる、豊富なフォームベースのオーサリングエクスペリエンスをサポートします。

コンテンツフラグメントには複数のバリアントを含めることができます。各バリアントは、コンテンツの異なる使用例（必ずしもチャネルとは限りません）に対応します。

*輸入用のアスリート伝記の例：*\
**[sandra-sprient-bio.txt](assets/sandra-sprient-bio.txt)**

## コンテンツフラグメントのダウンロード {#downloading-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22450/?quality=12&learn=on)

AEMコンテンツフラグメントは、バリエーション、要素、メタデータを含むZipファイルとしてAEMオーサーからダウンロードできます。

*コンテンツフラグメントのダウンロードZipファイルの例：*\
**[daniel_schreder.zip](assets/daniel_schreder.zip)**

## コンテンツフラグメントの編集機能 {#editorial-capabilities}

>[!VIDEO](https://video.tv.adobe.com/v/25891/?quality=12&learn=on)

>[!NOTE]
>
> コンテンツフラグメントの注釈とバージョンの比較は、[AEM 6.4 Service Pack 2](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=ja)と[AEM 6.3 Service Pack 3](https://helpx.adobe.com/jp/experience-manager/6-3/release-notes/sp3-release-notes.html)で導入されました。

## 次の手順

[コンテンツフラグメントの配信](content-fragments-delivery-feature-video-use.md)について説明します。

## その他のリソース {#additional-resources}

* [コンテンツフラグメントの配信](content-fragments-delivery-feature-video-use.md)
* [AEM WCMコアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)
* [AEM WCMコアコンテンツフラグメントコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)

ビデオシリーズから最終状態のAEM 6.4以降のインスタンスに以下のパッケージをダウンロードしてインストールするには：

**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
