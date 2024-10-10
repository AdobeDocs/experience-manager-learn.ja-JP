---
title: Asset Compute メタデータワーカーの開発
description: 画像アセットで最も一般的に使用されている色を取得し、その色の名前を AEM のアセットのメタデータに書き戻す Asset Compute メタデータワーカーの作成方法について説明します。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
duration: 432
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '1405'
ht-degree: 100%

---

# Asset Compute メタデータワーカーの開発

カスタム Asset Compute ワーカーは、AEM に送り返され、アセットにメタデータとして保存される XMP（XML）データを生成できます。

一般的なユースケースを次に示します。

+ 追加のメタデータを取得してアセットに保存する必要がある PIM（製品情報管理システム）などのサードパーティシステムと統合します。
+ Content and Commerce AI などのアドビサービスと統合して、アセットのメタデータを追加の機械学習属性で拡張します。
+ アセットに関するメタデータをバイナリから取得して、AEM as a Cloud Service にアセットメタデータとして保存します。

## 作業の内容

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

このチュートリアルでは、画像アセットで最も一般的に使用されている色を取得し、その色の名前を AEM のアセットのメタデータに書き戻す Asset Compute メタデータワーカーを作成します。このワーカー自体は基本的なものですが、このチュートリアルでは、このワーカーを通して、Asset Compute ワーカーを使用して AEM as a Cloud Service のアセットにメタデータを書き戻す方法について説明します。

## Asset Compute メタデータワーカー呼び出しの論理フロー

Asset Compute メタデータワーカーの呼び出しは、[バイナリレンディション生成ワーカー](../develop/worker.md)の呼び出しとほぼ同じですが、主な違いは、戻り値のタイプが XMP（XML）レンディションであり、その値もアセットのメタデータに書き込まれる点です。

Asset Compute ワーカーは、Asset Compute SDK ワーカー API コントラクトを `renditionCallback(...)` 関数で実装します。この関数は、概念的には次のようなものになります。

+ __入力：__ AEM アセットのオリジナルバイナリパラメーターと処理プロファイルパラメーター
+ __出力：__ AEM アセットにレンディションとして保存され、アセットのメタデータにも保存される XMP（XML）レンディション

![Asset Compute メタデータワーカーの論理フロー](./assets/metadata/logical-flow.png)

1. AEM オーサーサービスが Asset Compute メタデータワーカーを呼び出し、アセットの __(1a)__ オリジナルバイナリと、__(1b)__ 処理プロファイルで定義されているパラメーターを渡します。
1. Asset Compute SDK がカスタム Asset Compute メタデータワーカーの `renditionCallback(...)` 関数の実行を調整し、アセットのバイナリ __(1a)__ と処理プロファイルパラメーター __(1b)__ に基づいて、XMP（XML）レンディションを取得します。
1. Asset Compute ワーカーが XMP（XML）表現を `rendition.path` に保存します。
1. `rendition.path` に書き込まれた XMP（XML）データが、Asset Compute SDK を通じて AEM オーサーサービスに転送され、そこで __(4a)__ テキストレンディションとして公開され、__(4b)__ アセットのメタデータノードに保存されます。

## manifest.yml の設定{#manifest}

すべての Asset Compute ワーカーは、[manifest.yml](../develop/manifest.md) に登録する必要があります。

プロジェクトの `manifest.yml` を開き、新規ワーカー（この場合は `metadata-colors`）を設定するワーカーエントリを追加します。

_`.yml` では空白が区別されることに注意してください。_

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

`function` は、[次の手順](#metadata-worker)で作成するワーカー実装を指します。ワーカーの名前は[ワーカーの URL](#deploy) に表示され、[ワーカーのテストスイートフォルダー名](#test)の決定にも使用されるので、ワーカーには、具体的な意味を持つ名前を付けます（例えば、`actions/worker/index.js` は `actions/rendition-circle/index.js` と名付けた方が適切でしょう）。

`limits` と `require-adobe-auth` は、ワーカーごとに個別に設定されます。このワーカーでは、コードで（潜在的に）大きなバイナリ画像データを調べるので、`512 MB` のメモリが割り当てられます。もう 1 つの `limits` は削除され、デフォルトが使用されます。

## メタデータワーカーの開発{#metadata-worker}

[新しいワーカーの manifest.yml で定義されている](#manifest)パス（`/actions/metadata-colors/index.js`）に Asset Compute プロジェクトの新しいメタデータワーカーの JavaScript ファイルを作成します。

### npm モジュールのインストール

この Asset Compute ワーカーで使用されている追加の npm モジュール（[@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions)、[get-image-colors](https://www.npmjs.com/package/get-image-colors) および [color-namer](https://www.npmjs.com/package/color-namer)）をインストールします。

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### メタデータワーカーコード

このワーカーは、[レンディション生成ワーカー](../develop/worker.md)に非常に似ていますが、主な違いは、XMP（XML）データを `rendition.path` に書き込んで AEM に保存し戻す点です。


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

## メタデータワーカーのローカル実行{#development-tool}

ワーカーコードが完成したら、ローカルの Asset Compute 開発ツールを使用して実行できます。

Asset Compute プロジェクトには 2 つのワーカー（前回の[円のレンディション](../develop/worker.md)と今回の `metadata-colors` ワーカー）が含まれているので、[Asset Compute 開発ツール](../develop/development-tool.md)のプロファイル定義には、両方のワーカーの実行プロファイルがリストされています。2 つ目のプロファイル定義は、新しい `metadata-colors` ワーカーを指しています。

![XML メタデータレンディション](./assets/metadata/metadata-rendition.png)

1. Asset Compute プロジェクトのルートから
1. `aio app run` を実行して、Asset Compute 開発ツールを起動します。
1. 「__ファイルを選択...__」ドロップダウンで、処理する[サンプル画像](../assets/samples/sample-file.jpg)を選択します。
1. 2 つ目のプロファイル定義設定は `metadata-colors` ワーカーを指していますが、このワーカーが XMP（XML）レンディションを生成するので、`"name": "rendition.xml"` を更新します。オプションで、`colorsFamily` パラメーター（サポートされている値は `basic`、`hex`、`html`、`ntc`、`pantone` および `roygbiv`）を追加します。

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

1. 「__実行__」をタップして、XML レンディションが生成されるのを待ちます。
   + 両方のワーカーがプロファイル定義にリストされているので、両方のレンディションが生成されます。オプションで、[円形のレンディションワーカー](../develop/worker.md)を示す上部のプロファイル定義を削除して、開発ツールから実行しないようにすることができます。
1. この「__レンディション__」セクションでは、生成されたレンディションのプレビューが表示されます。「`rendition.xml`」をタップしてダウンロードし、VS Code（またはお好みの XML／テキストエディター）で開いて確認してください。

## ワーカーのテスト{#test}

メタデータワーカーは、[バイナリ レンディションと同じ Asset Compute テストフレームワーク](../test-debug/test.md)を使用してテストすることができます。唯一の違いは、テストケースの `rendition.xxx` ファイルは、期待される XMP（XML）レンディションの必要があることです。

1. Asset Compute プロジェクトで次の構造を作成します。

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. テストケースの `file.jpg` として[サンプルファイル](../assets/samples/sample-file.jpg)を使用します。
3. `params.json` に次の JSON を追加します。

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   `"fmt": "xml"` は、テストスイートが `.xml` テキストベースの表示を生成するよう指示するために必要であることに注意してください。

4. `rendition.xml` ファイルに適切な XML を提供します。これは、次の方法で取得できます。
   + 開発ツールを使用してテスト入力ファイルを実行し、（検証済みの）XML レンディションを保存します。

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Asset Compute プロジェクトのルートから `aio app test` を実行すると、すべてのテストスイートが実行されます。

### ワーカーを Adobe I/O Runtime にデプロイ{#deploy}

この新しいメタデータワーカーを AEM Assets から呼び出すには、次のコマンドを使用して、Adobe I/O Runtime にデプロイする必要があります。

```
$ aio app deploy
```

![aio アプリのデプロイ](./assets/metadata/aio-app-deploy.png)

これは、プロジェクト内のすべてのワーカーをデプロイします。ステージおよび実稼働ワークスペースへのデプロイ方法については、[包括的なデプロイの手順](../deploy/runtime.md)を確認してください。

### AEM 処理プロファイルとの統合{#processing-profile}

デプロイされたワーカーを呼び出すカスタム処理 プロファイルサービスを新規作成するか、既存のものを修正して、AEM からワーカーを呼び出します。

![処理プロファイル](./assets/metadata/processing-profile.png)

1. AEM as a Cloud Service オーサーサービスに __AEM 管理者__&#x200B;としてログイン
1. __ツール／アセット／処理プロファイル__&#x200B;に移動します。
1. ____&#x200B;新規作成または既存の処理プロファイルを&#x200B;__編集__
1. 「__カスタム__」タブをタップし、「__新規追加__」をタップします
1. 新しいサービスを定義します
   + __メタデータレンディションを作成__：アクティブに切り替えます
   + __エンドポイント：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + これは、[デプロイ](#deploy)またはコマンド `aio app get-url` で取得したワーカーへの URL です。URL が、AEM as a Cloud Service 環境に基づいて正しいワークスペースを指していることを確認します。
   + __サービスパラメーター__
      + 「__パラメーターを追加__」をタップします
         + キー：`colorFamily`
         + 値：`pantone`
            + サポートされている値：`basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`
   + __MIME タイプ__
      + __次を含む：__ `image/jpeg`、`image/png`、`image/gif`、`image/svg`
         + これらは、色の派生に使用されるサードパーティの npm モジュールでサポートされる唯一の MIME タイプです。
      + __除外：__ `Leave blank`
1. 右上の「__保存__」をタップ
1. AEM Assets フォルダーに処理プロファイルを適用する（まだ適用していない場合）

### メタデータスキーマの更新{#metadata-schema}

カラーのメタデータを確認するには、画像のメタデータスキーマ上の 2 つの新しいフィールドを、ワーカーが入力する新しいメタデータデータプロパティにマッピングします。

![メタデータスキーマ](./assets/metadata/metadata-schema.png)

1. AEM オーサーサービスで、__ツール／アセット／Metadata Schemas__&#x200B;に移動します。
1. __デフォルト__&#x200B;に移動し、「__画像__」を選択および編集して、生成されたカラーメタデータを公開するための読み取り専用フォームフィールドを追加する
1. __1 行のテキストプロパティを追加する__
   + __フィールドラベル__：`Colors Family`
   + __プロパティにマッピング__：`./jcr:content/metadata/wknd:colorsFamily`
   + __ルール／フィールド／編集を無効にする__：確認済み
1. __複数値テキスト__&#x200B;を追加します
   + __フィールドラベル__：`Colors`
   + __プロパティにマッピング__：`./jcr:content/metadata/wknd:colors`
1. 右上の「__保存__」をタップします

## アセットの処理

![アセットの詳細](./assets/metadata/asset-details.png)

1. AEM オーサーサービスで、__アセット／ファイル__&#x200B;に移動します。
1. 処理プロファイルが適用されるフォルダーまたはサブフォルダーに移動します
1. 新しい画像（JPEG、PNG、GIF または SVG）をフォルダーにアップロードするか、更新された[処理プロファイル](#processing-profile)を使用して、既存の画像を再処理します
1. 処理が完了したら、アセットを選択し、上部のアクションバーの&#x200B;__プロパティ__&#x200B;をタップしてメタデータを表示します
1. カスタム Asset Compute メタデータワーカーから書き戻されたメタデータの `Colors Family` および `Colors` [メタデータフィールド](#metadata-schema)を確認します。

`[dam:Asset]/jcr:content/metadata` リソース上のアセットのメタデータに書き込まれたカラーメタデータは、検索を通じてこれらの用語を使用して、アセットの発見性より高めるためにインデックス化され、__DAM メタデータの書き戻し__&#x200B;ワークフローが実行された場合はアセットのバイナリに書き戻すこともできます。

### AEM Assets でのメタデータのレンディション

![AEM Assets メタデータレンディションファイル](./assets/metadata/cqdam-metadata-rendition.png)

Asset Compute メタデータワーカーによって生成された実際の XMP ファイルも、アセット上の個別のレンディションとして保存されます。通常、このファイルは使用されず、アセットのメタデータノードに適用された値が使用されますが、ワーカーからの生の XML 出力は AEM で使用できます。

## GitHub でのメタデータカラーのワーカーコード

最終的な `metadata-colors/index.js` は、GitHub で次の場所から入手できます。

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

最終的な `test/asset-compute/metadata-colors` テストスイートは GitHub で次の場所から入手できます。

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
