---
title: Asset Compute の拡張性に対応するローカル開発環境のセットアップ
description: Asset Compute ワーカー（Node.js JavaScript アプリケーション）の開発には、Node.js や様々な npm モジュールから Docker Desktop や Microsoft Visual Studio Code に至るまで、従来の AEM 開発とは異なる特有の開発ツールが必要になります。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
duration: 96
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '459'
ht-degree: 100%

---

# ローカル開発環境のセットアップ

Adobe Asset Compute プロジェクトは、AEM SDK から提供されるローカル AEM ランタイムと統合することができないので、AEM Maven プロジェクトアーキタイプに基づく AEM アプリケーションで必要になるものとは別に、独自のツールチェーンを使用して開発されます。

Asset Compute マイクロサービスを拡張するには、ローカルの開発者マシンに次のツールをインストールする必要があります。

## 簡易セットアップ手順

以下は、簡素化されたセットアップ手順です。これらの開発ツールの詳細については、以下の個別の節で説明します。

1. [Docker Desktop をインストール](https://www.docker.com/products/docker-desktop)し、必要な Docker イメージを取り込みます。

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Visual Studio Code をインストール](https://code.visualstudio.com/download)します。
1. [Node.js 10 以降をインストールします。](../../local-development-environment/development-tools.md#node-js)
1. 必要な npm モジュールと Adobe I/O CLI プラグインをコマンドラインからインストールします。

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

簡易インストール手順について詳しくは、以下の節を参照してください。

## Visual Studio Code のインストール{#vscode}

[Microsoft Visual Studio Code](https://code.visualstudio.com/download) は、Asset Compute ワーカーの開発とデバッグに使用されます。ワーカーの開発には他の [JavaScript 互換 IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide) も使用できますが、Asset Compute ワーカーの[デバッグ](../test-debug/debug.md)に統合できるのは Visual Studio Code のみです。

このチュートリアルでは、Visual Studio Code を使用することを前提としています。Visual Studio Code が、Asset Compute を拡張する際の最適な開発者エクスペリエンスを提供するからです。

## Docker Desktop のインストール{#docker}

最新の安定した [Docker Desktop](https://www.docker.com/products/docker-desktop) をダウンロードしインストールします。これが、Asset Compute プロジェクトをローカルで[テスト](../test-debug/test.md)および[デバッグ](../test-debug/debug.md)するために必要なものだからです。

Docker Desktop をインストールしたら、Docker Desktop を起動し、コマンドラインから次の Docker イメージをインストールします。

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows マシンを使用する開発者は、上記のイメージに Linux コンテナを使用していることを確認してください。Linux コンテナに切り替える手順については、[Docker for Windows のドキュメント](https://docs.docker.com/docker-for-windows/)を参照してください。

## Node.js （および npm）のインストール{#node-js}

Asset Compute ワーカーは [Node.js](https://nodejs.org/) をベースにしているので、開発およびビルドには Node.js 10 以降（および npm）が必要です。

+ 従来の AEM 開発と同様に [Node.js（および npm）をインストール](../../local-development-environment/development-tools.md#node-js)します。

## Adobe I/O CLI のインストール{#aio}

[Adobe I/O CLI をインストール](../../local-development-environment/development-tools.md#aio-cli)します。Adobe I/O CLI（__aio__）は、Adobe I/O テクノロジーの使用および同テクノロジーとのやり取りを容易にするコマンドライン（CLI）npm モジュールで、カスタム Asset Compute ワーカーの生成にもローカル開発にも使用されます。

```
$ npm install -g @adobe/aio-cli
```

## Adobe I/O CLI Asset Compute プラグインのインストール{#aio-asset-compute}

[Adobe I/O CLI Asset Compute プラグイン](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## wskdebug のインストール{#wskdebug}

Asset Compute ワーカーのローカルデバッグを容易にする [Apache OpenWhisk デバッグ](https://www.npmjs.com/package/@openwhisk/wskdebug) npm モジュールをダウンロードしインストールします。

_[wskdebug](#wskdebug) が機能するには、Visual Studio Code 1.48.x 以降が必要です。_

```
$ npm install -g @openwhisk/wskdebug
```

## ngrok のインストール{#ngrok}

ローカル開発マシンへのパブリックアクセスを提供する [ngrok](https://www.npmjs.com/package/ngrok) npm モジュールをダウンロードしインストールして、Asset Compute ワーカーのローカルデバッグを容易に行えるようにします。

```
$ npm install -g ngrok --unsafe-perm=true
```
