---
title: AEMでのコンテンツフラグメントの配信
seo-title: Delivering Content Fragments in Adobe Experience Manager
description: コンテンツフラグメントは、レイアウトとは無関係に、コアコンポーネントを使用してAEM Sitesで直接使用することも、ヘッドレスな方法でダウンストリームチャネルに配信することもできます。
seo-description: Content Fragments, independent of layout, can be used directly in AEM Sites with Core Components or can be delivered in a headless manner to downstream channels.
sub-product: content-services
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: Content Management
role: User
level: Beginner
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 6%

---

# コンテンツフラグメントの配信 {#delivering-content-fragments}

Adobe Experience Manager(AEM) コンテンツフラグメントは、テキストベースの編集コンテンツで、関連付けられた構造化されたデータ要素を含めることができますが、デザイン情報やレイアウト情報のない純粋なコンテンツと見なされます。 コンテンツフラグメントは、通常、チャネルに依存しないコンテンツとして作成されます。これは、複数のチャネルで使用および再利用することを目的としており、コンテンツをコンテキスト固有のエクスペリエンスに含めます。

コンテンツフラグメントは、レイアウトとは無関係に、コアコンポーネントを使用してAEM Sitesで直接使用することも、ヘッドレスな方法でダウンストリームチャネルに配信することもできます。

このビデオシリーズでは、コンテンツフラグメントを使用するための配信オプションについて説明します。 定義と [コンテンツフラグメントのオーサリングは、こちらを参照してください。](content-fragments-feature-video-use.md).

1. Web ページでのコンテンツフラグメントの使用
2. AEM Content Services を使用した JSON としてのコンテンツフラグメントの公開
3. Assets HTTP API の使用

## Web ページでのコンテンツフラグメントの使用 {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

コンテンツフラグメントは、AEM Sitesページ上で、またはAEM WCM コアコンポーネントの [コンテンツフラグメントコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=ja).

コンテンツフラグメントコンポーネントは、AEMスタイルシステムを使用してスタイルを設定し、必要に応じてコンテンツを表示することができます。

## コンテンツフラグメントを JSON として公開 {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services は、コンテンツを正規化された JSON 形式にレンディションするAEMページベースの HTTP エンドポイントの作成を容易にします。

上記のビデオでは、 [コンテンツフラグメントコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html) 個々のコンテンツフラグメントを公開します。 この [コンテンツフラグメントリストコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html) は、作成者が、コンテンツフラグメントのリストを使用してページに動的に入力するクエリを定義できる新しいコンポーネントです。 複数のコンテンツフラグメントを公開する必要がある場合は、コンテンツフラグメントリストコンポーネントをお勧めします。

*Content Services エンドポイント JSON ペイロードの例：*\
**[astreets.json](assets/athletes.json)**

## Assets HTTP API の使用

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

AEM 6.5 で初めて導入されたは、Assets HTTP API を使用してコンテンツフラグメントのサポートを強化しました。 これにより、開発者はコンテンツフラグメントに対して作成、読み取り、更新、削除 (CRUD) 操作を簡単に実行できます。

*POSTMAN Requests の例：*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 使用する配信方法

### Web チャネル

Web チャネルを介してコンテンツフラグメントを配信する方法は、AEM Sitesでコンテンツフラグメントコンポーネントを使用することで簡単です。

### ヘッドレス

ヘッドレスの使用例でサードパーティチャネルをサポートするために、コンテンツフラグメントを JSON として公開する方法は 2 つあります。

1. 主な使用例がサードパーティチャネルで消費（読み取り専用）のためにコンテンツフラグメントを配信する場合は、AEM Content Services とプロキシ API ページ ( ビデオ#2) を使用します。 コンテンツサービスフレームワークは、公開されるデータに関して、より柔軟性とオプションを提供します。 開発者は、コンテンツサービスフレームワークを拡張して、データを拡張したり、エンリッチメントしたりすることもできます。

2. サードパーティチャネルでコンテンツフラグメントの変更や更新が必要な場合は、 Assets HTTP API( ビデオ#3) を使用します。 典型的な使用例は、AEMオーサー環境でサードパーティコンテンツを取り込む場合です。

## その他のリソース {#additional-resources}

* [コンテンツフラグメントのオーサリング](content-fragments-feature-video-use.md)
* [AEM WCM コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)
* [AEM WCM コアコンテンツフラグメントコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)

ビデオシリーズから最終状態のAEM 6.4 以降のインスタンスに以下のパッケージをダウンロードしてインストールするには、次の手順を実行します。\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
