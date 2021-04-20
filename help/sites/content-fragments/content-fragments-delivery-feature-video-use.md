---
title: AEMでのコンテンツフラグメントの配信
seo-title: コンテンツフラグメントのAdobe Experience Managerでの配信
description: コンテンツフラグメントは、レイアウトとは無関係に、コアコンポーネントを使用してAEM Sitesで直接使用することも、ヘッドレスな方法で下流のチャネルに配信することもできます。
seo-description: コンテンツフラグメントは、レイアウトとは無関係に、コアコンポーネントを使用してAEM Sitesで直接使用することも、ヘッドレスな方法で下流のチャネルに配信することもできます。
sub-product: content-services
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 7%

---


# コンテンツフラグメントの配信{#delivering-content-fragments}

Adobe Experience Manager(AEM)コンテンツフラグメントは、関連付けられた構造化データ要素の一部を含む場合があり、デザインやレイアウト情報を含まない純粋なコンテンツと見なされる、テキストベースの編集コンテンツです。 コンテンツフラグメントは通常、チャネルにとらわれないコンテンツとして作成されます。これは複数のチャネルで使用および再利用され、その後コンテンツをコンテキスト固有のエクスペリエンスにまとめることを目的としています。

コンテンツフラグメントは、レイアウトとは無関係に、コアコンポーネントを使用してAEM Sitesで直接使用することも、ヘッドレスな方法で下流のチャネルに配信することもできます。

このビデオシリーズでは、コンテンツフラグメントの使用に関する配信オプションについて説明します。 コンテンツフラグメントの定義と[オーサリングについて詳しくは、](content-fragments-feature-video-use.md)を参照してください。

1. Webページでのコンテンツフラグメントの使用
2. AEM Content Servicesを使用したJSONとしてのコンテンツフラグメントの公開
3. アセットHTTP APIの使用

## Webページでのコンテンツフラグメントの使用{#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

コンテンツフラグメントは、AEM Sitesのページで使用することも、同様の方法で、AEM WCMコアコンポーネント[コンテンツフラグメントコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/components/content-fragment-component.html)を使用して使用することもできます。

コンテンツフラグメントコンポーネントは、AEMスタイルシステムを使用してスタイル設定し、必要に応じてコンテンツを表示することができます。

## コンテンツフラグメントのJSONとしての公開{#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Servicesは、コンテンツを正規化されたJSON形式にレンダリングするAEMページベースのHTTPエンドポイントの作成を容易にします。

上記のビデオでは、[コンテンツフラグメントコンポーネント](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)を使用して、個々のコンテンツフラグメントを公開しています。 [コンテンツフラグメントリストコンポーネント](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html)は、コンテンツフラグメントのリストを使用してページに動的に埋め込むクエリを作成者が定義できる新しいコンポーネントです。 複数のコンテンツフラグメントを公開する必要がある場合は、コンテンツフラグメントリストコンポーネントをお勧めします。

*Content ServicesエンドポイントJSONペイロードの例：*\
**[aspreets.json](assets/athletes.json)**

## アセットHTTP APIの使用

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

AEM 6.5で初めて導入された、Assets HTTP APIを使用したコンテンツフラグメントのサポートが強化されました。 これにより、開発者はコンテンツフラグメントに対して作成、読み取り、更新、削除(CRUD)操作を簡単に実行できます。

*POSTMANリクエストの例：*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 使用する配信方法

### Web チャネル

コンテンツフラグメントをWebチャネル経由で配信する方法は、コンテンツフラグメントコンポーネントとAEM Sitesを使用することで簡単です。

### ヘッドレス

ヘッドレスの使用例でサードパーティのチャネルをサポートするために、コンテンツフラグメントをJSONとして公開する方法は2つあります。

1. サードパーティのチャネルが消費（読み取り専用）用にコンテンツフラグメントを配信する場合は、AEM Content ServicesおよびProxy APIページ（ビデオ#2）を使用します。 Content Servicesフレームワークは、公開されるデータに関する柔軟性とオプションを提供します。 開発者は、Content Servicesフレームワークを拡張して、データを拡張したり、拡張したりすることもできます。

2. サードパーティチャネルがコンテンツフラグメントを変更または更新する必要がある場合は、アセットHTTP API（ビデオ#3）を使用します。 一般的な使用例としては、AEM作成者環境にサードパーティのコンテンツを取り込む場合があります。

## その他のリソース {#additional-resources}

* [コンテンツフラグメントのオーサリング](content-fragments-feature-video-use.md)
* [AEM WCMコアコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html)
* [AEM WCMコアコンテンツフラグメントコンポーネント](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)

ビデオシリーズから最終状態のAEM 6.4以降のインスタンスに対して、以下のパッケージをダウンロードしてインストールするには：\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
