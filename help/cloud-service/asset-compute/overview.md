---
title: Cloud ServiceとしてのAEM用のAsset Compute microservices拡張機能
description: このチュートリアルでは、単純なアセット計算ワーカーの作成について説明します。このワーカーは、元のアセットを円に切り抜いてアセットのレンディションを作成し、設定可能なコントラストと明るさを適用します。 ワーカー自体は基本的ですが、このチュートリアルでは、AEMをCloud Serviceとして使用するカスタムのアセット計算ワーカーの作成、開発および配置を検討するために使用します。
feature: asset-compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '985'
ht-degree: 1%

---


# Asset Compute microservicesの拡張機能

AEM asCloud ServiceのAsset Compute microservicesは、AEMに保存されたアセットのバイナリデータを読み取り、操作し、カスタムアセットレンディションを作成するために使用されるカスタムワーカーの開発と展開をサポートします。

AEM 6.xでは、AEM Workflowプロセスはアセットレンディションの読み取り、変換、書き戻しに使用されていましたが、AEMではCloud Serviceアセットコンピューティングワーカーとして、このニーズを満たしていました。

## 今後の作業

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

このチュートリアルでは、単純なアセット計算ワーカーの作成について説明します。このワーカーは、元のアセットを円に切り抜いてアセットのレンディションを作成し、設定可能なコントラストと明るさを適用します。 ワーカー自体は基本的ですが、このチュートリアルでは、AEMをCloud Serviceとして使用するカスタムのアセット計算ワーカーの作成、開発および配置を検討するために使用します。

### 目的 {#objective}

1. アセットコンピューティングワーカーを構築しデプロイするために必要なアカウントとサービスをプロビジョニングおよび設定します
1. アセット計算プロジェクトの作成と設定
1. カスタムレンディションを生成するAsset Compute Workerの開発
1. カスタムアセット計算ワーカーのテストを書き込み、デバッグ方法を学ぶ
1. Asset Compute Workerをデプロイし、処理プロファイルを介してAEMをCloud Service作成者サービスとして統合します

## 設定

アセットコンピューティングワーカーの拡張に適した準備を行い、プロビジョニングと設定が必要なサービスとアカウント、および開発用にローカルにインストールするソフトウェアを理解する方法を説明します。

### アカウントとサービスのプロビジョニング{#accounts-and-services}

次のアカウントとサービスは、チュートリアルを完了するために、AEMをCloud Service開発環境またはSandboxプログラムとして、AdobeプロジェクトFireflyおよびMicrosoft Azure Blobストレージにアクセスするために、プロビジョニングとアクセスを必要とします。

+ [アカウントとサービスのプロビジョニング](./set-up/accounts-and-services.md)

### 現地開発環境

アセット計算プロジェクトのローカル開発には、従来のAEM開発とは異なる、次のような固有の開発ツールセットが必要です。Microsoft Visual Studioコード、Docker Desktop、Node.js、およびサポートするnpmモジュール

+ [ローカル開発環境の設定](./set-up/development-environment.md)

### Adobeプロジェクトの蛍

アセット計算プロジェクトは、特別に定義されたAdobeプロジェクトFireflyプロジェクトです。そのため、Adobe開発者コンソールでAdobeプロジェクトFireflyにアクセスして設定およびデプロイする必要があります。

+ [Adobeプロジェクトの蛍光を設定](./set-up/firefly.md)

## 開発

アセット計算プロジェクトを作成および設定し、ベスポークアセットのレンディションを生成するカスタムワーカーを開発する方法を説明します。

### 新しいAsset Computeプロジェクトの作成

1つ以上のアセット計算ワーカーを含むアセット計算プロジェクトは、インタラクティブAdobeI/O CLIを使用して生成されます。 アセット計算プロジェクトは、特別に構造化されたAdobeプロジェクトFireflyプロジェクトで、Node.jsプロジェクトになっています。

+ [新しいAsset Computeプロジェクトの作成](./develop/project.md)

### 環境変数の設定

環境変数は、ローカル開発用に `.env` ファイル内に保持され、ローカル開発に必要なAdobeI/O資格情報とクラウドストレージ資格情報を提供するために使用されます。

+ [環境変数の設定](./develop/environment-variables.md)

### manifest.ymlの設定

アセット計算プロジェクトには、プロジェクトに含まれるすべてのアセット計算作業者を定義するマニフェストと、実行のためにAdobe I/O Runtimeに展開したときに使用できるリソースが含まれます。

+ [manifest.ymlの設定](./develop/manifest.md)

### 労働者の育成

アセット計算ワーカーの開発は、アセット計算マイクロサービスの拡張の中核となり、アセット計算マイクロサービスを拡張します。ワーカーには、結果のアセットレンディションの生成を生成、つまり調整するカスタムコードが含まれます。

+ [資産計算ワーカーの開発](./develop/worker.md)

### Asset Compute Development Toolの使用

Asset Compute Development Toolは、ワーカー生成レンディションの展開、実行、プレビューを行うローカルWebハーネスを提供し、Asset Compute Workerの迅速な反復開発をサポートします。

+ [Asset Compute Development Toolの使用](./develop/development-tool.md)

## テストとデバッグ

カスタムのアセット計算ワーカーの動作に自信があるかどうかをテストし、アセット計算ワーカーをデバッグして、カスタムコードの実行方法を理解し、トラブルシューティングする方法を説明します。

### ワーカーのテスト

アセット計算は、ワーカーのテストスイートを作成するためのテストフレームワークを提供し、適切な動作を容易にするテストを定義します。

+ [ワーカーのテスト](./test-debug/test.md)

### ワーカーのデバッグ

アセット計算ワーカーは、従来の `console.log(..)` 出力から、 __VSコード______&#x200B;およびwskdebugとの統合に至るまで、様々なレベルのデバッグを提供し、開発者はワーカーコードをリアルタイムで実行しながらステップ実行できます。

+ [ワーカーのデバッグ](./test-debug/debug.md)

## デプロイ

カスタムのアセットコンピューティングワーカーをAEMにCloud Serviceとして統合する方法を説明します。まず、ワーカーをAdobe I/O Runtimeに展開し、次にAEM Assets処理プロファイルを使用してAEMからCloud Service作成者として呼び出します。

### Adobe I/O Runtimeに展開

AEMと共にCloud Serviceとして使用するには、アセットコンピューティングワーカーをAdobe I/O Runtimeに導入する必要があります。

+ [処理プロファイルの使用](./deploy/runtime.md)

### AEM処理プロファイルを使用したワーカーの統合

Adobe I/O Runtimeに導入すると、アセットコンピューティングワーカーは、 [アセット処理プロファイルを介してCloud ServiceとしてAEMに登録できます](../../assets/configuring/processing-profiles.md)。 次に、処理プロファイルーは、そのアセットに適用されるアセットフォルダーに適用されます。

+ [AEM処理プロファイルとの統合](./deploy/processing-profiles.md)

## Githubのチュートリアルのコードベース

チュートリアルのコードベースは、次の場所でGithubで入手できます。

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ master branch

ソースコードに必要なファイル `.env` または `config.json` ファイルが含まれていません。 アカウントおよびサービスの [情報を使用して、これらを追加および設定する必要があります](#accounts-and-services) 。

## その他のリソース

以下は、アセットコンピューティングワーカーの開発に役立つAPIとSDKに関する詳細情報や役立つ情報を提供する様々なAdobeリソースです。

### ドキュメント

+ [Asset Compute Serviceドキュメント](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [Asset Compute Development Toolのお読みください](https://github.com/adobe/asset-compute-devtool)

### その他のコードサンプル

+ [資産計算サンプルワーカー](https://github.com/adobe/asset-compute-example-workers)

### APIとSDK

+ [Asset Compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [資産計算コモンズ](https://github.com/adobe/asset-compute-commons)
+ [AdobeクラウドBlobstoreラッパーライブラリ](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobeノード取得の再試行ライブラリ](https://github.com/adobe/node-fetch-retry)
