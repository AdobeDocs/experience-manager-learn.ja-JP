---
title: asset compute拡張用のAsset computeプロジェクトの作成
description: asset computeプロジェクトは、Adobe I/OCLI を使用して生成される Node.js プロジェクトで、Adobe I/O RuntimeにデプロイしてAEMas a Cloud Serviceと統合できる特定の構造に準拠します。
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
source-git-commit: 839152aa67ba7ab2929f2c8093bfdc873761a645
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 2%

---

# asset computeプロジェクト

asset computeプロジェクトは、Adobe I/OCLI を使用して生成される Node.js プロジェクトで、Adobe I/O RuntimeにデプロイしてAEMas a Cloud Serviceと統合できる特定の構造に準拠します。 1 つのAsset computeプロジェクトに 1 つ以上のAsset computeワーカーを含め、それぞれがAEMas a Cloud Serviceの処理プロファイルから参照可能な個別の HTTP エンドポイントを持つことができます。

## プロジェクトを生成

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_asset computeプロジェクト生成のクリックスルー（音声なし）_

以下を使用： [Adobe I/OCLIAsset computeプラグイン](../set-up/development-environment.md#aio-cli) 新しい空のAsset computeプロジェクト

1. コマンドラインから、プロジェクトを含むフォルダに移動します。
1. コマンドラインから、を実行します。 `aio app init` をクリックして、インタラクティブなプロジェクトの生成 CLI を開始します。
   + このコマンドは、Web ブラウザに対して認証を求めるメッセージを表示するAdobe I/Oを生成します。存在する場合は、 [必要なAdobe サービスと製品](../set-up/accounts-and-services.md). ログインできない場合は、次の手順に従ってください。 [プロジェクトの生成方法に関する以下の手順](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __組織を選択__
   + AEM as a Cloud Serviceが登録されているAdobe組織を選択します。App Builder は
1. __プロジェクトを選択__
   + プロジェクトを探して選択します。 これは [プロジェクトタイトル](../set-up/app-builder.md) App Builder プロジェクトテンプレートから作成。この場合は `WKND AEM Asset Compute`
1. __ワークスペースを選択__
   + を選択します。 `Development` workspace
1. __このプロジェクトに対して有効にするAdobe I/Oアプリの機能を教えてください。 含めるコンポーネントを選択__
   + 選択 `Actions: Deploy runtime actions`
   + 矢印キーを使用して、選択を解除または選択を行い、選択を確定するには Enter キーを使用します。
1. __生成するアクションのタイプを選択__
   + 選択 `DX Asset Compute Worker v1`
   + 矢印キーを使用して選択し、選択を解除または選択するスペース、選択を確定するには Enter キーを使用します
1. __このアクションの名前を指定してください。__
   + デフォルト名を使用 `worker`.
   + 異なるアセット計算を実行する複数のワーカーがプロジェクトに含まれている場合は、意味的に名前を付けます

## console.json を生成

開発者ツールにはという名前のファイルが必要です `console.json` には、Adobe I/Oに接続するために必要な資格情報が含まれています。このファイルは、Adobe I/Oコンソールからダウンロードします。

1. asset computeワーカーの [Adobe I/O](https://console.adobe.io) プロジェクト
1. プロジェクトワークスペースを選択して、 `console.json` の資格情報を選択します。この場合は、を選択します。 `Development`
1. Adobe I/Oプロジェクトのルートに移動し、をタップします。 __すべてダウンロード__ をクリックします。
1. ファイルが `.json` 次のように、プロジェクトとワークスペースのプレフィックスが付いたファイルが表示されます。 `wkndAemAssetCompute-81368-Development.json`
1. 次のいずれかの操作を実行できます。
   + ファイル名をに変更します。 `console.json` をクリックし、Asset computeワーカープロジェクトのルートに移動します。 これは、このチュートリアルのアプローチです。
   + 任意のフォルダーに移動し、 `.env` 設定エントリを持つファイル `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. ファイルパスは、絶対パスまたはプロジェクトのルートを基準とした相対パスにすることができます。 次に例を示します。
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      または
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> 注意
>  ファイルには資格情報が含まれています。プロジェクト内にファイルを保存する場合は、必ず `.gitignore` ファイルを共有しないように設定します。 同じことが `.env` file — これらの資格情報ファイルを共有したり、Git に保存したりしないでください。

## GitHub のasset computeプロジェクト

最終的なAsset computeプロジェクトは、GitHub で次の場所から入手できます。

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub には、プロジェクトの最終状態が含まれ、ワーカーとテストケースが完全に入力されますが、資格情報 ( つまり、 `.env`, `console.json` または `.aio`._
