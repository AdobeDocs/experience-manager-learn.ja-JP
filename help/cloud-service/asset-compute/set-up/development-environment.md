---
title: asset computeの拡張機能用のローカル開発環境の設定
description: Node.js JavaScriptAsset computeである開発ワーカーは、従来のAEM開発とは異なる、特定の開発ツール（Node.jsや様々なnpmモジュール、Docker Desktop、Microsoft Visual Studio Codeなど）を必要とします。
feature: asset computeマイクロサービス
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: 統合、開発
role: デベロッパー
level: 中級、経験豊富
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 1%

---


# ローカル開発環境の設定

AdobeAsset computeプロジェクトは、AEM SDKが提供するローカルAEMランタイムと統合できず、AEM Mavenプロジェクトアーキタイプに基づくAEMアプリケーションが必要とするものとは別に、独自のツールチェーンを使用して開発されます。

asset computeマイクロサービスを拡張するには、ローカルの開発者マシンに次のツールをインストールする必要があります。

## 簡潔な設定手順

以下は、設定手順の要約です。 これらの開発ツールの詳細については、以下の個別の節で説明します。

1. [Docker ](https://www.docker.com/products/docker-desktop) Desktopをインストールし、必要なDockerイメージを取り出します。

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:latest
   ```

1. [Visual Studioコードのインストール](https://code.visualstudio.com/download)
1. [Node.js 10以降のインストール](../../local-development-environment/development-tools.md#node-js)
1. 必要なnpmモジュールとAdobe I/OCLIプラグインをコマンドラインからインストールします。

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

簡体的なインストール手順の詳細については、以下の節を参照してください。

## Visual Studioコードのインストール{#vscode}

[Microsoft Visual Studio ](https://code.visualstudio.com/download) Codeは、Asset computeワーカーの開発とデバッグに使用されます。[JavaScript互換の他のIDE](../../local-development-environment/development-tools.md#set-up-the-development-ide)を使用してワーカーを開発できますが、Visual Studioコードのみを[debug](../test-debug/debug.md)Asset computeワーカーに統合できます。

_wskdebugを機能させるには、Visual Studio Code 1.48.x以降が必要 [](#wskdebug) です。_

このチュートリアルでは、Asset computeを拡張するための最高の開発者エクスペリエンスを提供するVisual Studioコードを使用することを前提としています。

## Docker Desktopのインストール{#docker}

[test](../test-debug/test.md)と[debug](../test-debug/debug.md)Asset computeプロジェクトに必要な最新の安定版[Docker Desktop](https://www.docker.com/products/docker-desktop)をローカルにダウンロードしてインストールします。

Docker Desktopをインストールした後、開始し、コマンドラインから次のDockerイメージをインストールします。

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windowsマシンの開発者は、上のイメージにLinuxコンテナを使っていることを確認してください。 Linuxコンテナに切り替える手順は、[Windows用ドッカーのドキュメント](https://docs.docker.com/docker-for-windows/)に記載されています。

## Node.js （およびnpm）{#node-js}のインストール

asset computeワーカーは[Node.js](https://nodejs.org/)ベースであるため、開発と構築にNode.js 10+ （およびnpm）が必要です。

+ [従来のAEM開発と同じ方法](../../local-development-environment/development-tools.md#node-js) で、Node.js（およびnpm）をインストールします。

## Adobe I/OCLI{#aio}のインストール

[Adobe I/OCLI](../../local-development-environment/development-tools.md#aio-cli) ____ 、またはAdobe I/Oテクノロジの使用と操作を容易にするCLI （コマンドライン） npmモジュールをインストールし、カスタムAsset computeワーカーの生成とローカル開発の両方に使用します。

```
$ npm install -g @adobe/aio-cli
```

## Adobe I/OCLIAsset computeプラグインのインストール{#aio-asset-compute}

[Adobe I/OCLIAsset computeプラグイン](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## wskdebug{#wskdebug}をインストール

[Apache OpenWisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npmモジュールをダウンロードしてインストールし、Asset computeワーカーのローカルデバッグを容易にします。

_wskdebugを機能させるには、Visual Studio Code 1.48.x以降が必要 [](#wskdebug) です。_

```
$ npm install -g @openwhisk/wskdebug
```

## ngrok{#ngrok}のインストール

[ngrok](https://www.npmjs.com/package/ngrok) npmモジュールをダウンロードしてインストールします。このモジュールは、ローカル開発マシンに対する一般的なアクセスを提供し、Asset compute作業者のローカルデバッグを容易にします。

```
$ npm install -g ngrok --unsafe-perm=true
```
