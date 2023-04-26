---
title: AEMヘッドレスでの最適化された画像の使用
description: AEMヘッドレスを使用して最適化された画像 URL をリクエストする方法について説明します。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
source-git-commit: 97a311e043d3903070cd249d993036b5d88a21dd
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 6%

---

# AEMヘッドレスで画像を最適化 {#images-with-aem-headless}

画像は、 [豊富で魅力的なAEMヘッドレスエクスペリエンスの開発](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=ja). AEMヘッドレスは、画像アセットの管理と、その最適化された配信をサポートします。

AEMヘッドレスコンテンツモデリングで使用されるコンテンツフラグメントは、多くの場合、ヘッドレスエクスペリエンスでの表示を目的とした画像アセットを参照します。 AEM GraphQLクエリを記述して、画像の参照元に基づいて画像に URL を提供できます。

この `ImageRef` タイプには、コンテンツ参照用の 4 つの URL オプションがあります。

+ `_path` はAEMで参照されているパスで、AEM origin（ホスト名）は含まれていません
+ `_dynamicUrl` は、Web に最適化された優先画像アセットの完全な URL です。
   + この `_dynamicUrl` にはAEM接触チャネルが含まれていないので、ドメイン（AEM オーサーまたは AEM パブリッシュサービス）はクライアントアプリケーションによって提供される必要があります。
+ `_authorUrl` は、AEM オーサー上の画像アセットの完全な URL です
   + [AEM オーサー](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) は、ヘッドレスアプリケーションのプレビューエクスペリエンスを提供するために使用できます。
+ `_publishUrl` は、AEM パブリッシュ上の画像アセットの完全な URL です。
   + [AEM パブリッシュ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) は、通常、ヘッドレスアプリケーションの実稼動デプロイメントで画像が表示される場所です。

この `_dynamicUrl` は、画像アセットに使用する推奨 URL で、 `_path`, `_authorUrl`、および `_publishUrl` 可能な限り

|  | AEM as a Cloud Service | AEMas a Cloud ServiceRDE | AEM SDK | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| Web に最適化された画像をサポートしますか？ | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="AEM ヘッドレスによる画像"
>abstract="AEM ヘッドレスが画像アセットの管理とその最適化された配信をどのようにサポートするかについて説明します。"

## コンテンツフラグメントモデル

画像参照を含む「コンテンツフラグメント」フィールドが __コンテンツ参照__ データタイプ。

フィールドタイプについては、 [コンテンツフラグメントモデル](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)、フィールドを選択し、 __プロパティ__ 」タブを右にクリックします。

![画像へのコンテンツ参照を含むコンテンツフラグメントモデル](./assets/images/content-fragment-model.jpeg)

## GraphQL永続クエリ

GraphQLクエリで、フィールドを `ImageRef` を入力し、リクエストします。 `_dynamicUrl` フィールドに入力します。 例えば、 [WKND Site プロジェクト](https://github.com/adobe/aem-guides-wknd) 画像アセット参照用の画像 URL をその中に含める `primaryImage` フィールドに入力し、新しい永続化クエリで実行できます `wknd-shared/adventure-image-by-path` 次のように定義されます。

```graphql {highlight="11"}
query($path: String!, $imageFormat: AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int, $imageQuality: Int) {
  adventureByPath(
    _path: $path
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
    }
  ) {
    item {
      _path
      title
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

### クエリ変数

```json
{ 
  "path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
  "imageFormat": "JPG",
  "imageWidth": 1000,
}
```

この `$path` 変数 `_path` フィルターにはコンテンツフラグメントへの完全パスが必要です ( 例： `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`) をクリックします。

この `_assetTransform` は、 `_dynamicUrl` は、提供される画像レンディションを最適化するように構築されます。 Web に最適化された画像の URL は、URL のクエリパラメーターを変更することで、クライアント上で調整することもできます。

| GraphQLパラメーター | URL パラメーター | 説明 | 必須 | GraphQL変数値 | URL パラメーターの値 | URL パラメーターの例 |
|:---------|:----------|:-------------------------------|:--:|:--------------------------|:---|:--|
| `format` | 該当なし | 画像アセットの形式。 | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`,  `WEBP`, `WEBPLL`, `WEBPLY` | 該当なし | 該当なし |
| `seoName` | 該当なし | URL のファイルセグメントの名前。 指定しなかった場合は、画像アセット名が使用されます。 | ✘ | 英数字、 `-`または `_` | 該当なし | 該当なし |
| `crop` | `crop` | 画像から取り出した切り抜きフレームは、画像のサイズ内にある必要があります | ✘ | 元の画像サイズの範囲内で切り抜き領域を定義する正の整数 | 数値座標のコンマ区切り文字列 `<X_ORIGIN>,<Y_ORIGIN>,<CROP_WIDTH>,<CROP_HEIGHT>` | `?crop=10,20,300,400` |
| `size` | `size` | 出力画像のサイズ（高さと幅の両方）をピクセル単位で指定します。 | ✘ | 正の整数 | カンマ区切りの正の整数（順） `<WIDTH>,<HEIGHT>` | `?size=1200,800` |
| `rotation` | `rotate` | イメージの回転（度単位）。 | ✘ | `R90`、`R180`、`R270` | `90`、`180`、`270` | `?rotate=90` |
| `flip` | `flip` | 画像を反転します。 | ✘ | `HORIZONTAL`、`VERTICAL`、`HORIZONTAL_AND_VERTICAL` | `h`、`v`、`hv` | `?flip=h` |
| `quality` | `quality` | 画質（元の画質のパーセント）。 | ✘ | 1-100 | 1-100 | `?quality=80` |
| `width` | `width` | 出力画像の幅（ピクセル単位）。 条件 `size` が指定されている `width` は無視されます。 | ✘ | 正の整数 | 正の整数 | `?width=1600` |
| `preferWebP` | `preferwebp` | If `true` ブラウザーが WebP をサポートしている場合、AEMは WebP を提供します。 `format`. | ✘ | `true`、`false` | `true`、`false` | `?preferwebp=true` |

## GraphQL応答

結果の JSON 応答には、画像アセットへの Web 最適化 URL を含む要求されたフィールドが含まれます。

```json {highlight="8"}
{
  "data": {
    "adventureByPath": {
      "item": {
        "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
        "title": "Bali Surf Camp",
        "primaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--a38886f7-4537-4791-aa20-3f6ef0ac3fcd/adobestock_175749320.jpg?preferwebp=true&width=1000&quality=80"
        }
      }
    }
  }
}
```

アプリケーションで参照画像の Web 最適化画像を読み込むには、 `_dynamicUrl` の `primaryImage` を画像のソース URL として設定します。

React では、AEM Publish から Web に最適化された画像を表示すると、次のようになります。

```jsx
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

忘れないで下さい `_dynamicUrl` ではAEMドメインが含まれていないので、解決する画像 URL の目的の接触チャネルを指定する必要があります。

## レスポンシブ URL

上記の例は、単一サイズの画像を使用することを示していますが、Web エクスペリエンスでは、レスポンシブ画像セットが必要になる場合が多くあります。 レスポンシブ画像は、 [img srcsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) または [画像要素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). 次のコードスニペットに、 `_dynamicUrl` をベースにし、異なる幅のパラメーターを追加して、異なるレスポンシブビューを強化します。 また、 `width` クエリパラメーターを使用できますが、クライアントは他のクエリパラメーターを追加して、必要に応じて画像アセットをさらに最適化できます。

```javascript
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
// Read the data from GraphQL response
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
let alt = data.adventureByPath.item.title;
...
{/*-- Example img srcset --*/}
document.body.innerHTML=`<img>
    alt="${alt}"
    src="${dynamicUrl}&width=1000}"
    srcset="`
      ${dynamicUrl}&width=1000 1000w,
      ${dynamicUrl}&width=1600 1600w,
      ${dynamicUrl}&width=2000 2000w,
      `"
    sizes="calc(100vw - 10rem)"/>`;
...
{/*-- Example picture --*/}
document.body.innerHTML=`<picture>
      <source srcset="${dynamicUrl}&width=2600" media="(min-width: 2001px)"/>
      <source srcset="${dynamicUrl}&width=2000" media="(min-width: 1000px)"/>
      <img src="${dynamicUrl}&width=400" alt="${alt}"/>
    </picture>`;
```

## React の例

次の手順に従って、Web に最適化された画像を表示するシンプルな React アプリケーションを作成します。 [レスポンシブ画像パターン](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). レスポンシブ画像には、主に次の 2 つのパターンがあります。

+ [srcset を含む Img 要素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) パフォーマンスの向上
+ [画像要素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) 設計管理のため

### srcset を含む Img 要素

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[srcset を含む img 要素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) を `sizes` 属性を使用して、画面サイズごとに異なる画像アセットを指定します。 Img srcsets は、画面サイズごとに異なる画像アセットを提供する場合に役立ちます。

### 画像要素

[画像要素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) 複数の `source` 要素を使用して、画面サイズごとに異なる画像アセットを提供します。 Picture 要素は、画面サイズごとに異なる画像レンディションを提供する場合に役立ちます。

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### サンプルコード

この簡単な React アプリでは、 [AEMヘッドレス SDK](./aem-headless-sdk.md) アドベンチャーコンテンツにAEMヘッドレス API を照会し、 [srcset を含む img 要素](#img-element-with-srcset) および [画像要素](#picture-element). この `srcset` および `sources` カスタムを使用 `setParams` 関数を使用して、web 最適化された配信クエリパラメーターを `_dynamicUrl` 画像のレンディションを変更する必要があるので、web クライアントのニーズに基づいて配信される画像レンディションを変更します。

AEMに対するクエリは、カスタム React フックで実行されます。 [AEMヘッドレス SDK を使用する useAdventureByPath](./aem-headless-sdk.md#graphql-persisted-queries).

```javascript
// src/App.js

import "./App.css";
import { useAdventureByPath } from './api/persistedQueries'

const AEM_HOST = process.env.AEM_HOST;

function App() {

  /**
   * Update the dynamic URL with client-specific query parameters
   * @param {*} imageUrl the image URL
   * @param {*} params the AEM web-optimized image query parameters
   * @returns the dynamic URL with the query parameters
   */
  function setOptimizedImageUrlParams(imageUrl, params) {
    let url = new URL(imageUrl);
    Object.keys(params).forEach(key => {
      url.searchParams.set(key, params[key]);
    });
    return url.toString();
  }

  // Get data from AEM using GraphQL persisted query as defined above 
  // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
  // The 2nd parameter define the base GraphQL query parameters used to request the web-optimized image
  let { data, error } = useAdventureByPath(
        "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp", 
        { imageFormat: "JPG" }
      );

  // Wait for AEM Headless APIs to provide data
  if (!data) { return <></> }

  const alt = data.adventureByPath.item.title;
  const imageUrl =  AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;

  return (
    <div className="app">
      
      <h1>Web-optimized images</h1>

      {/* Render the web-optimized image img with srcset for the Adventure Primary Image */}
      <h2>Img srcset</h2>

      <img
        alt={alt}
        src={setOptimizedImageUrlParams(imageUrl, { width: 1000 })}
        srcSet={
            `${setOptimizedImageUrlParams(imageUrl, { width: 1000 })} 1000w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 1600 })} 1600w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 2000 })} 2000w`
        }
        sizes="calc(100vw - 10rem)"/>

       {/* Render the web-optimized picture for the Adventure Primary Image */}
        <h2>Picture element</h2>

        <picture>
          {/* When viewport width is greater than 2001px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2600 })} media="(min-width: 2001px)"/>        
          {/* When viewport width is between 1000px and 2000px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2000})} media="(min-width: 1000px)"/>
          {/* When viewport width is less than 799px */}
          <img src={setOptimizedImageUrlParams(imageUrl, { width : 400, crop: "550,300,400,400" })} alt={alt}/>
        </picture>
    </div>
  );
}

export default App;
```
