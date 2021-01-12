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
source-git-commit: 676d4bfceaaec3ae8d4feb9f66294ec04e1ecd2b
workflow-type: tm+mt
source-wordcount: '772'
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
   + これにより、Adobe I/Oへの認証を求めるウェブブラウザが生成される場合があります。ログインしている場合は、[必要なAdobeサービスと製品](../set-up/accounts-and-services.md)に関連付けられたAdobe資格情報を提供してください。 ログインできない場合は、[プロジェクトの生成方法に関する次の手順に従ってください](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user)。
1. __組織の選択__
   + AEMをCloud Serviceとして持つAdobe組織を選択します。Project Fireflyは次の場所に登録されます。
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

新しく作成したAsset computeプロジェクトのルートから、次のコマンドを実行して`console.json`を生成します。

```
$ aio app use
```

現在のワークスペースの詳細が正しいことを確認し、`Y`を押すか、enterを押して`console.json`を生成します。 `.env`と`.aio`が既に存在すると検出された場合は、`x`をタップして作成をスキップします。

新しい`.env`を作成するか、または&lt;a0/>を上書きする場合は、見つからないキーや値を新しい`.env`に再度追加します。

```
## please provide the following environment variables for the Asset Compute devtool. You can use AWS or Azure, not both:
#ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=
#S3_BUCKET=
#AWS_ACCESS_KEY_ID=
#AWS_SECRET_ACCESS_KEY=
#AWS_REGION=
#AZURE_STORAGE_ACCOUNT=
#AZURE_STORAGE_KEY=
#AZURE_STORAGE_CONTAINER_NAME=
```

## プロジェクトの構造を確認する

生成されるAsset computeプロジェクトは、特殊なAdobeのProject Fireflyプロジェクト用のNode.jsプロジェクトです。以下は、Asset computeプロジェクトに固有のものです。

+ `/actions` にはサブフォルダが含まれ、各サブフォルダはAsset compute作業者を定義します。
   + `/actions/<worker-name>/index.js` は、このワーカーの作業を実行するために実行するJavaScriptを定義します。
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
+ `/.env` 環境変数を `key=value` 構文で定義し、共有しないシークレットを含めます。これは生成できます。または、これらのシークレットを保護するために、このファイルはGitにチェックインしないでください。プロジェクトのデフォルトの`.gitignore`ファイルを使用して無視されます。
   + このファイルは、`aio app use`コマンドを使用して生成/更新できます。
   + このファイルで定義されている変数は、コマンドラインで[変数](../deploy/runtime.md)を書き出すことで上書きできます。

プロジェクト構造のレビューの詳細については、「[Adobeプロジェクトの構造](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)」を参照してください。

開発の大部分は、開発中のワーカー実装を開発する`/actions`フォルダー、およびカスタムAsset computeワーカーのテストを作成する`/test/asset-compute`フォルダーで行われます。

## asset computeプロジェクトオンギトブ

最終的なAsset computeプロジェクトは、次の場所でGithubで利用できます。

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Githubにはプロジェクトの最後の状態が含まれ、ワーカーとテストケースが完全に入力されますが、資格情報は含まれません。`.env`、 `console.json` または `.aio`。_

