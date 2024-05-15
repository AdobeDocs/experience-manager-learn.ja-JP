---
title: Asset Compute ワーカーを Adobe I/O Runtime にデプロイして、AEM as a Cloud Service で使用する
description: Asset Compute プロジェクトとそれに含まれるワーカーは、AEM as a Cloud Service で使用するためには Adobe I/O Runtime にデプロイする必要があります。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
duration: 128
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 100%

---

# Adobe I/O Runtime へのデプロイ

Asset Compute プロジェクトとそれに含まれるワーカーは、AEM as a Cloud Serviceで使用するには Adobe I/O CLI を介して Adobe I/O Runtime にデプロイする必要があります。

AEM as a Cloud Service オーサーサービスで使用するために Adobe I/O Runtime にデプロイする場合は、次の 2 つの環境変数のみが必要です。

+ `AIO_runtime_namespace` は、デプロイ先の App Builder ワークスペースを指します
+ `AIO_runtime_auth` は、App Builder ワークスペースの認証資格情報です

`.env` ファイルで定義された他の標準変数は、AEM as a Cloud Service が Asset compute ワーカーを起動する際に暗黙的に提供されます。

## 開発ワークスペース

このプロジェクトは `Development` ワークスペースを使用して `aio app init` で生成されたため、`AIO_runtime_namespace` は自動的に `81368-wkndaemassetcompute-development` に設定され、ローカルの `.env` ファイルで一致する `AIO_runtime_auth` が設定されます。`.env` ファイルが deploy コマンドの発行に使用されるディレクトリに存在する場合、OS レベルの変数書き出しによって置き換えられない限り、その値が使用されます。このようにして[ステージングと実稼動](#stage-and-production)ワークスペースがターゲットされます。

![.env 変数を使用した aio アプリのデプロイ](./assets/runtime/development__aio.png)

プロジェクトの `.env` ファイルで定義されたワークスペースにデプロイする手順は次のとおりです。

1. Asset Compute プロジェクトのルートでコマンドラインを開きます。
1. コマンド `aio app deploy` を実行します。
1. コマンド `aio app get-url` を実行して、AEM as a Cloud Service 処理プロファイルでこのカスタム Asset Compute ワーカーを参照するために使用するワーカー URL を取得します。プロジェクトに複数のワーカーが含まれる場合、各ワーカーの個別の URL がリストされます。

ローカル開発環境と AEM as a Cloud Service 開発環境で個別の Asset Compute デプロイメントを使用する場合、AEM as a Cloud Service 開発環境へのデプロイメントは、[ステージングおよび実稼働デプロイメント](#stage-and-production)と同じ方法で管理できます。

## ステージングワークスペースと実稼動ワークスペース{#stage-and-production}

ステージング環境および実稼動環境へのデプロイは、通常、任意の CI/CD システムで行います。Asset Compute プロジェクトは、個別に各ワークスペース（ステージングおよび実稼動）にデプロイする必要があります。

環境変数を true に設定すると、`.env` 内の同じ名前の変数の値が上書きされます。

![書き出し変数を使用した aio アプリのデプロイ](./assets/runtime/stage__export-and-aio.png)

ステージング環境および実稼動環境にデプロイする場合の一般的なアプローチは、通常、CI/CD システムで自動化されます。

1. [Adobe I/O CLI npm モジュールおよび Asset Compute プラグイン](../set-up/development-environment.md#aio)がインストールされていることを確認します
1. デプロイする Asset Compute プロジェクトを Git からチェックアウトします
1. 環境変数に、ターゲットワークスペース（ステージングまたは実稼動）に対応する値を設定します
   + 2 つの必須変数は `AIO_runtime_namespace` と `AIO_runtime_auth` であり、ワークスペースの&#x200B;__すべてダウンロード__&#x200B;機能を介して、Adobe I/O Developer Console のワークスペースごとに取得されます。

![Developer Console - AIO Runtime 名前空間と認証](./assets/runtime/stage-auth-namespace.png)

これらのキーの値は、コマンドラインから export コマンドを発行することで設定できます。

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Asset Compute ワーカーがクラウドストレージなどの他の変数を必要とする場合は、それらを環境変数として書き出す必要があります。

1. ターゲットワークスペースのデプロイ先となるすべての環境変数を設定したら、次の deploy コマンドを実行します。
   + `aio app deploy`
1. AEM as a Cloud Service 処理プロファイルで参照されるワーカー URL は、次からも取得できます。
   + `aio app get-url`。

Asset Compute プロジェクトのバージョンを変更すると、ワーカーの URL も新しいバージョンを反映するように変更され、URL を処理プロファイルで更新する必要があります。

## ワークスペース API のプロビジョニング{#workspace-api-provisioning}

条件 [Adobe I/Oでの App Builder プロジェクトの設定](../set-up/app-builder.md) ローカル開発をサポートするために、新しい開発ワークスペースが作成され、 __asset compute、I/O イベント__ および __I/O イベント管理 API__ が追加されました。

__Asset Compute、I/O イベント__、__I/O イベント管理__&#x200B;の API は、ローカル開発に使用するワークスペースにのみ明示的に追加されます。 AEM as a Cloud Service 環境と（排他的に）統合するワークスペースでは、API が AEM as a Cloud Service で自然に利用できるようになるため、これらの API を明示的に追加する必要は&#x200B;__ありません__。
