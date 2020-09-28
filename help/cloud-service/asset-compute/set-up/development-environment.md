---
title: アセット計算の拡張機能用のローカル開発環境の設定
description: Node.js JavaScriptアプリケーションであるAsset Compute workerの開発には、Node.jsや様々なnpmモジュール、Docker DesktopやMicrosoft Visual Studioコードなど、従来のAEMの開発とは異なる特定の開発ツールが必要です。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: 53e4235c55d890765e9f13ffeb37a2c805fb307b
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# ローカル開発環境の設定

Adobeアセット計算アプリケーションは、AEM SDKが提供するローカルAEMランタイムと統合できず、AEM Mavenプロジェクトアーキタイプに基づくAEMアプリケーションが必要とするものとは別に、独自のツールチェーンを使用して開発されます。

Asset Computeマイクロサービスを拡張するには、ローカルの開発者マシンに次のツールをインストールする必要があります。

## 簡潔な設定手順

以下は、設定手順の要約です。 これらの開発ツールの詳細については、以下の個別の節で説明します。

1. [Docker Desktop](https://www.docker.com/products/docker-desktop) をインストールし、必要なDockerイメージを取り出します。

   ```
   $ docker pull openwhisk/action-nodejs-v10:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Visual Studioコードのインストール](https://code.visualstudio.com/download)
1. [Node.js 10以降のインストール](../../local-development-environment/development-tools.md#node-js)
1. 必要なnpmモジュールとAdobeI/O CLIプラグインをコマンドラインからインストールします。

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

## Visual Studioコードのインストール{#vscode}

[Microsoft Visual Studioコード](https://code.visualstudio.com/download) は、Asset Computeアプリケーションの開発とデバッグに使用されます。 アプリケーションの開発には他の [JavaScript互換のIDE](../../local-development-environment/development-tools.md#set-up-the-development-ide) も使用できますが、Visual Studioコードのみを統合して [、アセット計算アプリケーションの](../test-debug/debug.md) デバッグに使用できます。

_wskdebugを機能させるには、Visual Studio Code 1.48.x以[降が必要です](#wskdebug)。_

このチュートリアルでは、Visual Studioコードを使用することを前提としています。Visual Studioコードを使用すると、アセット計算を拡張するための最高の開発者エクスペリエンスが得られます。

## Docker Desktopのインストール{#docker}

最新の安定した [Docker Desktop](https://www.docker.com/products/docker-desktop)をダウンロードしてインストールします。これは、Asset [Computeプロジェクトをローカルで](../test-debug/test.md) テストおよび [](../test-debug/debug.md) デバッグするために必要です。

Docker Desktopをインストールした後、開始し、コマンドラインから次のDockerイメージをインストールします。

```
$ docker pull openwhisk/action-nodejs-v10:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windowsマシンの開発者は、上のイメージにLinuxコンテナを使っていることを確認してください。 Linuxコンテナに切り替える手順は、 [Docker for Windowsのドキュメントで説明されています](https://docs.docker.com/docker-for-windows/)。

## Node.js（およびnpm）のインストール{#node-js}

アセット計算ワーカーは [Node.js](https://nodejs.org/) アプリケーションです。したがって、開発と構築にNode.js 10以降（およびnpm）が必要です。

+ [従来のAEM開発と同じ方法でNode.js（およびnpm）](../../local-development-environment/development-tools.md#node-js) をインストールします。

## Install Adobe I/O CLI{#aio}

[AdobeI/O CLI](../../local-development-environment/development-tools.md#aio-cli)、または ____ aioは、AdobeI/Oテクノロジの使用とやりとりを容易にするコマンドライン(CLI) npmモジュールで、カスタムのAsset Compute Workerの生成とローカル開発の両方に使用されます。

```
$ npm install -g @adobe/aio-cli
```

## AdobeI/O CLIのAsset Computeプラグインのインストール{#aio-asset-compute}

AdobeI/O CLI Asset Computeプラグイン [](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## wskdebugのインストール{#wskdebug}

Asset Computeアプリケーションのローカルデバッグを容易にするために、 [Apache OpenWisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npmモジュールをダウンロードしてインストールします。

_wskdebugを機能させるには、Visual Studio Code 1.48.x以[降が必要です](#wskdebug)。_

```
$ npm install -g @openwhisk/wskdebug
```

## ngrokのインストール{#ngrok}

Asset Computeアプリケーションのローカルデバッグを容易にするために、ローカル開発マシンへの公開アクセスを提供するngrok [](https://www.npmjs.com/package/ngrok) npmモジュールをダウンロードしてインストールします。

```
$ npm install -g ngrok --unsafe-perm=true
```
