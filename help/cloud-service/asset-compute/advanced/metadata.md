---
title: asset computeメタデータワーカーの開発
description: asset computeアセット内で最も一般的に使用される色を派生させ、色の名前をAEM内のアセットのメタデータに書き戻す画像メタデータワーカーの作成方法を説明します。
feature: asset computeマイクロサービス
topics: metadata, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
topic: 統合、開発
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: 2efb7050b0b0c783c5f34c1f2e375cf21fa7a6cd
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 1%

---


# asset computeメタデータワーカーの開発

カスタムAsset computeワーカーは、AEMに送り返され、アセットのメタデータとして保存されるXMP(XML)データを生成できます。

一般的な使用例を次に示します。

+ 追加のメタデータを取得してアセットに保存する必要があるPIM(Product Information Management System)などのサードパーティ製システムとの統合
+ コンテンツやコマースAIなどのAdobeサービスとの統合により、追加の機械学習属性を使用してアセットメタデータを拡張
+ バイナリからアセットに関するメタデータを取得し、AEMでアセットメタデータとしてCloud Serviceとして保存する

## 今後の作業

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

このチュートリアルでは、Asset computeアセットで最も一般的に使用される色を派生させ、色の名前をAEMのアセットのメタデータに書き戻す画像メタデータワーカーを作成します。 ワーカー自体は基本的ですが、このチュートリアルでは、ワーカーを使用して、AEM内のアセットにCloud Serviceとしてメタデータを書き戻すAsset computeワーカーを使用する方法を検討します。

## asset computeメタデータワーカー呼び出しの論理フロー

asset computeメタデータワーカーの呼び出しは、ワーカーを生成する[バイナリレンディション](../develop/worker.md)の呼び出しとほぼ同じです。主な違いは、戻り値の型がXMP(XML)レンディションで、値もアセットのメタデータに書き込まれます。

asset computeワーカーは、Asset computeSDKのワーカーAPI契約を`renditionCallback(...)`関数に実装します。この関数は概念上次のとおりです。

+ __入力：AEMアセット__ の元のバイナリパラメーターと処理プロファイルパラメーター
+ __出力：AEMアセット__ にレンディションとして保持され、アセットのメタデータに保存されるXMP(XML)レンディション

![asset computeメタデータワーカーの論理フロー](./assets/metadata/logical-flow.png)

1. AEM Authorサービスは、Asset computeメタデータワーカーを呼び出し、アセットの&#x200B;__(1a)__&#x200B;元のバイナリと&#x200B;__(1b)__&#x200B;処理プロファイルで定義されたパラメーターを提供します。
1. asset computeSDKは、アセットのバイナリ&#x200B;__(1a)__&#x200B;と処理プロファイルのパラメーター&#x200B;__(1b)__&#x200B;に基づいて、カスタムAsset computeメタデータワーカーの`renditionCallback(...)`関数の実行を調整し、XMP(XML)レンディションを導き出します。
1. asset computeワーカーはXMP(XML)表現を`rendition.path`に保存します。
1. `rendition.path`に書き込まれたXMP(XML)データは、Asset computeSDKを介してAEM Author Serviceに転送され、__(4a)__&#x200B;テキストレンディションとして公開され、__(4b)__&#x200B;はアセットのメタデータノードに持続します。

## manifest.yml{#manifest}を設定

すべてのAsset computeワーカーは、[manifest.yml](../develop/manifest.md)に登録する必要があります。

プロジェクトの`manifest.yml`を開き、新しいワーカーを構成するワーカーエントリを追加します（この場合は`metadata-colors`）。

_覚えてお `.yml` くと、空白が区別されます。_

```
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
      metadata-colors:
        function: actions/metadata-colors/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          memorySize: 512 # in MB   
```

`function` は、 [次の手順で作成したワーカー実装を指します](#metadata-worker)。[ワーカーのURL](#deploy)に示すように、ワーカーに意味的に名前を付け（例えば、`actions/worker/index.js`は`actions/rendition-circle/index.js`という名前を付けた方が良い）、[ワーカーのテストスイートフォルダー名](#test)も判断します。

`limits`と`require-adobe-auth`は、ワーカーごとに個別に設定されます。 このワーカーでは、大きなバイナリイメージデータを検査する（潜在的に）コードとしてメモリの`512 MB`が割り当てられます。 他の`limits`はデフォルトを使用するために削除されます。

## メタデータワーカーの開発{#metadata-worker}

新しいワーカー](#manifest)のパス[定義済みのmanifest.ymlにあるAsset computeプロジェクト(`/actions/metadata-colors/index.js`)に、新しいメタデータワーカーのJavaScriptファイルを作成します

### npmモジュールのインストール

このAsset computeワーカーで使用する追加のnpmモジュール([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions)、[get-image-colors](https://www.npmjs.com/package/get-image-colors)、[color-namer](https://www.npmjs.com/package/color-namer))をインストールします。

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### メタデータワーカーコード

このワーカーは、[レンディション生成ワーカー](../develop/worker.md)と非常に似ています。主な違いは、XMP(XML)データを`rendition.path`に書き込んでAEMに保存する点です。


```javascript
"use strict";

const { worker, SourceCorruptError } = require("@adobe/asset-compute-sdk");
const fs = require("fs").promises;

// Require the @adobe/asset-compute-xmp module to create XMP 
const { serializeXmp } = require("@adobe/asset-compute-xmp");

// Require supporting npm modules to derive image colors from image data
const getColors = require("get-image-colors");
// Require supporting npm modules to convert image colors to color names
const namer = require("color-namer");

exports.main = worker(async (source, rendition, params) => {
  // Perform any necessary source (input) checks
  const stats = await fs.stat(source.path);
  if (stats.size === 0) {
    // Throw appropriate errors whenever an erring condition is met
    throw new SourceCorruptError("source file is empty");
  }
  const MAX_COLORS = 10;
  const DEFAULT_COLORS_FAMILY = 'basic';

  // Read the color family parameter to use to derive the color names
  let colorsFamily = rendition.instructions.colorsFamily || DEFAULT_COLORS_FAMILY;

  if (['basic', 'hex', 'html', 'ntc', 'pantone', 'roygbiv'].indexOf(colorsFamily) === -1) { 
      colorsFamily = DEFAULT_COLORS_FAMILY;
  }
  
  // Use the `get-image-colors` module to derive the most common colors from the image
  let colors = await getColors(source.path, { options: MAX_COLORS });

  // Convert the color Chroma objects to their closest names
  let colorNames = colors.map((color) => getColorName(colorsFamily, color));

  // Serialize the data to XMP metadata
  // These properties are written to the [dam:Asset]/jcr:content/metadata resource
  // This stores
  // - The list of color names is stored in a JCR property named `wknd:colors`
  // - The colors family used to derive the color names is stored in a JCR property named `wknd:colorsFamily`
  const xmp = serializeXmp({
      // Use a Set to de-duplicate color names
      "wknd:colors": [...new Set(colorNames)],
      "wknd:colorsFamily": colorsFamily
    }, {
      // Define any property namespaces used in the above property/value definition
      // These namespaces will be automatically registered in AEM if they do not yet exist
      namespaces: {
        wknd: "https://wknd.site/assets/1.0/",
      },
    }
  );

  // Save the XMP metadata to be written back to the asset's metadata node
  await fs.writeFile(rendition.path, xmp, "utf-8");
});

/**
 * Helper function that derives the closest color name for the color, based on the colors family
 * 
 * @param {*} colorsFamily the colors name family to use
 * @param {*} color the color to convert to a name
 */
function getColorName(colorsFamily, color) {
    if ('hex' === colorsFamily) {  return color; }

    let names = namer(color.rgb())[colorsFamily];

    if (names.length >= 1) { return names[0].name; }
}
```

## メタデータワーカーをローカルで実行{#development-tool}

ワーカーコードが完了すると、ローカルAsset compute開発ツールを使用して実行できます。

当社のAsset computeプロジェクトには2人のワーカー（以前の[サークルのレンディション](../develop/worker.md)とこの`metadata-colors`ワーカー）が含まれるので、[Asset compute開発ツールの](../develop/development-tool.md)プロファイル定義リストの実行プロファイルは両方のワーカーに対して行われます。 2つ目のプロファイル定義は、新しい`metadata-colors`ワーカーを指します。

![XMLメタデータレンダリング](./assets/metadata/metadata-rendition.png)

1. asset computeプロジェクトのルートから
1. `aio app run`を実行してAsset compute開発ツールを開始します
1. __ファイルを選択…__&#x200B;ドロップダウン、処理する[サンプル画像](../assets/samples/sample-file.jpg)を選択
1. 2番目のプロファイル定義の設定（`metadata-colors`ワーカーを指す）で、`"name": "rendition.xml"`を更新します。これは、このワーカーがXMP(XML)レンディションを生成するときに行います。 必要に応じて、`colorsFamily`パラメーター（サポートされている値`basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`）を追加します。

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.xml",
               "colorsFamily": "pantone"
           }
       ]
   }
   ```

1. __「Run__」をタップし、XMLレンディションが生成されるのを待ちます
   + 両方のワーカーがプロファイル定義に表示されるので、両方のレンディションが生成されます。 必要に応じて、[サークルレンディションのワーカー](../develop/worker.md)を指すトッププロファイル定義を削除して、開発ツールから実行しないようにできます。
1. __レンディション__&#x200B;セクションには、生成されたレンディションがプレビューされます。 `rendition.xml`をタップしてダウンロードし、VSコード（またはお気に入りのXML/テキストエディター）で開いて確認します。

## ワーカー{#test}をテストします。

メタデータワーカーは、バイナリレンディション](../test-debug/test.md)と同じAsset computeテストフレームワーク[を使用してテストできます。 唯一の違いは、テストケースの`rendition.xxx`ファイルは、期待されるXMP(XML)レンディションである必要があります。

1. asset computeプロジェクトに次の構造を作成します。

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. [サンプルファイル](../assets/samples/sample-file.jpg)をテストケースの`file.jpg`として使用します。
3. 追加`params.json`に対する次のJSONです。

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   `"fmt": "xml"`は、テストスイートに`.xml`テキストベースのレンディションを生成するように指示するために必要です。

4. `rendition.xml`ファイルに、必要なXMLを指定します。 これは、次の方法で取得できます。
   + 開発ツールでテスト入力ファイルを実行し、（検証済みの）XMLレンディションを保存します。

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. asset computeプロジェクトのルートから`aio app test`を実行し、すべてのテストスイートを実行します。

### ワーカーをAdobe I/O Runtime{#deploy}に配置します

この新しいメタデータワーカーをAEM Assetsから呼び出すには、次のコマンドを使用して、Adobe I/O Runtimeに配置する必要があります。

```
$ aio app deploy
```

![aioアプリのデプロイ](./assets/metadata/aio-app-deploy.png)

これにより、プロジェクト内のすべてのワーカーが配置されます。 StageおよびProductionワークスペースに展開する方法については、[完全な展開手順](../deploy/runtime.md)を確認してください。

### AEM処理プロファイルとの統合{#processing-profile}

新しいを作成するか、このデプロイ済みワーカーを呼び出す既存のカスタム処理プロファイルサービスを変更して、AEMからワーカーを呼び出します。

![処理プロファイル](./assets/metadata/processing-profile.png)

1. __AEM管理者__&#x200B;としてAEMにCloud Service作成者サービスとしてログインします
1. __ツール/アセット/処理プロファイル__&#x200B;に移動します。
1. __処理プロファイルを__ 新規作成、または ____ 編集して既存に作成する
1. 「__カスタム__」タブをタップし、「__追加新規__」をタップします
1. 新しいサービスの定義
   + __メタデータレンディションを作成__:アクティブに切り替え
   + __エンドポイント：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + これは、[deploy](#deploy)またはコマンド`aio app get-url`の使用中に取得されたワーカーのURLです。 URLが、Cloud Service環境ーとしてのAEMに基づく正しいワークスペースを指していることを確認します。
   + __サービスパラメーター__
      + __追加パラメータ__&#x200B;をタップします
         + キー: `colorFamily`
         + 値：`pantone`
            + サポートされる値：`basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`
   + __MIME タイプ__
      + __次を含む：__ `image/jpeg`、 `image/png`、 `image/gif`、  `image/svg`
         + 色の派生に使用されるサードパーティのnpmモジュールでサポートされるMIMEタイプはこれらのみです。
      + __除外：__ `Leave blank`
1. 右上の「__保存__」をタップします
1. 処理プロファイルーをAEM Assetsフォルダーに適用する（まだ適用していない場合）

### メタデータスキーマの更新{#metadata-schema}

色のメタデータをレビューするには、画像のメタデータスキーマ上の2つの新しいフィールドを、ワーカーが入力する新しいメタデータプロパティにマップします。

![メタデータスキーマ](./assets/metadata/metadata-schema.png)

1. AEM Authorサービスで、__ツール/アセット/メタデータスキーマ__&#x200B;に移動します。
1. __デフォルト__&#x200B;に移動し、__画像__&#x200B;を選択して編集し、読み取り専用フォームフィールドを追加して、生成された色のメタデータを公開します
1. 追加&#x200B;__1行テキスト__
   + __フィールドラベル__: `Colors Family`
   + __プロパティにマッピング__: `./jcr:content/metadata/wknd:colorsFamily`
   + __ルール/フィールド/編集を無効にする__:チェック済み
1. 追加&#x200B;__複数値テキスト__
   + __フィールドラベル__: `Colors`
   + __プロパティにマッピング__: `./jcr:content/metadata/wknd:colors`
1. 右上の「__保存__」をタップします

## アセットの処理

![アセットの詳細](./assets/metadata/asset-details.png)

1. AEM Authorサービスで、__アセット/ファイル__&#x200B;に移動します。
1. フォルダー（サブフォルダー）に移動すると、「処理」プロファイルが
1. 新しい画像（JPEG、PNG、GIFまたはSVG）をフォルダーにアップロードするか、更新された[処理プロファイル](#processing-profile)を使用して既存の画像を再処理します。
1. 処理が完了したら、アセットを選択し、上部のアクションバーの&#x200B;__プロパティ__&#x200B;をタップしてメタデータを表示します
1. カスタムAsset computeメタデータワーカーから書き戻されたメタデータについて、`Colors Family`と`Colors` [メタデータフィールド](#metadata-schema)を確認します。

色メタデータがアセットのメタデータに書き込まれた状態で、`[dam:Asset]/jcr:content/metadata`リソース上で、検索を使用して、このメタデータのインデックスが追加されたアセット検出機能を使用して作成されます。また、__DAM Metadata Writeback__&#x200B;ワークフローが呼び出された場合にも、アセットのバイナリに書き戻すことができます。

### AEM Assetsでのメタデータレンディション

![AEM Assetsメタデータレンディションファイル](./assets/metadata/cqdam-metadata-rendition.png)

asset computeメタデータワーカーによって生成された実際のXMPファイルも、アセット上の個別のレンディションとして保存されます。 このファイルは一般に使用されず、アセットのメタデータノードに適用された値が使用されますが、ワーカーからの生のXML出力はAEMで使用できます。

## Github上のmetadata-colorsワーカーコード

最終版`metadata-colors/index.js`はGithubで次の場所で入手できます。

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

最終的な`test/asset-compute/metadata-colors`テストスイートは、Githubで次の場所で入手できます。

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
