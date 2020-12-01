---
title: Cloud ServiceとしてのAEM用のasset computeマイクロサービス拡張機能
description: このチュートリアルでは、単純なAsset computeワーカーの作成について説明します。このワーカーは、元のアセットを丸に切り抜いてアセットのレンディションを作成し、設定可能なコントラストと明るさを適用します。 ワーカー自体は基本的ですが、このチュートリアルでは、AEMをCloud Serviceとして使用するカスタムAsset computeワーカーの作成、開発および配置を検討するために使用します。
feature: asset-compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1028'
ht-degree: 3%

---


# asset computeマイクロサービス拡張機能

AEM asCloud ServiceAsset computeマイクロサービスは、AEMに保存されたアセットのバイナリデータを読み取り、操作し、カスタムアセットレンディションを作成するために使用されるカスタムワーカーの開発と展開をサポートしています。

AEM 6.xでは、AEM Workflowプロセスはアセットレンディションの読み取り、変換、書き込みに使用されていましたが、AEMではCloud ServiceAsset computeの作業者はこのニーズを満たしています。

## 今後の作業

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

このチュートリアルでは、単純なAsset computeワーカーの作成について説明します。このワーカーは、元のアセットを丸に切り抜いてアセットのレンディションを作成し、設定可能なコントラストと明るさを適用します。 ワーカー自体は基本的ですが、このチュートリアルでは、AEMをCloud Serviceとして使用するカスタムAsset computeワーカーの作成、開発および配置を検討するために使用します。

### 目的 {#objective}

1. asset computeワーカーを構築しデプロイするために必要なアカウントとサービスのプロビジョニングと設定
1. asset computeプロジェクトの作成と設定
1. カスタムレンディションを生成するAsset computeワーカーの開発
1. カスタムAsset computeワーカーのテストを作成し、デバッグする方法を学びます
1. asset computeワーカーをデプロイし、処理プロファイルを介してAEMをCloud Service作成者サービスとして統合します

## 設定

asset computeワーカーの拡張に適した準備を行い、プロビジョニングと設定が必要なサービスとアカウント、および開発用にローカルにインストールするソフトウェアを理解する方法を説明します。

### アカウントとサービスのプロビジョニング{#accounts-and-services}

次のアカウントとサービスは、チュートリアルを完了するために、AEMをCloud Service開発環境またはSandboxプログラムとして、AdobeプロジェクトFireflyおよびMicrosoft Azure Blobストレージにアクセスするために、プロビジョニングとアクセスを必要とします。

+ [アカウントとサービスのプロビジョニング](./set-up/accounts-and-services.md)

### 現地開発環境

asset computeプロジェクトのローカル開発には、従来のAEM開発とは異なる、次のような固有の開発ツールセットが必要です。Microsoft Visual Studioコード、Docker Desktop、Node.js、およびサポートするnpmモジュール

+ [ローカル開発環境の設定](./set-up/development-environment.md)

### Adobeプロジェクトの蛍

asset computeプロジェクトとは、特別に定義されたAdobeプロジェクトFireflyプロジェクトです。そのため、Adobeプロジェクトを設定してデプロイするには、Adobe開発者コンソールでプロジェクトFireflyにアクセスする必要があります。

+ [Adobeプロジェクトの蛍光を設定](./set-up/firefly.md)

## 開発

asset computeプロジェクトを作成および設定し、特注アセットのレンディションを生成するカスタムワーカーを開発する方法を説明します。

### 新しいAsset computeプロジェクトの作成

1人以上のAsset computeワーカーを含むasset computeプロジェクトは、インタラクティブなAdobe I/OCLIを使用して生成されます。 asset computeプロジェクトは、特別に構成されたAdobeプロジェクトFireflyプロジェクトで、Node.jsプロジェクトになっています。

+ [新しいAsset computeプロジェクトの作成](./develop/project.md)

### 環境変数の設定

環境変数は、ローカル開発のために`.env`ファイルに保持され、Adobe I/Oの資格情報と、ローカル開発に必要なクラウドストレージの資格情報を提供するために使用されます。

+ [環境変数の設定](./develop/environment-variables.md)

### manifest.ymlの設定

asset computeプロジェクトには、プロジェクトに含まれるすべてのAsset computeワーカーを定義するマニフェストと、Adobe I/O Runtimeに展開して実行する際に使用できるリソースが含まれます。

+ [manifest.ymlの設定](./develop/manifest.md)

### 労働者の育成

asset computeワーカーの開発は、Asset computeマイクロサービスの拡張の中核となります。ワーカーには、結果のアセットレンディションの生成を生成（調整）するカスタムコードが含まれています。

+ [asset computeワーカーの開発](./develop/worker.md)

### asset compute開発ツールの使用

asset compute開発ツールは、ワーカー生成レンディションの展開、実行、およびプレビューを行うローカルWebハーネスを提供し、Asset computeワーカーの迅速な開発と反復的な開発をサポートします。

+ [asset compute開発ツールの使用](./develop/development-tool.md)

## テストとデバッグ

カスタムAsset computeワーカーの動作に自信があるかどうかをテストし、Asset computeワーカーをデバッグして、カスタムコードの実行方法を理解し、トラブルシューティングする方法を説明します。

### ワーカーのテスト

asset computeは、ワーカーのテストスイートを作成するためのテストフレームワークを提供し、適切な動作を容易にするテストを定義します。

+ [ワーカーのテスト](./test-debug/test.md)

### ワーカーのデバッグ

asset computeワーカーは、従来の`console.log(..)`出力から、__VSコード__&#x200B;および&#x200B;__wskdebug__&#x200B;との統合に至るまで、様々なレベルのデバッグを提供し、開発者はワーカーコードをリアルタイムで実行します。

+ [ワーカーのデバッグ](./test-debug/debug.md)

## デプロイ

カスタムAsset computeワーカーをCloud ServiceとしてAEMに統合する方法について説明します。まず、カスタムワーカーをAdobe I/O Runtimeにデプロイし、次にAEM Assetsの処理プロファイルを介してAEMをCloud Service作成者として呼び出します。

### Adobe I/O Runtimeに展開

AEMでCloud Serviceとして使用するには、asset computeワーカーをAdobe I/O Runtimeに配置する必要があります。

+ [処理プロファイルの使用](./deploy/runtime.md)

### AEM処理プロファイルを使用したワーカーの統合

Adobe I/O Runtimeに導入すると、Asset computeワーカーは、[アセット処理プロファイル](../../assets/configuring/processing-profiles.md)を介して、AEMにCloud Serviceとして登録できます。 次に、処理プロファイルは、そのアセットに適用されるアセットフォルダに適用されます。

+ [AEM処理プロファイルとの統合](./deploy/processing-profiles.md)

## アドバンス

これらの要約チュートリアルは、前章で確立した基礎知識に基づく、より高度な使用例に取り組みます。

+ [メタデータを](./advanced/metadata.md) 

## Githubのコードベース

チュートリアルのコードベースは、次の場所でGithubで入手できます。

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ master branch

ソースコードに必要な`.env`または`config.json`ファイルが含まれていません。 これらは、[アカウントとサービス](#accounts-and-services)の情報を使用して追加および設定する必要があります。

## その他のリソース

以下は、Asset computeワーカーの開発に役立つAPIおよびSDKに関する詳細情報や役立つ情報を提供する様々なAdobeリソースです。

### ドキュメント

+ [asset computeサービスドキュメント](https://docs.adobe.com/content/help/ja-JP/asset-compute/using/extend/understand-extensibility.html)
+ [asset compute開発ツールのお読みください](https://github.com/adobe/asset-compute-devtool)
+ [asset computeサンプル作業者](https://github.com/adobe/asset-compute-example-workers)

### APIとSDK

+ [asset computeSDK](https://github.com/adobe/asset-compute-sdk)
   + [asset computeコモンズ](https://github.com/adobe/asset-compute-commons)
   + [asset computeXMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [AdobeクラウドBlobstoreラッパーライブラリ](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobeノード取得の再試行ライブラリ](https://github.com/adobe/node-fetch-retry)
+ [asset computeサンプル作業者](https://github.com/adobe/asset-compute-example-workers)
