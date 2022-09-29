---
title: asset computeメタデータワーカーの開発
description: asset computeアセット内で最もよく使用される色を派生し、色の名前をAEMのアセットのメタデータに書き戻す、画像メタデータワーカーの作成方法を説明します。
feature: Asset Compute Microservices
topics: metadata, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1433'
ht-degree: 1%

---

# asset computeメタデータワーカーの開発

カスタムAsset computeワーカーは、XMP(XML) データを生成し、AEMに送り返して、アセットにメタデータとして保存することができます。

一般的な使用例を次に示します。

+ 追加のメタデータを取得してアセットに保存する必要がある PIM(Product Information Management System) などのサードパーティシステムとの統合
+ Content や Commerce AI などのAdobe サービスとの統合により、追加の機械学習属性でアセットメタデータを拡張
+ アセットに関するメタデータをバイナリから取得し、AEM as a Cloud Serviceにアセットメタデータとして保存する

## 今後の予定

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

このチュートリアルでは、Asset computeアセット内で最もよく使用される色を派生させ、色の名前をAEMのアセットのメタデータに書き戻す画像メタデータワーカーを作成します。 ワーカー自体は基本的ですが、このチュートリアルでは、ワーカーを使用して、AEM as a Cloud ServiceでAsset computeにメタデータを書き戻すために、アセットワーカーがどのように使用できるかを調べます。

## asset computeメタデータワーカー呼び出しの論理フロー

asset computeメタデータワーカーの呼び出しは、 [ワーカーを生成するバイナリレンディション](../develop/worker.md)に値を書き込む場合、戻り値のタイプはXMP(XML) レンディションになります。このレンディションの値はアセットのメタデータにも書き込まれます。

asset computeワーカーは、 `renditionCallback(...)` 関数の呼び出しは、概念的には次のようになります。

+ __入力：__ AEMアセットの元のバイナリパラメーターと処理プロファイルパラメーター
+ __出力：__ XMP(XML) レンディションは、AEMアセットをレンディションとして、アセットのメタデータに保持されます。

![asset computeメタデータワーカーの論理フロー](./assets/metadata/logical-flow.png)

1. AEM オーサーサービスは、Asset computeのメタデータワーカーを呼び出し、アセットの __(1a)__ 元のバイナリ、および __(1b)__ 処理プロファイルで定義されたパラメーター。
1. asset computeSDK は、カスタムAsset computeメタデータワーカーの実行を調整します。 `renditionCallback(...)` 関数、アセットのバイナリに基づくXMP(XML) レンディションの派生 __(1a)__ および任意の処理プロファイルパラメーター __(1b)__.
1. asset computeワーカーは、XMP (XML) 表現をに保存します。 `rendition.path`.
1. に書き込まれるXMP(XML) データ `rendition.path` は、Asset computeSDK を介して AEM オーサーサービスに転送され、 __(4a)__ テキストレンディションと __(4b)__ アセットのメタデータノードに保持されます。

## manifest.yml の設定{#manifest}

すべてのAsset computeワーカーは、 [manifest.yml](../develop/manifest.md).

プロジェクトの `manifest.yml` 次に、新しいワーカーを構成するワーカーエントリを追加します。この場合は、 `metadata-colors`.

_記憶する `.yml` は空白を区別します。_

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

`function` は、 [次の手順](#metadata-worker). 意味的に workers に名前を付ける ( 例： `actions/worker/index.js` 名前の方が良かったかもしれない `actions/rendition-circle/index.js`) で、 [ワーカーの URL](#deploy) また、 [ワーカーのテストスイートのフォルダ名](#test).

この `limits` および `require-adobe-auth` は、ワーカーごとに個別に設定されます。 この労働者は `512 MB` のメモリは、コードが（潜在的に）大きなバイナリイメージデータを調べる際に割り当てられます。 もう 1 つは `limits` が削除され、デフォルトが使用されます。

## メタデータワーカーの開発{#metadata-worker}

新しいメタデータワーカー JavaScript ファイルを、Asset computeプロジェクトのパスに作成します。 [新しいワーカー用に定義された manifest.yml](#manifest)、 `/actions/metadata-colors/index.js`

### npm モジュールのインストール

追加の npm モジュール ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors)、および [色名](https://www.npmjs.com/package/color-namer)) をクリックします。

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### メタデータワーカーコード

この作業者は、 [レンディション生成作業者](../develop/worker.md)の場合、主な違いは、XMP(XML) データを `rendition.path` をクリックしてAEMに保存し直します。


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
      // These namespaces are automatically registered in AEM if they do not yet exist
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

## メタデータワーカーをローカルで実行する{#development-tool}

ワーカーコードが完了したら、ローカルの開発ツールを使用してAsset computeを実行できます。

これは、Asset computeプロジェクトに 2 人のワーカー ( [円レンディション](../develop/worker.md) そして `metadata-colors` 作業者 )、 [asset compute開発ツールの](../develop/development-tool.md) プロファイル定義は、両方のワーカーの実行プロファイルをリストします。 2 つ目のプロファイル定義は、新しい `metadata-colors` 作業者

![XML メタデータのレンディション](./assets/metadata/metadata-rendition.png)

1. asset computeプロジェクトのルートから
1. 実行 `aio app run` asset compute開発ツールを開始する
1. 内 __ファイルを選択…__ ドロップダウン、 [サンプル画像](../assets/samples/sample-file.jpg) 処理する
1. 2 つ目のプロファイル定義設定では、 `metadata-colors` ワーカー、更新 `"name": "rendition.xml"` XMP (XML) レンディションが生成されるので、 オプションで、 `colorsFamily` パラメーター（サポートされている値） `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`) をクリックします。

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

1. タップ __実行__ XML レンディションが生成されるのを待ちます。
   + 両方のワーカーはプロファイル定義にリストされているので、両方のレンディションが生成されます。 オプションで、 [円レンディションワーカー](../develop/worker.md) を削除して、開発ツールから実行しないようにすることができます。
1. この __レンディション__ セクションでは、生成されたレンディションのプレビューが表示されます。 次をタップします。 `rendition.xml` をダウンロードし、VS Code（またはお気に入りの XML/テキストエディター）で開いて確認します。

## ワーカーをテスト{#test}

メタデータワーカーは、 [バイナリレンディションと同じAsset computeテストフレームワーク](../test-debug/test.md). 唯一の違いは `rendition.xxx` テストケースのファイルは、想定されるXMP (XML) レンディションである必要があります。

1. asset computeプロジェクトで次の構造を作成します。

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. 以下を使用： [サンプルファイル](../assets/samples/sample-file.jpg) テストケースの `file.jpg`.
3. 次の JSON を `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   次の点に注意してください。 `"fmt": "xml"` を生成するようにテストスイートに指示するには、が必要です。 `.xml` テキストベースのレンディション。

4. に適切な XML を指定 `rendition.xml` ファイル。 これは、次の方法で取得できます。
   + 開発ツールを使用してテスト入力ファイルを実行し、（検証済みの）XML レンディションを保存します。

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. 実行 `aio app test` をAsset computeプロジェクトのルートから選択し、すべてのテストスイートを実行します。

### ワーカーをAdobe I/O Runtimeにデプロイ{#deploy}

この新しいメタデータワーカーをAEM Assetsから呼び出すには、次のコマンドを使用して、Adobe I/O Runtimeにデプロイする必要があります。

```
$ aio app deploy
```

![aio アプリデプロイ](./assets/metadata/aio-app-deploy.png)

これは、プロジェクト内のすべてのワーカーをデプロイします。 以下を確認します。 [完全展開命令](../deploy/runtime.md) を参照してください。

### AEM処理プロファイルとの統合{#processing-profile}

新しいを作成するか、このデプロイ済みのワーカーを呼び出す既存のカスタム処理プロファイルサービスを変更して、AEMからワーカーを呼び出します。

![処理プロファイル](./assets/metadata/processing-profile.png)

1. AEM as a Cloud Service Author サービスに as a としてログイン __AEM Administrator__
1. に移動します。 __ツール/ Assets /処理プロファイル__
1. __作成__ 新しい、または __編集__ および既存、処理プロファイル
1. 次をタップします。 __カスタム__ タブで、 __新規追加__
1. 新しいサービスを定義
   + __メタデータレンディションを作成__:アクティブに切り替え
   + __エンドポイント：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + これは、 [デプロイ](#deploy) またはコマンドを使用 `aio app get-url`. URL が、AEM as a Cloud Service環境に基づいて正しいワークスペースを指していることを確認します。
   + __サービスパラメーター__
      + タップ __パラメータを追加__
         + キー: `colorFamily`
         + 値：`pantone`
            + サポートされている値： `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __MIME タイプ__
      + __次を含む：__ `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + これらは、色の派生に使用されるサードパーティの npm モジュールでサポートされる唯一の MIME タイプです。
      + __除外：__ `Leave blank`
1. タップ __保存__ 右上に
1. AEM Assetsフォルダーに処理プロファイルを適用する（まだ適用していない場合）

### メタデータスキーマの更新{#metadata-schema}

色のメタデータを確認するには、画像のメタデータスキーマ上の 2 つの新しいフィールドを、ワーカーが入力する新しいメタデータデータプロパティにマッピングします。

![メタデータスキーマ](./assets/metadata/metadata-schema.png)

1. AEM オーサーサービスで、に移動します。 __ツール/アセット/メタデータスキーマ__
1. に移動します。 __デフォルト__ を選択し、編集します。 __画像__ 生成されたカラーメタデータを公開するための読み取り専用フォームフィールドを追加します。
1. を追加します。 __1 行のテキスト__
   + __フィールドラベル__: `Colors Family`
   + __プロパティにマッピング__: `./jcr:content/metadata/wknd:colorsFamily`
   + __ルール/フィールド/編集を無効にする__:確認済み
1. を追加します。 __複数値テキスト__
   + __フィールドラベル__: `Colors`
   + __プロパティにマッピング__: `./jcr:content/metadata/wknd:colors`
1. タップ __保存__ 右上に

## アセットを処理中

![アセットの詳細](./assets/metadata/asset-details.png)

1. AEM オーサーサービスで、に移動します。 __アセット/ファイル__
1. そのフォルダー（サブフォルダー）に移動すると、処理プロファイルが適用されます。
1. 新しい画像 (JPEG、PNG、GIFまたはSVG) をフォルダーにアップロードするか、更新された [処理プロファイル](#processing-profile)
1. 処理が完了したら、アセットを選択し、 __プロパティ__ 上部のアクションバーにメタデータを表示
1. 以下を確認します。 `Colors Family` および `Colors` [メタデータフィールド](#metadata-schema) カスタムAsset computeメタデータワーカーから書き戻されたメタデータ用。

アセットのメタデータに書き込まれたカラーメタデータが `[dam:Asset]/jcr:content/metadata` リソース、このメタデータは、検索を通じてこれらの用語を使用して、より高いアセット発見性を示すインデックスが作成されます。インデックスが作成された場合、アセットのバイナリに書き戻すこともできます。 __DAM メタデータの書き戻し__ ワークフローが呼び出されます。

### AEM Assetsでのメタデータのレンディション

![AEM Assetsメタデータレンディションファイル](./assets/metadata/cqdam-metadata-rendition.png)

asset computeメタデータワーカーによって生成された実際のXMPファイルも、アセット上の個別のレンディションとして保存されます。 通常、このファイルは使用されず、アセットのメタデータノードに適用された値が使用されますが、ワーカーからの生の XML 出力はAEMで使用できます。

## GitHub でのメタデータ色のワーカーコード

最終 `metadata-colors/index.js` は、GitHub で以下の場所から入手できます。

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

最終 `test/asset-compute/metadata-colors` テストスイートは GitHub で次の場所から入手できます。

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
