---
title: AEM as a Adobe I/O Runtimeで使用するAsset computeワーカーのCloud Serviceへのデプロイ
description: 'asset computeプロジェクトとその中に含まれるワーカーは、AEM as a Adobe I/O Runtimeで使用するために、Cloud Serviceにデプロイする必要があります。 '
feature: asset computeマイクロサービス
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: 統合、開発
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---


# Adobe I/O Runtimeへのデプロイ

asset computeプロジェクトとそれに含まれるワーカーは、AEM as aCloud Serviceで使用するAdobe I/OCLIを介してAdobe I/O Runtimeにデプロイする必要があります。

AEM as a Experience Authorサービスで使用するためにAdobe I/O Runtimeにデプロイする場合は、次の2つの環境変数が必要です。

+ `AIO_runtime_namespace` は、AdobeProject Firefly Workspaceのデプロイ先を指します。
+ `AIO_runtime_auth` は、AdobeProject Firefly workspaceの認証資格情報です。

`.env`ファイルに定義されているその他の標準変数は、Asset computeワーカーを呼び出す際に、AEMによってCloud Serviceとして暗黙的に提供されます。

## 開発ワークスペース

このプロジェクトは`Development`ワークスペースを使用して`aio app init`を使用して生成されたので、`AIO_runtime_namespace`は、ローカルの`.env`ファイル内の対応する`AIO_runtime_auth`を使用して、自動的に`81368-wkndaemassetcompute-development`に設定されます。  deployコマンドの発行に使用するディレクトリに`.env`ファイルが存在する場合は、その値が使用されます。ただし、OSレベルの変数書き出しで置き換えられる場合を除きます。つまり、[stageおよびproduction](#stage-and-production)ワークスペースがターゲットになります。

![.env変数を使用したaioアプリのデプロイ](./assets/runtime/development__aio.png)

プロジェクト`.env`ファイルでワークスペースにデプロイするには、次の手順に従います。

1. asset compute・プロジェクトのルートでコマンド・ラインを開く
1. コマンド`aio app deploy`を実行します。
1. コマンド`aio app get-url`を実行して、AEM as a  as aCloud Service処理プロファイルで使用するワーカーURLを取得し、このカスタムAsset computeワーカーを参照します。 プロジェクトに複数のワーカーが含まれる場合は、各ワーカーの個別のURLがリストされます。

ローカル開発とAEM as aCloud Service開発環境が別々のAsset computeデプロイメントを使用する場合は、[ステージデプロイメントおよび実稼動デプロイメント](#stage-and-production)と同じ方法で、AEM as aCloud Service開発へのデプロイメントを管理できます。

## ステージングワークスペースと実稼動ワークスペース{#stage-and-production}

ステージングおよび実稼動環境へのデプロイは、通常、選択したCI/CDシステムでおこないます。 asset computeプロジェクトは、個別に各Workspace（ステージングおよび実稼動）にデプロイする必要があります。

trueの環境変数を設定すると、`.env`内の同じ名前の変数の値が上書きされます。

![書き出し変数を使用したaioアプリデプロイ](./assets/runtime/stage__export-and-aio.png)

通常、CI/CDシステムで自動化され、ステージ環境と実稼動環境にデプロイする一般的なアプローチは次のとおりです。

1. [Adobe I/OCLI npmモジュールとAsset computeプラグイン](../set-up/development-environment.md#aio)がインストールされていることを確認します。
1. GitからデプロイするAsset computeプロジェクトを確認します。
1. 環境変数に、ターゲットワークスペース（ステージングまたは実稼動）に対応する値を設定します
   + 必要な変数は`AIO_runtime_namespace`と`AIO_runtime_auth`の2つで、Adobe I/O開発者コンソールのワークスペースごとに、ワークスペースの&#x200B;__「すべて__&#x200B;をダウンロード」機能を通じて取得されます。

![Adobe開発者コンソール — AIOランタイムの名前空間と認証](./assets/runtime/stage-auth-namespace.png)

これらのキーの値は、コマンドラインからexportコマンドを発行することで設定できます。

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

asset computeワーカーがクラウドストレージなど、その他の変数を必要とする場合は、環境変数としてもエクスポートする必要があります。

1. ターゲットワークスペースのデプロイ先となるすべての環境変数を設定したら、次のdeployコマンドを実行します。
   + `aio app deploy`
1. AEM as a Cloud Service処理プロファイルで参照されるワーカーURLは、次の場所からも使用できます。
   + `aio app get-url`

asset computeプロジェクトのバージョンが変更されると、ワーカーURLも新しいバージョンを反映するように変更され、URLを処理プロファイルで更新する必要があります。

## Workspace APIプロビジョニング{#workspace-api-provisioning}

ローカル開発をサポートするために[Adobe I/OにAdobeProject Fireflyプロジェクトを設定する際に、新しいDevelopment Workspaceが作成され、__Asset compute、I/Oイベント__、__I/O Events Management API__&#x200B;が追加されました。](../set-up/firefly.md)

__Asset compute、I/Oイベント__&#x200B;および&#x200B;__I/Oイベント管理API__ APIは、ローカル開発に使用されるワークスペースにのみ明示的に追加されます。 AEM as aCloud Service環境と（排他的に）統合するワークスペースでは、APIがAEM as a Workspaceとして自然に利用できるようになるので、明示的に追加されたこれらのAPIは&#x200B;__必要ありません__。
