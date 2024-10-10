---
title: Asset Compute 拡張機能用の環境変数の設定
description: 環境変数は、ローカル開発用に .env ファイルで管理され、ローカル開発に必要な Adobe I/O 資格情報とクラウドストレージ資格情報を提供するために使用されます。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
duration: 121
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '587'
ht-degree: 100%

---

# 環境変数の設定

![ドット環境ファイル](assets/environment-variables/dot-env-file.png)

Asset Compute ワーカーの開発を開始する前に、プロジェクトが Adobe I/O とクラウドストレージ情報で設定されていることを確認します。この情報はプロジェクトの `.env` に保存されており、ローカル開発のみに使用され、Git には保存されません。この `.env` ファイルは、キーと値のペアをローカルの Asset Compute ローカル開発環境に表示する便利な方法を提供します。Asset Compute ワーカーを Adobe I/O Runtime に[導入](../deploy/runtime.md)する場合、`.env` ファイルは使用されず、環境変数を介して値のサブセットが渡されます。サードパーティの web サービスの開発資格情報など、その他のカスタムパラメーターとシークレットも `.env` ファイルに保存する必要があります。

## `private.key` を参照

![秘密鍵](assets/environment-variables/private-key.png)

`.env` ファイルを開き、`ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` キーのコメントを解除し、Adobe I/O App Builder プロジェクトに追加された公開証明書とペアになる `private.key` へのファイルシステム上の絶対パスを指定します。

+ キーペアが Adobe I/O によって生成された場合は、`config.zip` の一部として自動ダウンロードされます。
+ 公開鍵を Adobe I/O に指定した場合は、一致する秘密鍵も所有している必要があります。
+ これらのキーペアがない場合は、新規キーペアを生成するか、次の場所に新規公開鍵をアップロードできます。
  [https://console.adobe.com](https://console.adobe.io)／Asset Compute App Builder プロジェクト／Workspaces @ Development ／サービスアカウント（JWT）。

`private.key` ファイルはシークレットを含んでいるので、Git にチェックインせず、プロジェクト外部の安全な場所に保存する必要があります。

例えば、macOS では次のようになります。

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## クラウドストレージの資格情報の設定

Asset Compute ワーカーのローカル開発には、[クラウドストレージ](../set-up/accounts-and-services.md#cloud-storage)へのアクセスが必要です。ローカル開発に使用するクラウドストレージの資格情報は、`.env` ファイルで提供されます。

このチュートリアルでは Azure Blob Storage の使用を推奨しますが、Amazon S3 とそれに対応する `.env` ファイル内のキーを代わりに使用することもできます。

### Azure Blob Storage の使用

コメントを解除して `.env` ファイルに次のキーを入力し、Azure Portal にあるプロビジョニングされたクラウドストレージの値を入力します。

![Azure Blob Storage](./assets/environment-variables/azure-portal-credentials.png)

1. `AZURE_STORAGE_CONTAINER_NAME` キーの値
1. `AZURE_STORAGE_ACCOUNT` キーの値
1. `AZURE_STORAGE_KEY` キーの値

例えば、次のようになります（値は説明のみを目的としています）。

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

結果の `.env` ファイルは次のようになります。

![Azure Blob Storage の資格情報](assets/environment-variables/cloud-storage-credentials.png)

Microsoft Azure Blob Storage を使用していない場合は、これらを削除するか、コメントアウトしたままにします（プレフィックスとして `#` を付けます）。

### Amazon S3 クラウドストレージの使用{#amazon-s3}

Amazon S3 クラウドストレージを使用している場合は、コメントを解除して `.env` ファイルに次のキーを入力します。

例えば、次のようになります（値は説明のみを目的としています）。

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## プロジェクト設定の検証

生成された Asset Compute プロジェクトを設定したら、コードを変更する前に設定を検証し、サポートするサービスが `.env` ファイルにプロビジョニングされていることを確認します。

Asset Compute プロジェクトの Asset Compute 開発ツールを開始するには：

1. Asset Compute プロジェクトのルートでコマンドラインを開き（VS Code では、IDE で、ターミナル／新規ターミナルから直接開くことができます）、次のコマンドを実行します。

   ```
   $ aio app run
   ```

1. ローカルの Asset Compute 開発ツールが、デフォルトの web ブラウザー __http://localhost:9000__ で開きます。

   ![aio app run](assets/environment-variables/aio-app-run.png)

1. 開発ツール初期化時のエラーメッセージについては、コマンドライン出力と web ブラウザーで確認します。
1. Asset Compute 開発ツールを停止するには、`aio app run` を実行したウィンドウで `Ctrl-C` をタップしてプロセスを終了します。

## トラブルシューティング

+ [private.key が見つからず、開発ツールを開始できない場合](../troubleshooting.md#missing-private-key)
