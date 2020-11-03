---
title: 「アセット計算の拡張機能」の環境変数の設定
description: 環境変数は、ローカル開発用に.envファイルに保持され、ローカル開発に必要なAdobeI/O資格情報とクラウドストレージ資格情報を提供するために使用されます。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 1%

---


#  環境変数の設定

![ドット環境ファイル](assets/environment-variables/dot-env-file.png)

アセットコンピューティングワーカーの開発を開始する前に、AdobeI/Oとクラウドストレージ情報を使用してプロジェクトを構成してください。 この情報はプロジェクトに保存されます。このプロジェクト `.env` は、ローカル開発にのみ使用され、Gitには保存されません。 この `.env` ファイルは、キー/値のペアをローカルのアセット計算ローカル開発環境に公開する便利な方法を提供します。 アセット計算ワーカーを [Adobe I/O Runtimeに](../deploy/runtime.md) 配置する場合 `.env` 、ファイルは使用されず、値のサブセットが環境変数を介して渡されます。 サードパーティWebサービスの開発資格情報など、その他のカスタムパラメーターやシークレットも `.env` ファイルに保存できます。

## Webページの `private.key`

![秘密鍵](assets/environment-variables/private-key.png)

ファイルを開き、 `.env` キーのコメントを解除し、AdobeI/O FireFlyプロジェクトに追加された公開証明書とペア `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH``private.key` になるファイルシステムの絶対パスを指定します。

+ キーペアがAdobeI/Oによって生成された場合は、の一部として自動的にダウンロードされました `config.zip`。
+ AdobeI/Oに公開鍵を提供した場合は、一致する秘密鍵も所有する必要があります。
+ これらのキーペアがない場合は、新しいキーペアを生成するか、次の下部に新しい公開鍵をアップロードできます。
   [https://console.adobe.com](https://console.adobe.io) /Your Asset Compute Firefly project/Workspaces @ Development/Service Account(JWT)。

ファイルはシークレットが含まれているので、Gitにチェックインしないでください。プロジェクトの外部の安全な場所に保存する必要があります。 `private.key`

例えば、macOSでは次のようになります。

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## クラウドストレージ資格情報の設定

アセットコンピューティングワーカーの地域開発では、 [クラウドストレージへのアクセスが必要](../set-up/accounts-and-services.md#cloud-storage)。 ローカル開発に使用されるクラウドストレージ資格情報は、 `.env` ファイル内に提供されます。

このチュートリアルはAzure Blobストレージの使用を優先しますが、AmazonS3および `.env` ファイル内の対応するキーを代わりに使用できます。

### Azure Blobストレージを使用しています

ファイルの次のキーのコメントを解除して入力し、Azure Portalで見つかったプロビジョニングされたクラウドストレージの値を入力します。 `.env`

![Azure Blobストレージ](./assets/environment-variables/azure-portal-credentials.png)

1. キーの値 `AZURE_STORAGE_CONTAINER_NAME` 。
1. キーの値 `AZURE_STORAGE_ACCOUNT` 。
1. キーの値 `AZURE_STORAGE_KEY` 。

例えば、次のようになります（図の値のみ）。

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

結果の `.env` ファイルは次のようになります。

![Azure Blobストレージ資格情報](assets/environment-variables/cloud-storage-credentials.png)

Microsoft Azure Blobストレージを使用していない場合は、(前付けにを使用して `#`)コメントアウトを削除するか、そのままにします。

### AmazonS3クラウドストレージの使用{#amazon-s3}

AmazonS3クラウドストレージを使用している場合は、コメントを解除し、 `.env` ファイルに次のキーを設定します。

例えば、次のようになります（図の値のみ）。

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## プロジェクト構成の検証

生成されたアセット計算プロジェクトが設定されたら、コードを変更する前に設定を検証し、サポートするサービスが `.env` ファイル内でプロビジョニングされていることを確認します。

開始資産計算プロジェクトの資産計算開発ツールを作成するには：

1. 「アセット計算」プロジェクトルート（VSコード内）のコマンドラインを開き（IDEで端末/新しい端末を使用して直接開くことができます）、次のコマンドを実行します。

   ```
   $ aio app run
   ```

1. ローカルのAsset Compute Development ToolがデフォルトのWebブラウザー(http://localhost:9000)で開き __ます__。

   ![aioアプリの実行](assets/environment-variables/aio-app-run.png)

1. 開発ツールの初期化時に、コマンドライン出力とWebブラウザーでエラーメッセージが表示されないかを確認します。
1. アセット計算開発ツールを停止するには、実行したウィンドウ `Ctrl-C` でをタップし、プロセス `aio app run` を終了します。

## トラブルシューティング

+ [private.keyが見つからないため、開発ツールは開始できません](../troubleshooting.md#missing-private-key)
