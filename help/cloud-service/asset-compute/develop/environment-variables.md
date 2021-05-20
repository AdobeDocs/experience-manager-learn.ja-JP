---
title: 環境変数の拡張機能のAsset compute
description: 環境変数は、ローカル開発用に.envファイルで管理され、ローカル開発に必要なAdobe I/O資格情報とクラウドストレージ資格情報を提供するために使用されます。
feature: asset computeマイクロサービス
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: 統合、開発
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 1%

---


#  環境変数の設定

![ドット環境ファイル](assets/environment-variables/dot-env-file.png)

asset computeワーカーの開発を開始する前に、プロジェクトにAdobe I/Oとクラウドストレージの情報が設定されていることを確認します。 この情報は、ローカル開発にのみ使用され、Gitには保存されない、プロジェクトの`.env`に保存されます。 `.env`ファイルは、キーと値のペアをローカルAsset computeローカル開発環境に公開する便利な方法を提供します。 [Asset computeワーカーをAdobe I/O Runtimeにデプロイする場合、`.env`ファイルは使用されず、値のサブセットが環境変数を介して渡されます。 ](../deploy/runtime.md)その他のカスタムパラメーターおよびシークレットは、サードパーティWebサービスの開発用資格情報など、 `.env`ファイルにも保存できます。

## `private.key`

![秘密鍵](assets/environment-variables/private-key.png)

`.env`ファイルを開き、`ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH`キーのコメントを解除し、Adobe I/OのFireFlyプロジェクトに追加された公開証明書とペアになる`private.key`へのファイルシステムの絶対パスを指定します。

+ キーペアがAdobe I/Oによって生成された場合は、`config.zip`の一部として自動ダウンロードされました。
+ 公開鍵をAdobe I/Oに指定した場合は、一致する秘密鍵も所有する必要があります。
+ これらのキーペアがない場合は、新しいキーペアを生成するか、新しい公開鍵をの下部にアップロードできます。
   [https://console.adobe.com](https://console.adobe.io) /Asset computeのFireflyプロジェクト/ Workspaces @開発/サービスアカウント(JWT)。

`private.key`ファイルはシークレットが含まれているのでGitにチェックインしないでください。プロジェクトの外の安全な場所に保存する必要があります。

例えば、macOSでは次のようになります。

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## クラウドストレージ資格情報の設定

Asset computeワーカーのローカル開発には、[クラウドストレージ](../set-up/accounts-and-services.md#cloud-storage)へのアクセスが必要です。 ローカル開発に使用するクラウドストレージの資格情報は、`.env`ファイルに格納されます。

このチュートリアルではAzure Blob Storageの使用を推奨しますが、Amazon S3と、 `.env`ファイル内の対応するキーを代わりに使用できます。

### Azure Blob Storageの使用

`.env`ファイルで次のキーのコメントを解除して入力し、Azure Portalで見つかったプロビジョニング済みクラウドストレージの値をそれらのキーに入力します。

![Azure Blobストレージ](./assets/environment-variables/azure-portal-credentials.png)

1. `AZURE_STORAGE_CONTAINER_NAME`キーの値
1. `AZURE_STORAGE_ACCOUNT`キーの値
1. `AZURE_STORAGE_KEY`キーの値

例えば、次のようになります（説明用の値のみ）。

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

結果の`.env`ファイルは次のようになります。

![Azure BLOBストレージ資格情報](assets/environment-variables/cloud-storage-credentials.png)

Microsoft Azure Blob Storageを使用していない場合は、（`#`のプレフィックスを付けて）コメントアウトしたままにするか、削除します。

### Amazon S3クラウドストレージの使用{#amazon-s3}

Amazon S3クラウドストレージを使用している場合は、コメントを解除して`.env`ファイルに次のキーを入力します。

例えば、次のようになります（説明用の値のみ）。

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## プロジェクト設定の検証

生成されたAsset computeプロジェクトを設定したら、コードを変更する前に設定を検証し、サポートするサービスが`.env`ファイルでプロビジョニングされていることを確認します。

asset computeプロジェクトのAsset compute開発ツールを開始するには：

1. asset computeプロジェクトのルート（VS Codeで、ターミナル/新しいターミナルからIDEで直接開くことができます）のコマンドラインを開き、次のコマンドを実行します。

   ```
   $ aio app run
   ```

1. ローカルAsset compute開発ツールが、デフォルトのWebブラウザー(__http://localhost:9000__)で開きます。

   ![aioアプリ実行](assets/environment-variables/aio-app-run.png)

1. 開発ツールの初期化時に、コマンドライン出力とWebブラウザでエラーメッセージを確認します。
1. asset compute開発ツールを停止するには、`aio app run`を実行したウィンドウで`Ctrl-C`をタップし、プロセスを終了します。

## トラブルシューティング

+ [private.keyが見つからないため、開発ツールを開始できません](../troubleshooting.md#missing-private-key)
