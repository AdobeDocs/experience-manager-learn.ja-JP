---
title: AEM as a Cloud Service の Asset Compute マイクロサービスの拡張性
description: このチュートリアルでは、オリジナルのアセットを円に切り抜いてアセットレンディションを作成し設定可能なコントラストと明るさを適用する、シンプルな Asset Compute ワーカーの作成について順を追って説明します。ワーカー自体は基本的ですが、このチュートリアルではこのワーカーを使用して、AEM as a Cloud Service で使用するカスタム Asset Compute ワーカーの作成、開発およびデプロイを行います。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
duration: 333
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '986'
ht-degree: 100%

---

# Asset Compute マイクロサービスの拡張性

AEM as Cloud Service の Asset Compute マイクロサービスは、AEM に保存されたアセットのバイナリデータの読み取りと操作に使用される（最も一般的には、カスタムアセットレンディションの作成に使用される）カスタムワーカーの開発とデプロイをサポートしています。

AEM 6.x ではカスタム AEM ワークフロープロセスを使用してアセットレンディションの読み取り、変換および書き戻しを行っていましたが、AEM as a Cloud Service では、Asset Compute ワーカーがこのニーズを満たします。

## 作業の内容

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

このチュートリアルでは、オリジナルのアセットを円に切り抜いてアセットレンディションを作成し設定可能なコントラストと明るさを適用する、シンプルな Asset Compute ワーカーの作成について順を追って説明します。ワーカー自体は基本的ですが、このチュートリアルではこのワーカーを使用して、AEM as a Cloud Service で使用するカスタム Asset Compute ワーカーの作成、開発およびデプロイを行います。

### 目的 {#objective}

1. Asset Compute ワーカーの作成とデプロイに必要なアカウントとサービスをプロビジョニングしてセットアップします。
1. Asset Compute プロジェクトを作成し設定します。
1. カスタムレンディションを生成する Asset Compute ワーカーを開発します。
1. テストを作成し、カスタム Asset Compute ワーカーのデバッグ方法を学びます。
1. Asset Compute ワーカーをデプロイし、処理プロファイルを通じて AEM as a Cloud Service オーサーサービスと統合します。

## セットアップ

Asset Compute ワーカーの拡張に適切に備える方法を学び、プロビジョニングと設定が必要なサービスとアカウントや、開発用にローカルにインストールするソフトウェアについて理解します。

### アカウントとサービスのプロビジョニング{#accounts-and-services}

このチュートリアルを完了するには、次のアカウントとサービスのプロビジョニングとそれらへのアクセス、AEM as a Cloud Service 開発環境またはサンドボックスプログラム、Application Builder と Microsoft Azure Blob Storage へのアクセスが必要です。

+ [アカウントとサービスのプロビジョニング](./set-up/accounts-and-services.md)

### ローカル開発環境

Asset Compute プロジェクトのローカル開発には、Microsoft Visual Studio Code、Docker Desktop、Node.js、npm サポートモジュールなど、従来の AEM 開発とは異なる特定の開発者ツールセットが必要です。

+ [ローカル開発環境のセットアップ](./set-up/development-environment.md)

### App Builder

Asset Compute プロジェクトは特別に定義された App Builder プロジェクトなので、Adobe Developer Console で App Builder にアクセスして設定およびデプロイする必要があります。

+ [App Builder を設定する](./set-up/app-builder.md)

## 開発

Asset Compute プロジェクトを作成して設定したあと、カスタムのアセットレンディションを生成するカスタムワーカーを開発する方法について説明します。

### Asset Compute プロジェクトの新規作成

Asset Compute プロジェクトは、1 つ以上の Asset Compute ワーカーを含んでおり、インタラクティブな Adobe I/O CLI を使用して生成されます。Asset Compute プロジェクトは、特別に構造化された App Builder プロジェクトであり、ひいては Node.js プロジェクトになります。

+ [Asset Compute プロジェクトの新規作成](./develop/project.md)

### 環境変数の設定

環境変数は、ローカル開発用の `.env` ファイルに保持され、ローカル開発に必要な Adobe I/O 資格情報やクラウドストレージ資格情報を提供するために使用されます。

+ [ 環境変数の設定](./develop/environment-variables.md)

### manifest.yml の設定

Asset Compute プロジェクトには、プロジェクト内に含まれるすべての Asset Compute ワーカーを定義するマニフェストと、Adobe I/O Runtime にデプロイして実行するときに使用できるリソースが含まれています。

+ [manifest.yml の設定](./develop/manifest.md)

### ワーカーの開発

Asset Compute ワーカーには、結果のアセットレンディションの生成または生成の調整を行うカスタムコードが含まれているので、ワーカーの開発は、Asset Compute マイクロサービスの拡張の中核となります。

+ [Asset Compute ワーカーの開発](./develop/worker.md)

### Asset Compute 開発ツールの使用

Asset Compute 開発ツールは、ワーカーが生成したレンディションをデプロイ、実行およびプレビューするためのローカル web ハーネスを提供し、迅速かつ反復的な Asset Compute ワーカー開発をサポートします。

+ [Asset Compute 開発ツールの使用](./develop/development-tool.md)

## テストとデバッグ

カスタム Asset Compute ワーカーをテストして、自信を持って運用できるようにする方法と、Asset Compute ワーカーをデバッグして、カスタムコードがどう実行されるかを把握し問題をトラブルシューティングする方法を説明します。

### ワーカーのテスト

Asset Compute は、ワーカーのテストスイートを作成して、適切な動作を保証するテストを簡単に定義できるようにするためのテストフレームワークを提供しています。

+ [ワーカーのテスト](./test-debug/test.md)

### ワーカーのデバッグ

Asset Compute ワーカーは、従来の `console.log(..)` 出力から __VS Code__ や __wskdebug__ との統合まで、様々なレベルのデバッグを提供しており、開発者がワーカーコードをリアルタイムで実行しながら 1 ステップずつ実行できます。

+ [ワーカーのデバッグ](./test-debug/debug.md)

## デプロイ

カスタム Asset Compute ワーカーをまず Adobe I/O Runtime にデプロイし、次に AEM Assets の処理プロファイルを通じて AEM as a Cloud Service オーサーから呼び出すことで、カスタム Asset Compute ワーカーを AEM as a Cloud Service と統合する方法について説明します。

### Adobe I/O Runtime へのデプロイ

Asset Compute ワーカーを AEM as a Cloud Service で使用するには、Adobe I/O Runtime にデプロイする必要があります。

+ [処理プロファイルの使用](./deploy/runtime.md)

### AEM 処理プロファイルを通じたワーカーの統合

Asset Compute ワーカーは、Adobe I/O Runtime にいったんデプロイすると、[アセット処理プロファイル](../../assets/configuring/processing-profiles.md)を通じて AEM as a Cloud Service に登録できるようになります。処理プロファイルをアセットフォルダーに適用すると、そのフォルダー内のアセットにも処理プロファイルが適用されます。

+ [AEM 処理プロファイルとの統合](./deploy/processing-profiles.md)

## 詳細

これの簡略化されたチュートリアルでは、前の章で獲得した基本的な知識に基づいて、より高度なユースケースに取り組んでいます。

+ メタデータを AEM に書き戻すことができる [Asset Compute メタデータワーカーの開発](./advanced/metadata.md)

## GitHub のコードベース

このチュートリアルのコードベースは、GitHub で次の場所から入手できます。

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)（マスターブランチ）

ソースコードには、必要な `.env` ファイルまたは `config.json` ファイルが含まれていません。これらは、[アカウントとサービス](#accounts-and-services)の情報を使用して追加および設定する必要があります。

## その他のリソース

さらなる情報のほか、Asset Compute ワーカーの開発に役立つ API や SDK を提供する様々な Asset Compute リソースを以下に示します。

### ドキュメント化

+ [Asset Compute サービスのドキュメント](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=ja)
+ [Asset Compute 開発ツールの README](https://github.com/adobe/asset-compute-devtool)
+ [Asset Compute サンプルワーカー](https://github.com/adobe/asset-compute-example-workers)

### API と SDK

+ [Asset Compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute Commons](https://github.com/adobe/asset-compute-commons)
   + [Asset Compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe Cloud Blobstore Wrapper ライブラリ](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe Node Fetch Retry ライブラリ](https://github.com/adobe/node-fetch-retry)
+ [Asset Compute サンプルワーカー](https://github.com/adobe/asset-compute-example-workers)
