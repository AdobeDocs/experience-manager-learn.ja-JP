---
title: asset compute拡張用のAsset computeプロジェクトの作成
description: asset computeプロジェクトは、Adobe I/OCLIを使用して生成されるNode.jsプロジェクトで、Adobe I/O RuntimeにデプロイしてAEM as aCloud Serviceと統合できる特定の構造に準拠します。
feature: asset computeマイクロサービス
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
topic: 統合、開発
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '909'
ht-degree: 4%

---


# asset computeプロジェクト

asset computeプロジェクトは、Adobe I/OCLIを使用して生成されるNode.jsプロジェクトで、Adobe I/O RuntimeにデプロイしてAEM as aCloud Serviceと統合できる特定の構造に準拠します。 1つのAsset computeプロジェクトに1つ以上のAsset computeワーカーを含め、それぞれがAEMからCloud Service処理プロファイルとして参照可能な個別のHTTPエンドポイントを持つことができます。

## プロジェクトの生成

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_asset computeプロジェクトの生成のクリックスルー（オーディオなし）_

[Adobe I/OCLIAsset computeプラグイン](../set-up/development-environment.md#aio-cli)を使用して、新しい空のAsset computeプロジェクトを生成します。

1. コマンドラインから、プロジェクトを含むフォルダに移動します。
1. コマンドラインから`aio app init`を実行し、インタラクティブなプロジェクト生成CLIを開始します。
   + このコマンドは、認証を求めるWebブラウザを起動する場合があります。Adobe I/Oその場合は、必要なAdobeサービスと製品](../set-up/accounts-and-services.md)に関連付けられたAdobe資格情報を提供します。 [ログインできない場合は、以下の手順に従って、プロジェクト](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user)を生成します。[
1. __組織を選択__
   + AEMをAdobeとして持つCloud Service組織を選択します。Project Fireflyは次の場所に登録されています。
1. __プロジェクトを選択__
   + プロジェクトを探して選択します。 これは、Fireflyプロジェクトテンプレート（この場合は`WKND AEM Asset Compute`）から作成された[プロジェクトタイトル](../set-up/firefly.md)です。
1. __ワークスペースの選択__
   + `Development`ワークスペースを選択します。
1. __このプロジェクトに対して有効にするAdobe I/Oアプリ機能を選択してください。含めるコンポーネントを選択__
   +  `Actions: Deploy runtime actions`
   + 矢印キーを使用して選択および選択解除するにはスペースを選択し、選択を確定するにはEnterキーを使用します。
1. __生成するアクションのタイプの選択__
   +  `Adobe Asset Compute Worker`
   + 矢印キーを使用して選択し、選択を解除または選択する間隔、選択を確定するにはEnterキーを使用します。
1. __このアクションの名前を指定してください。__
   + デフォルトの名前`worker`を使用します。
   + 異なるアセット計算を実行する複数のワーカーがプロジェクトに含まれている場合は、意味的に名前を付けます

## console.jsonを生成します

開発者ツールには、Adobe I/Oに接続するために必要な資格情報を含む、`console.json`という名前のファイルが必要です。このファイルは、Adobe I/Oコンソールからダウンロードします。

1. asset computeワーカーの[Adobe I/O](https://console.adobe.io)プロジェクトを開きます。
1. `console.json`資格情報をダウンロードするプロジェクトワークスペースを選択します。この場合は`Development`を選択します。
1. Adobe I/Oプロジェクトのルートに移動し、右上隅の「__すべて__&#x200B;をダウンロード」をタップします。
1. ファイルは、次のように、プロジェクトとワークスペースのプレフィックスが付いた`.json`ファイルとしてダウンロードされます。`wkndAemAssetCompute-81368-Development.json`
1. 次のいずれかの操作を実行できます。
   + ファイルの名前を`config.json`に変更し、Asset computeワーカープロジェクトのルートに移動します。 これは、このチュートリアルのアプローチです。
   + 任意のフォルダーに移動し、設定エントリ`ASSET_COMPUTE_INTEGRATION_FILE_PATH`を使用して`.env`ファイルからそのフォルダーを参照します。 ファイルパスは、絶対パスまたはプロジェクトのルートを基準とした相対パスにすることができます。 以下に例を示します。
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      または
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> 注意
>  ファイルには資格情報が含まれています。プロジェクト内にファイルを保存する場合は、共有されないように、`.gitignore`ファイルにファイルを追加してください。 `.env`ファイルにも同じことが当てはまります。これらの資格情報ファイルを共有したり、Gitに保存したりすることはできません。

## プロジェクトの詳細な構造を確認する

生成されるAsset computeプロジェクトは、Project Firefly専用のAdobeプロジェクトとして使用されるNode.jsプロジェクトです。 次の構造要素は、プロジェクトに固有のAsset computeです。

+ `/actions` にはサブフォルダーが含まれ、各サブフォルダーにはAsset computeワーカーが定義されます。
   + `/actions/<worker-name>/index.js` は、このワーカーの作業を実行するために使用するJavaScriptを定義します。
      + フォルダー名`worker`はデフォルトで、`manifest.yml`に登録されている限り、任意の名前を指定できます。
      + 必要に応じて`/actions`の下に複数のワーカーフォルダーを定義できますが、`manifest.yml`に登録する必要があります。
+ `/test/asset-compute` には、各ワーカーのテストスイートが含まれます。`/actions`フォルダーと同様に、`/test/asset-compute`には複数のサブフォルダーを含めることができ、それぞれがテストするワーカーに対応します。
   + `/test/asset-compute/worker`は、特定のワーカーのテストスイートを表し、特定のテストケースを表すサブフォルダーと、テスト入力、パラメーター、予想される出力が含まれます。
+ `/build` には、テストケース実行時の出力、ログおよびAsset computeのアーティファクトが含まれます。
+ `/manifest.yml` プロジェクトが提供するAsset computeワーカーを定義します。各ワーカー実装をAEMでCloud Serviceとして使用できるようにするには、このファイルにワーカー実装を列挙する必要があります。
+ `/console.json` Adobe I/O設定の定義
   + このファイルは、`aio app use`コマンドを使用して生成/更新できます。
+ `/.aio` aio CLIツールで使用される設定が含まれます。
   + このファイルは、`aio app use`コマンドを使用して生成/更新できます。
+ `/.env` は構文で環境変数を定義 `key=value` し、共有しないシークレットを含みます。これらのシークレットを保護するために、このファイルはGitにチェックインしないでください。また、プロジェクトのデフォルトの`.gitignore`ファイルでは無視されます。
   + このファイルは、`aio app use`コマンドを使用して生成/更新できます。
   + このファイルで定義された変数は、コマンドラインで[変数](../deploy/runtime.md)を書き出すことで上書きできます。

プロジェクト構造のレビューの詳細については、「[AdobeProject Fireflyプロジェクトの分析](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)」を参照してください。

開発の大部分は、ワーカー実装を開発する`/actions`フォルダーと、カスタムAsset computeワーカーのテストを記述する`/test/asset-compute`フォルダーで行われます。

## asset computeプロジェクト(GitHub)

最終的なAsset computeプロジェクトは、GitHubで次の場所から入手できます。

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHubには、プロジェクトの最終状態が含まれ、ワーカーとテストケースが完全に入力されますが、資格情報( `.env`、または)は含まれ `console.json` ません `.aio`。_

