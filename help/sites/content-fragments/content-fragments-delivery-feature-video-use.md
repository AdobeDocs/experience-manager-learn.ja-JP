---
title: AEM でのコンテンツフラグメントの配信
description: コンテンツフラグメントは、レイアウトとは無関係に、コアコンポーネントを使用して AEM Sites で直接使用することも、ヘッドレスでダウンストリームチャネルに配信することもできます。
feature: Content Fragments
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
duration: 878
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '520'
ht-degree: 100%

---

# コンテンツフラグメントの配信 {#delivering-content-fragments}

Adobe Experience Manager（AEM）コンテンツフラグメントは、関連付けられた構造化データ要素を含む場合がある、テキストベースのエディトリアルコンテンツですが、デザイン情報やレイアウト情報のない純粋なコンテンツと見なされます。コンテンツフラグメントは、通常、チャネルに依存しないコンテンツとして作成されます。これは、複数のチャネルで使用および再利用することを目的としており、コンテンツをコンテキスト固有のエクスペリエンスに含めます。

コンテンツフラグメントは、レイアウトとは無関係に、コアコンポーネントを使用して AEM Sites で直接使用することも、ヘッドレスでダウンストリームチャネルに配信することもできます。

このビデオシリーズでは、コンテンツフラグメントを使用するための配信オプションについて説明します。コンテンツフラグメントの定義とオーサリングについて詳しくは、[こちらを参照](content-fragments-feature-video-use.md)してください。

1. Web ページでのコンテンツフラグメントの使用
2. AEM Content Services を使用してコンテンツフラグメントを JSON として公開
3. Assets HTTP API の使用

## Web ページでのコンテンツフラグメントの使用 {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449?quality=12&learn=on)

コンテンツフラグメントは、AEM Sites ページで使用することができ、同様の方法で、AEM WCM コアコンポーネントの[コンテンツフラグメントコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=ja)を使用して、エクスペリエンス フラグメントで使用することもできます。

コンテンツフラグメントコンポーネントは、AEM のスタイルシステムを使用してスタイルを設定し、必要に応じてコンテンツを表示できます。

## コンテンツフラグメントを JSON として公開 {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448?quality=12&learn=on)

AEM コンテンツサービスは、コンテンツを正規化された JSON 形式にレンディションする、AEM ページベースの HTTP エンドポイントの作成を容易にします。

上記のビデオでは、[コンテンツフラグメントコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=ja)を使用して、個々のコンテンツフラグメントを公開します。[コンテンツフラグメントのリストコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html?lang=ja)は新しいコンポーネントであり、コンテンツフラグメントのリストを動的にページに入力するクエリを、オーサーが定義できるようになります。複数のコンテンツフラグメントを公開する必要がある場合は、コンテンツフラグメントのリストコンポーネントをお勧めします。

*コンテンツサービスのエンドポイント JSON ペイロードの例：*\
**[athletes.json](assets/athletes.json)**

## Assets HTTP API の使用

>[!VIDEO](https://video.tv.adobe.com/v/26390?quality=12&learn=on)

AEM 6.5 で初めて導入され、Assets HTTP API を使用してコンテンツフラグメントのサポートを強化しました。これにより、開発者はコンテンツフラグメントに対して、作成、読み取り、更新、削除（CRUD）操作を簡単に実行できます。

*POSTMAN 要求の例：*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 使用する配信方法

### Web チャネル

AEM Sites でコンテンツフラグメントコンポーネントを使用すると、web チャネルを介したコンテンツフラグメントの配信が簡単になります。

### ヘッドレス

ヘッドレスの使用例でサードパーティチャネルをサポートするために、コンテンツフラグメントを JSON として公開する方法は 2 つあります。

1. サードパーティチャネルで消費（読み取り専用）のためにコンテンツフラグメントを配信することが主な使用例である場合は、AEM コンテンツサービスとプロキシ API ページ（ビデオ #2）を使用します。コンテンツサービスのフレームワークにより、どのようなデータを公開するかについて、高い柔軟性と選択肢を提供します。また、開発者は、コンテンツサービスのフレームワークを拡張して、データを補強したり、充実させたりすることができます。

2. サードパーティチャネルでコンテンツフラグメントの変更や更新が必要な場合は、Assets HTTP API（ビデオ #3）を使用します。典型的な使用例は、AEM オーサー環境でサードパーティコンテンツを取り込む場合です。

## その他のリソース {#additional-resources}

* [コンテンツフラグメントのオーサリング](content-fragments-feature-video-use.md)
* [AEM WCM コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)
* [AEM WCM コアコンテンツフラグメントコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=ja)

ビデオシリーズから最終状態の AEM 6.4 以降のインスタンスに以下のパッケージをダウンロードしてインストールするには、次の手順を実行します。\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
