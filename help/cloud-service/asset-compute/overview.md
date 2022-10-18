---
title: AEM as a Cloud Serviceのasset computeマイクロサービス拡張機能
description: このチュートリアルでは、元のAsset computeを円に切り抜いてアセットレンディションを作成し、設定可能なコントラストと明るさを適用する、シンプルなアセットワーカーの作成について説明します。 ワーカー自体は基本的ですが、このチュートリアルでは、AEM as a Cloud Serviceで使用するカスタムAsset computeワーカーの作成、開発およびデプロイを調べるために、このワーカーを使用します。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
source-git-commit: 09f6c4b0bec10edd306270a7416fcaff8a584e76
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 3%

---

# asset computeマイクロサービス拡張機能

AEM as a AEMのAsset computeマイクロサービスは、Cloud Serviceに保存されたアセットのバイナリデータの読み取りと操作に使用されるカスタムワーカーの開発とデプロイをサポートし、カスタムアセットレンディションの作成に最も一般的に使用されます。

AEM 6.x のカスタムAEM Workflow プロセスは、アセットレンディションの読み取り、変換、書き戻しに使用されていましたが、AEMのas a Cloud ServiceAsset computeワーカーは、このニーズを満たします。

## 今後の予定

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

このチュートリアルでは、元のAsset computeを円に切り抜いてアセットレンディションを作成し、設定可能なコントラストと明るさを適用する、シンプルなアセットワーカーの作成について説明します。 ワーカー自体は基本的ですが、このチュートリアルでは、AEM as a Cloud Serviceで使用するカスタムAsset computeワーカーの作成、開発およびデプロイを調べるために、このワーカーを使用します。

### 目的 {#objective}

1. asset compute・ワーカーの構築とデプロイに必要なアカウントとサービスのプロビジョニングと設定
1. asset computeプロジェクトの作成と設定
1. カスタムレンディションを生成するAsset computeワーカーの開発
1. テストの書き込みと、カスタムAsset computeワーカーのデバッグ方法の学習
1. asset computeワーカーをデプロイし、処理プロファイルを介してAEM as a Cloud Serviceオーサーサービスを統合します

## 設定

asset computeワーカーの拡張に適切に準備し、プロビジョニングと設定が必要なサービスとアカウント、および開発用にローカルにインストールされたソフトウェアを理解する方法を説明します。

### アカウントとサービスのプロビジョニング{#accounts-and-services}

次のアカウントおよびサービスでは、チュートリアル、AEMas a Cloud Serviceの開発環境またはサンドボックスプログラム、App Builder およびMicrosoft Azure Blob Storage へのアクセスを完了するために、へのプロビジョニングとアクセスが必要です。

+ [アカウントとサービスのプロビジョニング](./set-up/accounts-and-services.md)

### ローカル開発環境

Asset computeプロジェクトのローカル開発には、以下を含む従来のAEM開発とは異なる、特定の開発者ツールセットが必要です。Microsoft Visual Studio Code、Docker Desktop、Node.js およびサポートする npm モジュール。

+ [ローカル開発環境の設定](./set-up/development-environment.md)

### App Builder

asset computeプロジェクトは、特別に定義された App Builder プロジェクトなので、Adobe Developerコンソールで App Builder にアクセスして設定およびデプロイする必要があります。

+ [App Builder のセットアップ](./set-up/app-builder.md)

## 開発

asset computeプロジェクトを作成および設定し、カスタムワーカーを開発してカスタムのアセットレンディションを生成する方法を説明します。

### 新しいAsset computeプロジェクト

1 つ以上のasset computeワーカーを含むAsset computeプロジェクトは、インタラクティブAdobe I/OCLI を使用して生成されます。 asset computeプロジェクトは、特別に構造化された App Builder プロジェクトで、Node.js プロジェクトです。

+ [新しいAsset computeプロジェクト](./develop/project.md)

### 環境変数の設定

環境変数は、 `.env` ローカル開発用のファイル。ローカル開発に必要なAdobe I/O資格情報とクラウドストレージ資格情報を提供するために使用されます。

+ [ 環境変数の設定](./develop/environment-variables.md)

### manifest.yml の設定

asset computeプロジェクトには、プロジェクト内に含まれるすべてのAsset computeワーカーを定義するマニフェストと、Adobe I/O Runtimeにデプロイして実行に使用できるリソースが含まれます。

+ [manifest.yml の設定](./develop/manifest.md)

### 作業者の開発

asset computeワーカーの開発は、Asset computeマイクロサービスを拡張する中核となります。ワーカーには、結果のアセットレンディションの生成を生成または編成するカスタムコードが含まれているからです。

+ [asset computeワーカーの開発](./develop/worker.md)

### asset compute開発ツールの使用

asset compute開発ツールは、ワーカー生成レンディションをデプロイ、実行、プレビューし、迅速かつ反復的なAsset computeワーカー開発をサポートするローカル Web ハーネスを提供します。

+ [asset compute開発ツールの使用](./develop/development-tool.md)

## テストとデバッグ

カスタムAsset computeワーカーをテストして操作に自信を持たせる方法を学び、Asset computeワーカーをデバッグして、カスタムコードの実行方法を理解し、トラブルシューティングする方法を学びます。

### ワーカーのテスト

asset computeは、適切な動作が容易になるようにテストを定義する、ワーカー用のテストスイートを作成するためのテストフレームワークを提供します。

+ [ワーカーのテスト](./test-debug/test.md)

### ワーカーのデバッグ

asset computeワーカーは、従来の `console.log(..)` 出力、との統合 __VS Code__ および  __wskdebug__&#x200B;を使用すると、開発者は、リアルタイムで実行されるワーカーコードを順を追って実行できます。

+ [ワーカーのデバッグ](./test-debug/debug.md)

##  のデプロイ

カスタムAsset computeワーカーをAEM as a Cloud Serviceにデプロイし、AEM Assets の処理プロファイルを介してAEMのas a Cloud Serviceオーサーから呼び出すことで、カスタムアセットワーカーをAdobe I/O Runtimeに統合する方法を説明します。

### Adobe I/O Runtimeにデプロイ

asset computeワーカーをAEM as a Cloud Serviceで使用するには、Adobe I/O Runtimeにデプロイする必要があります。

+ [処理プロファイルの使用](./deploy/runtime.md)

### AEM処理プロファイルを使用したワーカーの統合

Adobe I/O Runtimeにデプロイすると、Asset computeワーカーは、を介してAEM as a Cloud Serviceに登録できます。 [アセット処理プロファイル](../../assets/configuring/processing-profiles.md). 処理プロファイルは、そのアセットに適用されるアセットフォルダーにも適用されます。

+ [AEM処理プロファイルとの統合](./deploy/processing-profiles.md)

## 詳細

これらの簡潔なチュートリアルは、前の章で確立された基礎的な学習に基づいて構築された、より高度な使用例に取り組んでいます。

+ [asset computeメタデータワーカーの開発](./advanced/metadata.md) メタデータをに書き戻すことができる

## Github のコードベース

このチュートリアルのコードベースは、GitHub で次の場所から入手できます。

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ master ブランチ

ソースコードに必要なが含まれていません `.env` または `config.json` ファイル。 これらは、 [アカウントとサービス](#accounts-and-services) 情報。

## その他のリソース

以下に、その他の情報と、Adobeワーカーを開発するために役立つ API および SDK を提供する様々なAsset computeリソースを示します。

### ドキュメント化

+ [asset computeサービスドキュメント](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=ja)
+ [asset compute開発ツールの readme](https://github.com/adobe/asset-compute-devtool)
+ [asset computeサンプルワーカー](https://github.com/adobe/asset-compute-example-workers)

### API と SDK

+ [asset computeSDK](https://github.com/adobe/asset-compute-sdk)
   + [asset computeコモンズ](https://github.com/adobe/asset-compute-commons)
   + [asset computeXMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobeクラウド Blobstore ラッパーライブラリ](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobeノード取得再試行ライブラリ](https://github.com/adobe/node-fetch-retry)
+ [asset computeサンプルワーカー](https://github.com/adobe/asset-compute-example-workers)
