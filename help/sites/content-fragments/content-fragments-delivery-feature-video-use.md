---
title: AEMでのコンテンツフラグメントの配信
seo-title: Adobe Experience Managerでのコンテンツフラグメントの配信
description: コンテンツフラグメントは、レイアウトとは無関係に、コアコンポーネントを使用してAEM Sitesで直接使用することも、ヘッドレスな方法でダウンストリームチャネルに配信することもできます。
seo-description: コンテンツフラグメントは、レイアウトとは無関係に、コアコンポーネントを使用してAEM Sitesで直接使用することも、ヘッドレスな方法でダウンストリームチャネルに配信することもできます。
sub-product: content-services
feature: コンテンツフラグメント
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: コンテンツ管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 7%

---


# コンテンツフラグメントの配信{#delivering-content-fragments}

Adobe Experience Manager(AEM)コンテンツフラグメントは、テキストベースの編集コンテンツで、関連付けられた構造化されたデータ要素が含まれていても、デザイン情報やレイアウト情報のない純粋なコンテンツと見なされる場合があります。 コンテンツフラグメントは、通常、チャネルに依存しないコンテンツとして作成され、チャネル間で使用および再使用することを目的としています。その後、コンテンツをコンテキスト固有のエクスペリエンスにラップします。

コンテンツフラグメントは、レイアウトとは無関係に、コアコンポーネントを使用してAEM Sitesで直接使用することも、ヘッドレスな方法でダウンストリームチャネルに配信することもできます。

このビデオシリーズでは、コンテンツフラグメントの使用に関する配信オプションについて説明します。 コンテンツフラグメントの定義と[オーサリングについて詳しくは、](content-fragments-feature-video-use.md)を参照してください。

1. Webページでのコンテンツフラグメントの使用
2. AEM Content Servicesを使用したJSONとしてのコンテンツフラグメントの公開
3. Assets HTTP APIの使用

## Webページでのコンテンツフラグメントの使用{#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

コンテンツフラグメントは、AEM Sitesページで、または同様の方法で、AEM WCMコアコンポーネントの[コンテンツフラグメントコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/components/content-fragment-component.html)を使用して、エクスペリエンスフラグメントを使用できます。

コンテンツフラグメントコンポーネントは、必要に応じてAEMスタイルシステムを使用してスタイルを設定し、コンテンツを表示することができます。

## コンテンツフラグメントをJSONとして公開する{#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Servicesは、コンテンツを正規化されたJSON形式にレンディションするAEMページベースのHTTPエンドポイントの作成を容易にします。

上記のビデオでは、 [コンテンツフラグメントコンポーネント](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)を使用して、個々のコンテンツフラグメントを公開します。 [コンテンツフラグメントリストコンポーネント](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html)は、コンテンツフラグメントのリストをページに動的に埋め込むクエリを作成者が定義できる新しいコンポーネントです。 複数のコンテンツフラグメントを公開する必要がある場合は、コンテンツフラグメントリストコンポーネントをお勧めします。

*Content ServicesエンドポイントJSONペイロードの例：*\
**[asterts.json](assets/athletes.json)**

## Assets HTTP APIの使用

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

AEM 6.5で初めて導入されたは、Assets HTTP APIを使用したコンテンツフラグメントのサポートを強化しました。 これにより、開発者は、コンテンツフラグメントに対して作成、読み取り、更新、削除(CRUD)操作を簡単に実行できます。

*POSTMANリクエストの例：*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 使用する配信方法

### Web チャネル

AEM Sitesでコンテンツフラグメントコンポーネントを使用すると、Webチャネルを介してコンテンツフラグメントを配信する方法は簡単です。

### ヘッドレス

ヘッドレスの使用例でサードパーティチャネルをサポートするために、コンテンツフラグメントをJSONとして公開するオプションは2つあります。

1. 主な使用例で、サードパーティチャネルによって消費用（読み取り専用）にコンテンツフラグメントを配信する場合は、AEM Content ServicesおよびプロキシAPIページ(ビデオ#2)を使用します。 コンテンツサービスフレームワークは、公開されるデータに関して、より柔軟性とオプションを提供します。 開発者は、コンテンツサービスフレームワークを拡張して、データを拡張したり、エンリッチメントしたりすることもできます。

2. サードパーティチャネルでコンテンツフラグメントの変更や更新が必要な場合は、 Assets HTTP API(ビデオ#3)を使用します。 一般的な使用例は、AEMオーサー環境でサードパーティコンテンツを取り込む場合です。

## その他のリソース {#additional-resources}

* [コンテンツフラグメントのオーサリング](content-fragments-feature-video-use.md)
* [AEM WCMコアコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html)
* [AEM WCMコアコンテンツフラグメントコンポーネント](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)

ビデオシリーズから最終状態のAEM 6.4以降のインスタンスに以下のパッケージをダウンロードしてインストールするには：\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
