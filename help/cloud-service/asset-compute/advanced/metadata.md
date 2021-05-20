---
title: asset computeメタデータワーカーの開発
description: asset computeアセット内で最もよく使用される色を派生させ、色の名前をAEMのアセットのメタデータに書き戻す、画像メタデータワーカーの作成方法を説明します。
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
source-git-commit: dbc0a35ae96594fec1e10f411d57d2a3812c1cf2
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 1%

---


# asset computeメタデータワーカーの開発

カスタムAsset computeワーカーは、XMP(XML)データを生成し、AEMに送り返して、アセットにメタデータとして保存できます。

一般的な使用例を次に示します。

+ 追加のメタデータを取得してアセットに保存する必要があるPIM（Product Information Managementシステム）などのサードパーティシステムとの統合
+ コンテンツやコマースAIなどのAdobeサービスとの統合により、追加の機械学習属性でアセットメタデータを拡張
+ バイナリからアセットに関するメタデータを取得し、アセットメタデータとしてAEMにCloud Serviceとして保存する

## どうするか

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

このチュートリアルでは、Asset computeアセットで最もよく使用される色を引き出し、色の名前をAEMのアセットのメタデータに書き戻す画像メタデータワーカーを作成します。 ワーカー自体は基本的ですが、このチュートリアルでは、ワーカーを使用して、AEM内のCloud Serviceにメタデータを書き戻すためのAsset computeワーカーの使用方法を調べます。

## asset computeメタデータワーカー呼び出しの論理フロー

asset computeメタデータワーカーの呼び出しは、ワーカーを生成する[バイナリレンディション](../develop/worker.md)の呼び出しとほぼ同じです。戻り値のタイプは、値がアセットのメタデータにも書き込まれるXMP(XML)レンディションです。

asset computeワーカーは、Asset computeSDKワーカーAPI契約を、概念上次のような`renditionCallback(...)`関数に実装します。

+ __入力：__ AEMアセットの元のバイナリパラメーターと処理プロファイルパラメーター
+ __出力：__ AEMアセットにレンディションとして保持され、アセットのメタデータに保持されるXMP(XML)レンディション

![asset computeメタデータワーカーの論理フロー](./assets/metadata/logical-flow.png)

1. AEMオーサーサービスは、Asset computeのメタデータワーカーを呼び出し、アセットの&#x200B;__(1a)__&#x200B;元のバイナリと、処理プロファイルで定義された&#x200B;__(1b)__&#x200B;パラメーターを提供します。
1. asset computeSDKは、カスタムAsset computeメタデータワーカーの`renditionCallback(...)`関数の実行を編成し、アセットのバイナリ&#x200B;__(1a)__&#x200B;と処理プロファイルのパラメーター&#x200B;__(1b)__&#x200B;に基づいてXMP(XML)レンディションを導き出します。
1. asset computeワーカーは、XMP (XML)表現を`rendition.path`に保存します。
1. `rendition.path`に書き込まれたXMP(XML)データは、Asset computeSDKを介してAEMオーサーサービスに転送され、__(4a)__&#x200B;テキストレンディションとして公開され、__(4b)__&#x200B;がアセットのメタデータノードに保持されます。

## manifest.ymlの設定{#manifest}

すべてのAsset computeワーカーは、[manifest.yml](../develop/manifest.md)に登録する必要があります。

プロジェクトの`manifest.yml`を開き、新しいワーカーを構成するワーカーエントリを追加します（この場合は`metadata-colors`）。

_空白が `.yml` 区別されることを覚えておいてください。_

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

`function` は、次の手順で作成したワーカーの実装を指 [しています](#metadata-worker)。[ワーカーのURL](#deploy)に示され、[ワーカーのテストスイートフォルダー名](#test)も決定するので、意味的にワーカーに名前を付けます（`actions/worker/index.js`の方が`actions/rendition-circle/index.js`という名前の方が良い場合があります）。

`limits`と`require-adobe-auth`は、ワーカーごとに個別に設定されます。 このワーカーでは、コードが（潜在的に）大きなバイナリイメージデータを検査するために、メモリの`512 MB`が割り当てられます。 その他の`limits`は削除され、デフォルトが使用されます。

## メタデータワーカーの開発{#metadata-worker}

新しいワーカー](#manifest)のパス[定義されたmanifest.ymlのAsset computeプロジェクト内の`/actions/metadata-colors/index.js`に、新しいメタデータワーカーJavaScriptファイルを作成します。

### npmモジュールのインストール

このAsset computeワーカーで使用する追加のnpmモジュール([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions)、[get-image-colors](https://www.npmjs.com/package/get-image-colors)、[color-namer](https://www.npmjs.com/package/color-namer))をインストールします。

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### メタデータワーカーコード

このワーカーは、[レンディション生成ワーカー](../develop/worker.md)とよく似ています。主な違いは、XMP(XML)データを`rendition.path`に書き込んでAEMに保存し直すことです。


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

## メタデータワーカーをローカルで実行します。{#development-tool}

ワーカーコードが完了したら、ローカルの開発ツールを使用してAsset computeを実行できます。

asset computeプロジェクトには2人のワーカー（以前の[circle rendition](../develop/worker.md)とこの`metadata-colors` worker）が含まれるので、[Asset compute開発ツールの](../develop/development-tool.md)プロファイル定義には、両方のワーカーの実行プロファイルが一覧表示されます。 2つ目のプロファイル定義は、新しい`metadata-colors`ワーカーを指します。

![XMLメタデータのレンディション](./assets/metadata/metadata-rendition.png)

1. asset computeプロジェクトのルート
1. `aio app run`を実行して、Asset compute開発ツールを起動します
1. 「__ファイルを選択…__&#x200B;ドロップダウンで、[サンプル画像](../assets/samples/sample-file.jpg)を選択して処理します。
1. `metadata-colors`ワーカーを指す2つ目のプロファイル定義設定で、`"name": "rendition.xml"`を更新します。これは、このワーカーがXMP(XML)レンディションを生成するためです。 必要に応じて、`colorsFamily`パラメーター（サポートされている値`basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`）を追加します。

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

1. __「__&#x200B;を実行」をタップし、XMLレンディションが生成されるのを待ちます。
   + プロファイル定義には両方のワーカーがリストされているので、両方のレンディションが生成されます。 オプションで、[サークルレンディションワーカー](../develop/worker.md)を指す上部のプロファイル定義を削除して、開発ツールから実行しないようにできます。
1. __レンディション__&#x200B;セクションで、生成されたレンディションのプレビューを表示します。 `rendition.xml`をタップしてダウンロードし、VS Code（またはお気に入りのXML/テキストエディター）で開いて確認します。

## ワーカーのテスト{#test}

メタデータワーカーは、バイナリレンディション](../test-debug/test.md)と同じAsset computeテストフレームワークを使用してテストできます。 [唯一の違いは、テストケースの`rendition.xxx`ファイルは、期待されるXMP(XML)レンディションである必要があります。

1. 次の構造をAsset computeプロジェクトに作成します。

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. [サンプルファイル](../assets/samples/sample-file.jpg)をテストケースの`file.jpg`として使用します。
3. 次のJSONを`params.json`に追加します。

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   `"fmt": "xml"`は、`.xml`テキストベースのレンディションを生成するようにテストスイートに指示するために必要です。

4. `rendition.xml`ファイルに、必要なXMLを指定します。 これは、次の方法で取得できます。
   + 開発ツールを使用してテスト入力ファイルを実行し、（検証済みの）XMLレンディションを保存します。

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. asset computeプロジェクトのルートから`aio app test`を実行し、すべてのテストスイートを実行します。

### ワーカーのAdobe I/O Runtimeへのデプロイ{#deploy}

この新しいメタデータワーカーをAEM Assetsから呼び出すには、次のコマンドを使用してAdobe I/O Runtimeにデプロイする必要があります。

```
$ aio app deploy
```

![aioアプリデプロイ](./assets/metadata/aio-app-deploy.png)

これにより、プロジェクト内のすべてのワーカーがデプロイされます。 ステージングワークスペースと実稼動ワークスペースへのデプロイ方法については、デプロイ手順[を完全に確認してください。](../deploy/runtime.md)

### AEM処理プロファイルとの統合{#processing-profile}

新しいを作成するか、このデプロイ済みワーカーを呼び出す既存のカスタム処理プロファイルサービスを変更して、AEMからワーカーを呼び出します。

![処理プロファイル](./assets/metadata/processing-profile.png)

1. __AEM管理者__&#x200B;としてAEM as a Cloud Serviceオーサーサービスにログインします。
1. __ツール/アセット/処理プロファイル__&#x200B;に移動します。
1. ____ 新しい(または編集と既存 ____ の)処理プロファイルの作成
1. 「__カスタム__」タブをタップし、「__新規追加__」をタップします。
1. 新しいサービスの定義
   + __メタデータレンディションを作成__:アクティブに切り替え
   + __エンドポイント：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + これは、[deploy](#deploy)または`aio app get-url`コマンドを使用して取得したワーカーへのURLです。 URLが、AEM as a Workspace環境に基づいて正しいCloud Serviceを指していることを確認します。
   + __サービスパラメーター__
      + 「__パラメーターを追加__」をタップします。
         + キー: `colorFamily`
         + 値：`pantone`
            + サポートされている値：`basic`、`hex`、`html`、`ntc`、`pantone`、`roygbiv`
   + __MIME タイプ__
      + __次を含みます。__ `image/jpeg`、 `image/png`、 `image/gif`  `image/svg`
         + これらは、色の派生に使用されるサードパーティnpmモジュールでサポートされる唯一のMIMEタイプです。
      + __除外：__ `Leave blank`
1. 右上の「__保存__」をタップします
1. AEM Assetsフォルダーに処理プロファイルを適用する（まだ適用していない場合）

### メタデータスキーマ{#metadata-schema}の更新

色のメタデータを確認するには、画像のメタデータスキーマ上の2つの新しいフィールドを、ワーカーが入力する新しいメタデータデータプロパティにマッピングします。

![メタデータスキーマ](./assets/metadata/metadata-schema.png)

1. AEMオーサーサービスで、__ツール/アセット/メタデータスキーマ__&#x200B;に移動します。
1. __default__&#x200B;に移動し、__image__&#x200B;を選択して編集し、生成されたカラーメタデータを表示する読み取り専用フォームフィールドを追加します。
1. __1行のテキスト__&#x200B;を追加します。
   + __フィールドラベル__: `Colors Family`
   + __プロパティにマッピング__: `./jcr:content/metadata/wknd:colorsFamily`
   + __ルール/フィールド/編集を無効にする__:オン
1. __複数値テキスト__&#x200B;を追加します。
   + __フィールドラベル__: `Colors`
   + __プロパティにマッピング__: `./jcr:content/metadata/wknd:colors`
1. 右上の「__保存__」をタップします

## アセットの処理

![アセットの詳細](./assets/metadata/asset-details.png)

1. AEMオーサーサービスで、__Assets/Files__&#x200B;に移動します。
1. フォルダー（サブフォルダー）に移動すると、処理プロファイルが適用されます。
1. 新しい画像（JPEG、PNG、GIFまたはSVG）をフォルダーにアップロードするか、更新された[処理プロファイル](#processing-profile)を使用して既存の画像を再処理します。
1. 処理が完了したら、アセットを選択し、上部のアクションバーの&#x200B;__プロパティ__&#x200B;をタップして、メタデータを表示します
1. カスタムAsset computeメタデータワーカーから書き戻されたメタデータについて、 `Colors Family`と`Colors` [メタデータフィールド](#metadata-schema)を確認します。

アセットのメタデータに書き込まれたカラーメタデータを使用して、`[dam:Asset]/jcr:content/metadata`リソースでこのメタデータを検索を使用して、アセットの検出性を向上させるインデックスが作成されます。__DAMメタデータの書き戻し__&#x200B;ワークフローが呼び出された場合でも、アセットのバイナリに書き戻せます。

### AEM Assetsでのメタデータのレンディション

![AEM Assetsメタデータレンディションファイル](./assets/metadata/cqdam-metadata-rendition.png)

XMPメタデータワーカーによって生成された実際のAsset computeファイルも、アセット上の個別のレンディションとして保存されます。 通常、このファイルは使用されず、アセットのメタデータノードに適用された値が使用されますが、ワーカーからの生のXML出力はAEMで使用できます。

## GitHubのメタデータカラーワーカーコード

最終的な`metadata-colors/index.js`は、GitHubで次の場所から入手できます。

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

最終的な`test/asset-compute/metadata-colors`テストスイートは、GitHubで次の場所から入手できます。

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
