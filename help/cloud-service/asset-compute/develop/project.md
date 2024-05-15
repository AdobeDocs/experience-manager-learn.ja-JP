---
title: Asset Compute 拡張機能に対応する Asset Compute プロジェクトの作成
description: Asset Compute プロジェクトは、Adobe I/O CLI を使用して生成される Node.js プロジェクトであり、Adobe I/O Runtime へのデプロイと AEM as a Cloud Service との統合を可能にする特定の構造に準拠しています。
jira: KT-6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
duration: 177
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 100%

---

# Asset Compute プロジェクトの作成

Asset Compute プロジェクトは Adobe I/O CLI を使用して生成される Node.js プロジェクトであり、Adobe I/O Runtime へのデプロイと AEM as a Cloud Service との統合を可能にする特定の構造に準拠しています。1 つの Asset Compute プロジェクトに 1 つ以上の Asset Compute ワーカーを含め、それぞれに、AEM as a Cloud Service の処理プロファイルから参照可能な個別の HTTP エンドポイントを持たせることができます。

## プロジェクトの生成

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_Asset Compute プロジェクト生成のクリックスルー（音声なし）_

[Adobe I/O CLI Asset Compute プラグイン](../set-up/development-environment.md#aio-cli)を使用して、空の Asset Compute プロジェクトを新しく生成します。

1. コマンドラインから、プロジェクトを格納するフォルダーに移動します。
1. コマンドラインから `aio app init` を実行して、インタラクティブなプロジェクト生成 CLI を開始します。
   + このコマンドで web ブラウザーが開かれて、Adobe I/O への認証を求めるメッセージが表示される場合があります。その場合は、[必要なアドビサービスおよび製品](../set-up/accounts-and-services.md)に関連付けられているアドビ資格情報を指定します。ログインできない場合は、[プロジェクトの生成方法に関する手順](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user)に従ってください。
1. __組織を選択します__。
   + App Builder が登録されている AEM as a Cloud Service があるアドビ組織を選択します。
1. __プロジェクトを選択します__。
   + プロジェクトを見つけて選択します。これは、App Builder のプロジェクトテンプレートから作成された[プロジェクトタイトル](../set-up/app-builder.md)（ここでは `WKND AEM Asset Compute`）です。
1. __ワークスペースを選択します__。
   + `Development` ワークスペースを選択します。
1. __このプロジェクトに対して、どの Adobe I/O アプリ機能を有効にしますか。組み込むコンポーネントを選択します。__
   + `Actions: Deploy runtime actions` を選択します。
   + 矢印キーを使用して選択し、スペースキーを押して選択解除／選択を切り替え、Enter キーを押して選択を確定します。
1. __生成するアクションのタイプを選択します__。
   + `DX Asset Compute Worker v1` を選択します。
   + 矢印キーを使用して選択し、スペースを押して選択解除／選択を切り替え、Enter キーを押して選択を確定します。
1. __このアクションに名前を付けます__。
   + デフォルト名 `worker` を使用します。
   + 異なるアセット計算を行う複数のワーカーがプロジェクトに含まれている場合は、それらに意味のある名前を付けます。

## console.json の生成

開発者ツールには、Adobe I/O への接続に必要な資格情報が含まれている `console.json` という名前のファイルが必要です。このファイルは、Adobe I/O コンソールからダウンロードします。

1. Asset Compute ワーカーの [Adobe I/O](https://console.adobe.io) プロジェクトを開きます。
1. 資格情報を含んだ `console.json` をダウンロードするプロジェクトワークスペース（ここでは `Development`）を選択します。
1. Adobe I/O プロジェクトのルートに移動し、右上隅の「__すべてダウンロード__」をタップします。
1. ファイルが、プロジェクトとワークスペースを接頭辞とする `.json` ファイル（例：`wkndAemAssetCompute-81368-Development.json`）としてダウンロードされます。
1. 次のいずれかの操作を行えます。
   + ファイル名を `console.json` に変更し、Asset Compute ワーカープロジェクトのルートに移動します。これが、このチュートリアルのアプローチです。
   + ファイルを任意のフォルダーに移動し、設定エントリ `ASSET_COMPUTE_INTEGRATION_FILE_PATH` を持つ `.env` ファイルからそのフォルダーを参照します。ファイルパスは、絶対パスでも、プロジェクトのルートからの相対パスでも構いません。次に例を示します。
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

     または
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`

> メモ
> このファイルには資格情報が含まれています。プロジェクト内にファイルを保存する場合は、必ず `.gitignore` ファイルに追加して共有されないようにします。同じことが `.env` ファイルにも当てはまります。これらの資格情報ファイルを共有したり、Git に保存したりしないでください。

## GitHub 上の Asset Compute プロジェクト

最終的な Asset Computeプロジェクトは、GitHub の次の場所から入手できます。

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub にはプロジェクトの最終状態が含まれ、ワーカーとテストケースが完全に入力されますが、資格情報（つまり、`.env`、`console.json`、`.aio`）は含まれません。_
