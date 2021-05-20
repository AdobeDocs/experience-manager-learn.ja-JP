---
title: AEM as aCloud Serviceのasset computeマイクロサービス拡張機能
description: このチュートリアルでは、元のAsset computeを円に切り抜いてアセットレンディションを作成し、設定可能なコントラストと明るさを適用する、シンプルなアセットワーカーの作成について説明します。 ワーカー自体は基本的ですが、このチュートリアルでは、AEMをCloud Serviceとして使用するカスタムAsset computeワーカーの作成、開発およびデプロイを検討するために、このワーカーを使用します。
feature: asset computeマイクロサービス
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: 統合、開発
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1033'
ht-degree: 3%

---


# asset computeマイクロサービス拡張機能

AEM as a AEM as a Asset Microservicesは、Cloud Serviceに保存されたアセットのバイナリデータの読み取りと操作に使用されるカスタムワーカーの開発とデプロイメントをサポートしています。最も一般的に、カスタムAsset computeレンディションの作成に使用されます。

AEM 6.xでは、Cloud ServiceAsset computeワーカーはこのニーズを満たすので、アセットレンディションの読み取り、変換、書き戻しにAEM 6.xのカスタムAEM Workflowプロセスが使用されていました。

## どうするか

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

このチュートリアルでは、元のAsset computeを円に切り抜いてアセットレンディションを作成し、設定可能なコントラストと明るさを適用する、シンプルなアセットワーカーの作成について説明します。 ワーカー自体は基本的ですが、このチュートリアルでは、AEMをCloud Serviceとして使用するカスタムAsset computeワーカーの作成、開発およびデプロイを検討するために、このワーカーを使用します。

### 目的 {#objective}

1. asset compute・ワーカーの構築とデプロイに必要なアカウントとサービスのプロビジョニングと設定
1. asset computeプロジェクトの作成と設定
1. カスタムレンディションを生成するamAsset computeワーカーの開発
1. テストの書き込みと、カスタムAsset computeワーカーのデバッグ方法の学習
1. asset computeワーカーをデプロイし、処理プロファイルを介してAEMをCloud Serviceオーサーサービスとして統合する

## 設定

asset computeワーカーの拡張に適切に対応し、プロビジョニングと設定が必要なサービスとアカウント、および開発用にローカルにインストールされたソフトウェアを理解する方法を説明します。

### アカウントとサービスのプロビジョニング{#accounts-and-services}

次のアカウントとサービスでは、 AEM as aCloud Service開発環境またはサンドボックスプログラム、AdobeプロジェクトFireflyおよびMicrosoft Azure Blob Storageへのアクセスを完了するために、プロビジョニングとへのアクセスが必要です。

+ [アカウントとサービスのプロビジョニング](./set-up/accounts-and-services.md)

### ローカル開発環境

Asset computeプロジェクトのローカル開発には、従来のAEM開発とは異なり、特定の開発者ツールセットが必要です。次に例を示します。Microsoft Visual Studio Code、Docker Desktop、Node.jsおよびサポートするnpmモジュール。

+ [ローカル開発環境の設定](./set-up/development-environment.md)

### AdobeプロジェクトFirefly

asset computeプロジェクトは、特別に定義されたAdobeProject Fireflyプロジェクトです。そのため、設定およびデプロイするには、Adobe開発者コンソールのAdobeProject Fireflyにアクセスする必要があります。

+ [AdobeProject Fireflyの設定](./set-up/firefly.md)

## 開発

カスタムワーカーを開発し、カスタムAsset computeレンディションを生成するプロジェクトプロジェクトの作成と設定の方法について説明します。

### 新しいAsset computeプロジェクト

asset computeプロジェクトは、1つ以上のAsset computeワーカーを含み、インタラクティブAdobe I/OCLIを使用して生成されます。 asset computeプロジェクトは、特別に構造化されたAdobeProject Fireflyプロジェクトで、Node.jsプロジェクトになります。

+ [新しいAsset computeプロジェクト](./develop/project.md)

### 環境変数の設定

環境変数は、ローカル開発用に`.env`ファイルで管理され、ローカル開発に必要なAdobe I/O資格情報とクラウドストレージ資格情報を提供するために使用されます。

+ [ 環境変数の設定](./develop/environment-variables.md)

### manifest.ymlの設定

asset computeプロジェクトには、プロジェクト内に含まれるすべてのAsset computeワーカーを定義するマニフェストと、Adobe I/O Runtimeにデプロイして実行する際に使用可能なリソースが含まれます。

+ [manifest.ymlの設定](./develop/manifest.md)

### 作業者の育成

asset computeワーカーの開発は、Asset computeマイクロサービスを拡張する中核となります。ワーカーには、結果のアセットレンディションの生成を生成または編成するカスタムコードが含まれます。

+ [asset computeワーカーの開発](./develop/worker.md)

### asset compute開発ツールの使用

asset compute開発ツールは、ワーカー生成レンディションをデプロイ、実行、プレビューし、迅速かつ反復的なAsset computeワーカー開発をサポートするローカルWebハーネスを提供します。

+ [asset compute開発ツールの使用](./develop/development-tool.md)

## テストとデバッグ

カスタムAsset computeワーカーをテストして操作に自信を持つようにし、Asset computeワーカーをデバッグしてカスタムコードの実行方法を理解し、トラブルシューティングする方法を説明します。

### ワーカーのテスト

asset computeは、適切な動作が容易になるようにテストを定義する、ワーカー用のテストスイートを作成するためのテストフレームワークを提供します。

+ [ワーカーのテスト](./test-debug/test.md)

### ワーカーのデバッグ

asset computeワーカーは、従来の`console.log(..)`出力から、__VS Code__&#x200B;および&#x200B;__wskdebug__&#x200B;との統合に至るまで、様々なレベルのデバッグを提供し、開発者はリアルタイムで実行されるワーカーコードを順を追って実行できます。

+ [ワーカーのデバッグ](./test-debug/debug.md)

## デプロイ

カスタムAsset computeワーカーをAEM as aCloud Serviceと統合する方法について説明します。まずAdobe I/O Runtimeにデプロイし、次に、AEM Assetsの処理プロファイルを介してAEMをCloud Service作成者として呼び出します。

### Adobe I/O Runtimeへのデプロイ

asset computeワーカーをAEM as a Adobe I/O Runtimeで使用するには、Cloud Serviceにデプロイする必要があります。

+ [処理プロファイルの使用](./deploy/runtime.md)

### AEM処理プロファイルを使用したワーカーの統合

Adobe I/O Runtimeにデプロイすると、Asset computeワーカーは、[Assets処理プロファイル](../../assets/configuring/processing-profiles.md)を介してAEMにCloud Serviceとして登録できます。 処理プロファイルは、そのアセットに適用されるアセットフォルダーにも適用されます。

+ [AEM処理プロファイルとの統合](./deploy/processing-profiles.md)

## 詳細

これらの簡潔なチュートリアルは、前の章で確立された基礎的な学習に基づいて構築された、より高度な使用例に取り組んでいます。

+ [にメタデータを書き戻す](./advanced/metadata.md) ことができるAsset computeメタデータワーカーを開発する

## Githubのコードベース

チュートリアルのコードベースは、GitHubで次の場所から入手できます。

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ master branch

ソースコードに必要な`.env`ファイルや`config.json`ファイルが含まれていません。 [アカウントとサービス](#accounts-and-services)情報を使用して、これらを追加し、設定する必要があります。

## その他のリソース

以下に、Adobeワーカーを開発するための参考情報と役立つAPIおよびSDKを提供する様々なAsset computeリソースを示します。

### ドキュメント

+ [asset computeサービスドキュメント](https://docs.adobe.com/content/help/ja-JP/asset-compute/using/extend/understand-extensibility.html)
+ [asset compute開発ツールのreadme](https://github.com/adobe/asset-compute-devtool)
+ [asset computeサンプルワーカー](https://github.com/adobe/asset-compute-example-workers)

### APIとSDK

+ [asset computeSDK](https://github.com/adobe/asset-compute-sdk)
   + [asset computeコモンズ](https://github.com/adobe/asset-compute-commons)
   + [asset computeXMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [AdobeクラウドBlobstoreラッパーライブラリ](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobeノード取得の再試行ライブラリ](https://github.com/adobe/node-fetch-retry)
+ [asset computeサンプルワーカー](https://github.com/adobe/asset-compute-example-workers)
