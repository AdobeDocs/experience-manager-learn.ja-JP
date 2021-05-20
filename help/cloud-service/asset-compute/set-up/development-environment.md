---
title: asset compute拡張用のローカル開発環境の設定
description: asset computeワーカー（Node.js JavaScriptアプリケーション）の開発には、Node.jsや様々なnpmモジュール、Docker DesktopやMicrosoft Visual Studio Codeなど、従来のAEM開発とは異なる、特定の開発ツールが必要です。
feature: asset computeマイクロサービス
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: 統合、開発
role: Developer
level: Intermediate, Experienced
source-git-commit: 53c20b9774c15b04a1c78c7c0c7b61a60996bf60
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 1%

---


# ローカル開発環境の設定

AdobeAsset computeプロジェクトは、AEM SDKが提供するローカルAEMランタイムと統合できず、AEM Mavenプロジェクトアーキタイプに基づくAEMアプリケーションが必要とするものとは別に、独自のツールチェーンを使用して開発されます。

asset computeマイクロサービスを拡張するには、ローカルの開発者マシンに次のツールをインストールする必要があります。

## 指示の要約

以下は、設定手順の要約です。 これらの開発ツールの詳細については、以下の個別の節で説明します。

1. [Docker Desktopをインスト](https://www.docker.com/products/docker-desktop) ールし、必要なDocker画像を取り込みます。

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Visual Studio Codeのインストール](https://code.visualstudio.com/download)
1. [Node.js 10以降のインストール](../../local-development-environment/development-tools.md#node-js)
1. 必要なnpmモジュールとAdobe I/OCLIプラグインをコマンドラインからインストールします。

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

インストール手順の要約については、以下の節を参照してください。

## Visual Studio Codeのインストール{#vscode}

[Microsoft Visual Studio Codeは、](https://code.visualstudio.com/download) Asset compute・ワーカーの開発とデバッグに使用されます。他の[JavaScript互換のIDE](../../local-development-environment/development-tools.md#set-up-the-development-ide)を使用してワーカーを開発できますが、Visual Studioコードのみを[debug](../test-debug/debug.md)Asset computeワーカーに統合できます。

_wskdebugを機能させるには、Visual Studio Code 1.48.x以降が [](#wskdebug) 必要です。_

このチュートリアルでは、Visual Studio Codeを使用することを前提としています。Visual Studio Codeは、Asset computeを拡張するための最適な開発者エクスペリエンスを提供します。

## Docker Desktopのインストール{#docker}

[test](../test-debug/test.md)と[debug](../test-debug/debug.md)Asset computeプロジェクトをローカルで実行する場合は、最新の安定した[Docker Desktop](https://www.docker.com/products/docker-desktop)をダウンロードしてインストールします。

Docker Desktopをインストールしたら、起動し、コマンドラインから次のDockerイメージをインストールします。

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windowsマシンの開発者は、上記のイメージにLinuxコンテナを使用していることを確認する必要があります。 Linuxコンテナに切り替える手順は、[Docker for Windowsのドキュメント](https://docs.docker.com/docker-for-windows/)に記載されています。

## Node.js （およびnpm）{#node-js}のインストール

asset computeワーカーは[Node.js](https://nodejs.org/)ベースなので、開発と構築にはNode.js 10以降（およびnpm）が必要です。

+ [従来のAEM開発と同じ方法で、Node.js（およびnpm）](../../local-development-environment/development-tools.md#node-js) をインストールします。

## インストールAdobe I/OCLI{#aio}

[Adobe I/OCLI](../../local-development-environment/development-tools.md#aio-cli)、または ____ aioisは、Adobe I/Oテクノロジーの使用と操作を容易にするコマンドライン(CLI) npmモジュールをインストールし、カスタムAsset computeワーカーの生成とローカル開発の両方に使用します。

```
$ npm install -g @adobe/aio-cli
```

## Adobe I/OCLIAsset computeプラグイン{#aio-asset-compute}をインストールします。

[Adobe I/OCLIAsset computeプラグイン](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## wskdebugのインストール{#wskdebug}

[Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npmモジュールをダウンロードしてインストールし、Asset computeワーカーのローカルデバッグを容易にします。

_wskdebugを機能させるには、Visual Studio Code 1.48.x以降が [](#wskdebug) 必要です。_

```
$ npm install -g @openwhisk/wskdebug
```

## ngrokのインストール{#ngrok}

[ngrok](https://www.npmjs.com/package/ngrok) npmモジュールをダウンロードしてインストールします。このモジュールは、ローカル開発マシンに対して公開アクセスを提供し、Asset computeワーカーのローカルデバッグを容易にします。

```
$ npm install -g ngrok --unsafe-perm=true
```
