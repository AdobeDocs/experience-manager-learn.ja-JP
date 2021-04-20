---
title: AEMでCloud Serviceとして使用するために、Asset computeワーカーをAdobe I/O Runtimeに配置する
description: 'asset computeプロジェクトとその従業員を含む労働者は、AEMがCloud Serviceとして使用するために、Adobe I/O Runtimeに配備する必要があります。 '
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---


# Adobe I/O Runtimeに展開

asset computeプロジェクトとその従業員を含む従業員は、AEMがCloud Serviceとして使用するAdobe I/OCLIを介してAdobe I/O Runtimeに導入する必要があります。

AEMでCloud Service作成者サービスとして使用するためにAdobe I/O Runtimeに展開する場合は、2つの環境変数が必要です。

+ `AIO_runtime_namespace` AdobeプロジェクトのFirefelly Workspaceが
+ `AIO_runtime_auth` は、AdobeProject Firefly workspaceの認証資格情報です

`.env`ファイルに定義されている他の標準変数は、AEMがAsset computeワーカーを呼び出すときに、Cloud Serviceとして暗黙的に提供されます。

## 開発ワークスペース

このプロジェクトは`Development`ワークスペースを使用して`aio app init`を使用して生成されたので、`AIO_runtime_namespace`は、ローカルの`.env`ファイル内の`AIO_runtime_auth`と一致する`81368-wkndaemassetcompute-development`に自動的に設定されます。  deployコマンドの発行に使用されるディレクトリに`.env`ファイルが存在する場合、その値が使用されます。ただし、ファイルがOSレベルの変数エクスポートで置き換えられる場合は除きます。これは、[stageおよびproduction](#stage-and-production)ワークスペースをターゲットにします。

![.env変数を使用したaioアプリのデプロイ](./assets/runtime/development__aio.png)

プロジェクト`.env`ファイル内のワークスペース定義に配置するには：

1. asset computeプロジェクトのルートにあるコマンドラインを開きます
1. コマンド`aio app deploy`を実行します
1. コマンド`aio app get-url`を実行して、AEMでCloud Service処理プロファイルとして使用するワーカーURLを取得し、このカスタムAsset computeワーカーを参照します。 プロジェクトに複数のワーカーが含まれる場合、各ワーカーの個別のURLが表示されます。

Cloud Service開発環境としてのローカル開発とAEMで別々のAsset computeデプロイメントを使用する場合、AEMへのデプロイメントは、[Stage and Production deployments](#stage-and-production)と同じ方法でCloud Service開発として管理できます。

## ステージと実稼働のワークスペース{#stage-and-production}

ステージおよび実稼働環境への展開は、通常、選択したCI/CDシステムで行います。 asset computeプロジェクトは、個別に各Workspace(Stage and Production)にデプロイする必要があります。

true環境変数を設定すると、`.env`内の同じ名前の変数の値が上書きされます。

![エクスポート変数を使用したaioアプリのデプロイ](./assets/runtime/stage__export-and-aio.png)

一般的なアプローチは、通常、CI/CDシステムによって自動化され、ステージおよび実稼働環境に展開する場合は次のようになります。

1. [Adobe I/OCLI npmモジュールとAsset computeプラグイン](../set-up/development-environment.md#aio)がインストールされていることを確認します。
1. Gitから展開するAsset computeプロジェクトをチェックアウトします
1. 環境変数を、ターゲットワークスペース（ステージまたは実稼動）に対応する値で設定します
   + 必須の変数は`AIO_runtime_namespace`と`AIO_runtime_auth`の2つで、Adobe I/Oデベロッパーコンソールのワークスペースごとに、ワークスペースの&#x200B;__すべてをダウンロード__&#x200B;機能を介して取得されます。

![Adobe開発者コンソール — AIOランタイム名前空間と認証](./assets/runtime/stage-auth-namespace.png)

これらのキーの値は、コマンドラインからexportコマンドを発行することで設定できます。

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

asset computeワーカーがクラウドストレージなど、他の変数を必要とする場合は、これらも環境変数としてエクスポートする必要があります。

1. ターゲットワークスペースのデプロイ先となる環境変数がすべて設定されたら、デプロイコマンドを実行します。
   + `aio app deploy`
1. AEMがCloud Service処理プロファイルとして参照するワーカーURLは、次の場所からも利用できます。
   + `aio app get-url`

asset computeプロジェクトのバージョンが変更された場合、ワーカーURLも新しいバージョンを反映するように変更され、そのURLは「処理プロファイル」で更新する必要があります。

## Workspace APIプロビジョニング{#workspace-api-provisioning}

ローカル開発をサポートするためにAdobeプロジェクトFireflyプロジェクトをAdobe I/O](../set-up/firefly.md)に設定すると、新しい開発ワークスペースが作成され、__Asset compute、I/Oイベント__、__I/Oイベント管理API__&#x200B;が追加されました。[

__Asset compute、I/Oイベント__&#x200B;および&#x200B;__I/Oイベント管理API__ APIは、ローカル開発に使用するワークスペースにのみ明示的に追加されます。 Cloud Service環境としてAEMに統合（排他的）されるワークスペースは、APIとして明示的に追加されたこれらのAPIを&#x200B;__必要としません__、AEMではCloud Serviceとして自動的に利用できます。
