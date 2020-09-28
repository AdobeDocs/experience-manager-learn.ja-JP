---
title: アセットコンピューティングワーカーを、AEMをCloud Serviceとして使用するためにAdobe I/O Runtimeに導入
description: 'アセットコンピューティングプロジェクトとそのプロジェクトに含まれるワーカーは、AEMがCloud Serviceとして使用するためにAdobe I/O Runtimeにデプロイする必要があります。 '
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
translation-type: tm+mt
source-git-commit: 50519b9526182b528047069f211498099e3a4c88
workflow-type: tm+mt
source-wordcount: '649'
ht-degree: 0%

---


# Adobe I/O Runtimeに展開

アセット計算プロジェクトとそのプロジェクトに含まれるワーカーは、AEMがCloud Serviceとして使用するために、AdobeI/O CLIを介してAdobe I/O Runtimeに導入する必要があります。

AEMでCloud Service作成者サービスとして使用するためにAdobe I/O Runtimeに展開する場合は、2つの環境変数が必要です。

+ `AIO_runtime_namespace` adobeプロジェクトのFirefelly Workspaceが
+ `AIO_runtime_auth` は、AdobeProject Firefly workspaceの認証資格情報です

この `.env` ファイルに定義されている他の標準変数は、AEMがAsset Compute Workerを呼び出すときにCloud Serviceとして暗黙的に提供されます。

## 開発ワークスペース

このプロジェクトは `aio app init` Workspaceを使用して生成されたので、ローカル `Development` ファイルの一致に `AIO_runtime_namespace` 基づいて自動的にに設定され `81368-wkndaemassetcompute-development``AIO_runtime_auth``.env` ます。  deployコマンドの発行に使用されるディレクトリにファイルが存在する場合、OSレベルの変数書き出しで置き換えられる場合を除き、その値が使用されます。これは、ス `.env` テージワークスペースと実稼働 [](#stage-and-production) ワークスペースをターゲットにする方法です。

![.env変数を使用したaioアプリのデプロイ](./assets/runtime/development__aio.png)

プロジェクト `.env` ファイル内のワークスペース定義に配備するには：

1. アセット計算アプリケーションプロジェクトのルートにあるコマンドラインを開きます
1. コマンドを実行します。 `aio app deploy`
1. このコマンドを実行 `aio app get-url` して、AEMでCloud Service処理プロファイルとして使用するワーカーURLを取得し、このカスタムAsset Compute Workerを参照します。 プロジェクトに複数のワーカーが含まれる場合、各ワーカーの個別のURLが表示されます。

ローカル開発およびAEMをCloud Service開発環境として使用する場合は、個別のアセットコンピューティングデプロイメントを使用し、AEMへのデプロイメントを、 [ステージデプロイメントと実稼働デプロイメントと同じ方法で管理できます](#stage-and-production)。

## ステージと実稼働のワークスペース{#stage-and-production}

ステージおよび実稼働環境への展開は、通常、選択したCI/CDシステムで行います。 アセット計算プロジェクトは、個別に各ワークスペース（ステージおよび実稼働）に展開する必要があります。

true環境変数を設定すると、の同じ名前の変数の値が上書きされ `.env`ます。

![エクスポート変数を使用したaioアプリのデプロイ](./assets/runtime/stage__export-and-aio.png)

一般的なアプローチは、通常、CI/CDシステムによって自動化され、ステージおよび実稼働環境に展開する場合は次のようになります。

1. AdobeI/O CLI npmモジュールとAsset Computeプラグインがインストールされている [ことを確認します](../set-up/development-environment.md#aio) 。
1. Gitから展開するAsset Computeアプリケーションをチェックアウトします
1. 環境変数を、ターゲットワークスペース（ステージまたは実稼動）に対応する値で設定します
   + 必要な2つの変数は、ワークスペースごとに、Workspaceの「 `AIO_runtime_namespace` Download All `AIO_runtime_auth`____ 」機能を使用して、AdobeI/O Developer Consoleのワークスペースごとに取得されます。

![Adobe開発者コンソール — AIOランタイム名前空間と認証](./assets/runtime/stage-auth-namespace.png)

これらのキーの値は、コマンドラインからexportコマンドを発行することで設定できます。

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

アセット計算ワーカーがクラウドストレージなど、他の変数を必要とする場合、これらの変数も環境変数としてエクスポートする必要があります。

1. ターゲットワークスペースのデプロイ先となる環境変数がすべて設定されたら、デプロイコマンドを実行します。
   + `aio app deploy`
1. AEMがCloud Service処理プロファイルとして参照するワーカーURLは、次の場所からも利用できます。
   + `aio app get-url`.

「Asset Compute」アプリケーションのバージョンが変更されると、ワーカーURLも新しいバージョンを反映するように変更され、そのURLは「処理」プロファイルで更新する必要があります。

## Workspace APIプロビジョニング{#workspace-api-provisioning}

AdobeプロジェクトFireflyプロジェクトをAdobeI/O [に](../set-up/firefly.md) 設定してローカル開発をサポートする場合、新しい開発ワークスペースが作成され、 __アセット計算、I/Oイベント__ 、 ____ I/Oイベント管理APIが追加されました。

ア __セット計算、I/Oイベント__ 、 __I/Oイベント管理API__ APIは、ローカル開発に使用されるワークスペースにのみ明示的に追加されます。 Cloud Service環境としてAEMに統合（排他的）されるワークスペースには __、APIとして明示的に追加されたこれらのAPIは__ 必要ありません。AEMでは、Cloud Serviceとして自動的に使用できます。
