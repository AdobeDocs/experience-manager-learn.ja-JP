---
title: asset computeの拡張機能用のAsset computeプロジェクトの作成
description: asset computeプロジェクトは、Adobe I/OCLIを使用して生成されるNode.jsプロジェクトです。このプロジェクトは、特定の構造に従って、Adobe I/O Runtimeに展開し、AEMとCloud Serviceとして統合することができます。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: 23c91551673197cebeb517089e5ab6591f084846
workflow-type: tm+mt
source-wordcount: '904'
ht-degree: 3%

---


# asset computeプロジェクトの作成

asset computeプロジェクトは、Adobe I/OCLIを使用して生成されるNode.jsプロジェクトです。このプロジェクトは、Adobe I/O Runtimeに展開し、AEMとCloud Serviceとして統合できる特定の構造に従って作成されます。 1つのAsset computeプロジェクトに1人以上のAsset computeワーカーを含めることができます。各ワーカーは、AEMからCloud Service処理プロファイルとして参照可能な、個別のHTTPエンドポイントを持ちます。

## プロジェクトの生成

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_asset computeプロジェクトの生成時のクリックスルー（オーディオなし）_

[Adobe I/OCLIAsset computeプラグイン](../set-up/development-environment.md#aio-cli)を使用して、新しい空のAsset computeプロジェクトを生成します。

1. コマンドラインから、プロジェクトを含むフォルダに移動します。
1. コマンドラインで`aio app init`を実行し、対話型プロジェクトの生成CLIを開始します。
   + このコマンドは、Adobe I/Oに対する認証を要求するWebブラウザを生成する場合があります。ログインしている場合は、[必要なAdobeサービスと製品](../set-up/accounts-and-services.md)に関連付けられたAdobe資格情報を提供してください。 ログインできない場合は、[プロジェクトの生成方法に関する次の手順に従ってください](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user)。
1. __組織の選択__
   + AEMをCloud Serviceとして持つAdobe組織を選択し、Project Fireflyを登録します。
1. __プロジェクトを選択__
   + プロジェクトを探して選択します。 これは、Fireflyプロジェクトテンプレートから作成された[プロジェクトタイトル](../set-up/firefly.md)です。この場合`WKND AEM Asset Compute`
1. __ワークスペースの選択__
   + `Development`ワークスペースを選択
1. __このプロジェクトで有効にするAdobe I/Oアプリ機能を選択してください。__&#x200B;を含めるコンポーネントを選択
   +  `Actions: Deploy runtime actions`
   + 矢印キーを使用して選択と選択を解除/選択の間隔を空け、Enterキーを使用して選択を確認します
1. __生成するアクションのタイプを選択__
   +  `Adobe Asset Compute Worker`
   + 矢印キーを使用して選択し、選択を解除/選択します。選択を確認するにはEnterキーを使用します
1. __このアクションの名前を指定してください。__
   + デフォルト名`worker`を使用します。
   + プロジェクトに、異なる資産計算を実行する複数のワーカーが含まれる場合、意味的に名前を付けます

## console.jsonを生成

開発者ツールには、`console.json`という名前のファイルが必要です。このファイルには、Adobe I/Oへの接続に必要な資格情報が含まれています。このファイルは、Adobe I/Oのコンソールからダウンロードされます。

1. asset computeワーカーの[Adobe I/O](https://console.adobe.io)プロジェクトを開きます
1. `console.json`資格情報をダウンロードするプロジェクトワークスペースを選択します。この場合は`Development`を選択します
1. Adobe I/Oプロジェクトのルートに移動し、右上隅の「すべて&#x200B;__ダウンロード__」をタップします。
1. ファイルは、プロジェクトとワークスペースのプレフィックスが付いた`.json`ファイルとしてダウンロードされます。例：`wkndAemAssetCompute-81368-Development.json`
1. 次のいずれかの操作を実行できます。
   + ファイルの名前を`config.json`に変更し、Asset computeワーカープロジェクトのルートに移動します。 これは、このチュートリアルでのアプローチです。
   + 任意のフォルダーに移動し、`.env`ファイルからそのフォルダーを参照し、設定エントリ`ASSET_COMPUTE_INTEGRATION_FILE_PATH`を指定します。 ファイルパスは、絶対パスまたはプロジェクトのルートを基準とした相対パスにすることができます。 次に例を示します。
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      または
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> 注意
> ファイルには秘密鍵証明書が含まれています。 プロジェクト内にファイルを保存する場合は、`.gitignore`ファイルにファイルを追加して、共有されないようにしてください。 `.env`ファイルにも同じことが言えます。これらの資格情報ファイルを共有したり、Gitに保存したりすることはできません。

## プロジェクトの構造を確認する

生成されるAsset computeプロジェクトは、特殊なAdobeプロジェクトFireflyプロジェクトとして使用されるNode.jsプロジェクトです。 次の構造要素は、Asset computeプロジェクトに固有の要素です。

+ `/actions` サブフォルダーが含まれ、各サブフォルダーはAsset computeワーカーを定義します。
   + `/actions/<worker-name>/index.js` は、このワーカーの作業を実行するために使用するJavaScriptを定義します。
      + フォルダー名`worker`はデフォルトで、`manifest.yml`に登録されている限り、任意の名前を指定できます。
      + 必要に応じて`/actions`に複数のワーカーフォルダーを定義できますが、`manifest.yml`に登録する必要があります。
+ `/test/asset-compute` には、各ワーカーのテストスイートが含まれます。`/actions`フォルダーと同様、`/test/asset-compute`には複数のサブフォルダーを含めることができ、それぞれがテストするワーカーに対応します。
   + `/test/asset-compute/worker`は、特定のワーカーのテストスイートを表し、特定のテストケースを表すサブフォルダーと、テスト入力、パラメーター、期待出力が含まれます。
+ `/build` asset computeテストケース実行の出力、ログおよびアーティファクトが含まれます。
+ `/manifest.yml` プロジェクトが提供するAsset computeワーカーを定義します。AEMがCloud Serviceとして使用できるようにするには、各ワーカー実装をこのファイルに列挙する必要があります。
+ `/console.json` adobe i/o構成を定義する
   + このファイルは、`aio app use`コマンドを使用して生成/更新できます。
+ `/.aio` に、aio CLIツールで使用される設定を示します。
   + このファイルは、`aio app use`コマンドを使用して生成/更新できます。
+ `/.env` 環境変数を `key=value` 構文で定義し、共有しないシークレットを含めます。これらのシークレットを保護するには、このファイルをGitにチェックインしないでください。プロジェクトのデフォルトの`.gitignore`ファイルを使用して無視されます。
   + このファイルは、`aio app use`コマンドを使用して生成/更新できます。
   + このファイルで定義されている変数は、コマンドラインで[変数](../deploy/runtime.md)を書き出すことで上書きできます。

プロジェクト構造のレビューの詳細については、「[Adobeプロジェクトの構造](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)」を参照してください。

開発の大部分は、開発中のワーカー実装を開発する`/actions`フォルダー、およびカスタムAsset computeワーカーのテストを作成する`/test/asset-compute`フォルダーで行われます。

## GitHubでのasset computeプロジェクト

最終的なAsset computeプロジェクトは、次の場所でGitHubで利用できます。

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHubにはプロジェクトの最終状態が含まれ、ワーカーとテストケースが完全に埋め込まれますが、資格情報（または）は含まれま `.env`せん `console.json`  `.aio`。_

