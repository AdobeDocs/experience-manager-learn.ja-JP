---
title: asset computeワーカーをAdobe I/O Runtimeにデプロイして、AEM as a Cloud Serviceで使用する
description: asset computeプロジェクトとそれに含まれるワーカーは、AEM as a Cloud Serviceで使用するために、Adobe I/O Runtimeにデプロイする必要があります。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# Adobe I/O Runtimeにデプロイ

asset computeプロジェクトとそれに含まれるワーカーは、AEMのas a Cloud Serviceで使用するAdobe I/OCLI を介してAdobe I/O Runtimeにデプロイする必要があります。

AEM as a Cloud Serviceオーサーサービスで使用するためにAdobe I/O Runtimeにデプロイする場合は、次の 2 つの環境変数のみが必要です。

+ `AIO_runtime_namespace` は、デプロイ先の App Builder Workspace を指します。
+ `AIO_runtime_auth` は、App Builder ワークスペースの認証資格情報です

他の標準変数 `.env` ファイルは、AEM as a Cloud ServiceがAsset compute・ワーカーを起動する際に暗黙的に提供されます。

## 開発ワークスペース

このプロジェクトは `aio app init` の使用 `Development` ワークスペース `AIO_runtime_namespace` は自動的にに設定されます `81368-wkndaemassetcompute-development` 一致する `AIO_runtime_auth` 私たちの地域で `.env` ファイル。  次の場合、 `.env` ファイルは、OS レベルの変数書き出しで置き換えられる場合を除き、deploy コマンドを発行するために使用されるディレクトリに存在します。 [ステージと実稼動](#stage-and-production) ワークスペースがターゲットになります。

![.env 変数を使用した aio アプリデプロイ](./assets/runtime/development__aio.png)

プロジェクトで定義されたワークスペースにデプロイするには `.env` ファイル：

1. asset computeプロジェクトのルートでコマンドラインを開く
1. コマンドを実行します。 `aio app deploy`
1. コマンドを実行します。 `aio app get-url` を使用して、AEMas a Cloud Service処理プロファイルでこのカスタムAsset computeワーカーを参照するためのワーカー URL を取得します。 プロジェクトに複数のワーカーが含まれる場合、各ワーカーの個別の URL がリストされます。

ローカル開発およびAEMas a Cloud Service開発環境で別々のAsset computeデプロイメントを使用する場合、as a Cloud Serviceの開発環境へのデプロイメントは、 [ステージング環境および実稼動環境へのデプロイメント](#stage-and-production).

## ステージングワークスペースと実稼動ワークスペース{#stage-and-production}

ステージング環境および実稼動環境へのデプロイは、通常、選択した CI/CD システムでおこないます。 asset computeプロジェクトは、個別に各 Workspace（ステージングおよび実稼動）にデプロイする必要があります。

true の環境変数を設定すると、 `.env`.

![書き出し変数を使用した aio アプリのデプロイ](./assets/runtime/stage__export-and-aio.png)

ステージ環境および実稼動環境にデプロイする場合の一般的なアプローチは、通常、CI/CD システムで自動化されます。

1. 次を確認します。 [Adobe I/OCLI npm モジュールおよびAsset computeプラグイン](../set-up/development-environment.md#aio) インストール済み
1. Git からデプロイするAsset computeプロジェクトを確認します
1. 環境変数に、ターゲットワークスペース（ステージングまたは実稼動）に対応する値を設定します
   + 次の 2 つの必須変数があります。 `AIO_runtime_namespace` および `AIO_runtime_auth` とは、Workspace の __すべてダウンロード__ 機能。

![Adobe開発者コンソール — AIO ランタイムの名前空間と認証](./assets/runtime/stage-auth-namespace.png)

これらのキーの値は、コマンドラインから export コマンドを発行することで設定できます。

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

asset computeワーカーがクラウドストレージなど、他の変数を必要とする場合は、それらを環境変数としてエクスポートする必要があります。

1. ターゲットワークスペースのデプロイ先となるすべての環境変数を設定したら、次の deploy コマンドを実行します。
   + `aio app deploy`
1. AEM as a Cloud Service Processing Profile で参照されるワーカー URL は、次の場所からも使用できます。
   + `aio app get-url`

asset computeプロジェクトのバージョンを変更すると、ワーカーの URL も新しいバージョンを反映するように変更され、URL を処理プロファイルで更新する必要があります。

## Workspace API のプロビジョニング{#workspace-api-provisioning}

条件 [Adobe I/Oでの App Builder プロジェクトの設定](../set-up/app-builder.md) ローカル開発をサポートするために、新しい開発ワークスペースが作成され、 __asset compute、I/O イベント__ および __I/O イベント管理 API__ が追加されました。

この __asset compute、I/O イベント__ および __I/O イベント管理 API__ APIS は、ローカル開発に使用するワークスペースにのみ明示的に追加されます。 （排他的に）AEMas a Cloud Service環境と統合するワークスペースは、次の操作を実行します。 __not__ API がAEM as a Cloud Serviceで自然に利用可能になるので、これらの API を明示的に追加する必要があります。
