---
title: アセット計算ワーカーのデバッグ
description: アセット計算ワーカーは、簡単なデバッグログ文から、接続されたVSコード（リモートデバッガ）まで、AEMから開始されたAdobe I/O RuntimeのアクティベーションのログをCloud Serviceとして取り込むまで、様々な方法でデバッグできます。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---


# アセット計算ワーカーのデバッグ

アセット計算ワーカーは、簡単なデバッグログ文から、接続されたVSコード（リモートデバッガ）まで、AEMから開始されたAdobe I/O RuntimeのアクティベーションのログをCloud Serviceとして取り込むまで、様々な方法でデバッグできます。

## ログ

アセット計算ワーカーをデバッグする最も基本的な形式は、ワーカーコード内で従来の `console.log(..)` 文を使用します。 JavaScriptオブジェクトは、暗黙的なグローバルオブジェクトなので、すべての `console` コンテキストに常に存在するので、読み込みや要求を行う必要はありません。

これらのログ文は、アセット計算ワーカーの実行方法に応じて、様々な方法でレビューできます。

+ ログ `aio app run`は、標準出力と [開発ツールの](../develop/development-tool.md) アクティベーションログに印刷されます。
   ![aioアプリ実行console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ ログ `aio app test`の印刷元 `/build/test-results/test-worker/test.log`
   ![aioアプリテストコンソール.log(...)](./assets/debug/console-log__aio-app-test.png)
+ を使用 `wskdebug`すると、ログ文はVSコードデバッグコンソール(表示/デバッグコンソール)に、標準出力で印刷されます。
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ を使用 `aio app logs`すると、ログ文はアクティベーションログ出力に出力されます。

## 添付されたデバッガーを使用したリモートデバッグ

>[!WARNING]
>
>wskdebugとの互換性を確保するために、Microsoft Visual Studio Code 1.48.0以降を使用する

wskdebug [](https://www.npmjs.com/package/@openwhisk/wskdebug) npmモジュールは、VSコードでブレークポイントを設定し、コードをステップスルーする機能など、デバッガーをアセット計算ワーカーに接続できます。

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_wskdebugを使用したAsset Compute Workerのデバッグのクリックスルー（オーディオなし）_

1. wskdebug [と](../set-up/development-environment.md#wskdebug) ngrok [](../set-up/development-environment.md#ngork) npmモジュールがインストールされていることを確認します。
1. Docker Desktopと [サポートするDockerイメージがインストールされ](../set-up/development-environment.md#docker) 、実行されていることを確認します。
1. 開発ツールのアクティブな実行中のインスタンスをすべて閉じます。
1. デプロイ済みのアクション名(間に名前 `aio app deploy` を入力)を使用して最新のコードをデプロイし、記録 `[...]`します。 これは、手順8のを更新するた `launch.json` めに使用されます。

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```
1. コマンドを使用して、Asset Compute Development Toolの新しいインスタンスを開始します。 `npx adobe-asset-compute devtool`
1. VSコードで、左側のナビゲーションのデバッグアイコンをタップします
   + プロンプトが表示されたら、launch.jsonファイルを __作成/Node.jsをタップして、新しい__`launch.json` ファイルを作成します。
   + それ以外の場合は、「 __プログラムを__ 起動 __」ドロップダウンの右にある歯車__ アイコンをタップして、 `launch.json` 既存のアイコンをエディターで開きます。
1. 追加配列に対する次のJSONオブジェクトの設定： `configurations`

   ```json
   {
       "type": "pwa-node",
       "request": "launch",
       "name": "wskdebug",
       "attachSimplePort": 0,
       "runtimeExecutable": "wskdebug",
       "args": [
           "wkndAemAssetCompute-0.0.1/__secured_worker",  // Version must match your Asset Compute worker's version
           "${workspaceFolder}/actions/worker/index.js",  // Points to your worker
           "-l",
           "--ngrok"
       ],
       "localRoot": "${workspaceFolder}",
       "remoteRoot": "/code",
       "outputCapture": "std",
       "timeout": 30000
   }
   ```

1. ドロップダウンから新しい __wskdebug__ を選択します。
1. wskdebugドロップダウンの左にある緑色の __「__ 実行 __」ボタンをタップします__ 。
1. 改行番号 `/actions/worker/index.js` の左側を開き、をタップして、改行点1を追加します。 手順6で開いたAsset Compute Development Tool Webブラウザウィンドウに移動します。
1. 「 __実行__ 」ボタンをタップして、ワーカーを実行します
1. VSコードに戻り、コードを順に実行 `/actions/worker/index.js` します。
1. デバッグ可能な開発ツールを終了するには、手順6 `Ctrl-C` のコマンドを実行した端末でをタップ `npx adobe-asset-compute devtool` します

## Adobe I/O Runtimeからのログへのアクセス{#aio-app-logs}

[AEMをCloud Serviceとして使用する場合は、Adobe I/O Runtimeで直接呼び出すことで、処理プロファイル](../deploy/processing-profiles.md) (Processing Worker)を介してAsset Computeワーカーを活用します。 これらの呼び出しにはローカル開発は関与しないので、その実行はAsset Compute Development Toolやwskdebugなどのローカルツールを使用してデバッグすることはできません。 代わりに、AdobeI/O CLIを使用して、Adobe I/O Runtimeの特定のワークスペースで実行されるワーカーからログを取得できます。

1. デバッグが必要なワ [ークスペースに基づいて、](../deploy/runtime.md) ワークスペース固有の環境変数 `AIO_runtime_namespace` が `AIO_runtime_auth`、およびを介して設定されていることを確認します。
1. コマンドラインから、 `aio app logs`
   + ワークスペースに大量のトラフィックが発生している場合は、次の `--limit` フラグを使用してアクティベーションログの数を拡張します。
      `$ aio app logs --limit=25`
1. レビュー用のコマンドの出力として、最新(指定された `--limit`)のアクティベーションログが返されます。

   ![aioアプリログ](./assets/debug/aio-app-logs.png)

## トラブルシューティング

### デバッガがアタッチしません

+ __エラー__:起動の処理中にエラーが発生しました：エラー：次の時点でデバッグターゲットに接続できませんでした…
+ __原因__:Docker Desktopがローカルシステムで実行されていません。 VSコードデバッグコンソール(表示/デバッグコンソール)を確認して、このエラーがレポートされることを確認します。
+ __解像度__:開始 [ドッカーデスクトップで、必要なドッカーイメージがインストールされていることを確認し](../set-up/development-environment.md#docker)ます。

### ブレークポイントが一時停止されない

+ __エラー__:デバッグ可能な開発ツールからアセット計算ワーカーを実行する場合、VSコードはブレークポイントで一時停止しません。

#### VSコードデバッガがアタッチされていません

+ __原因：__ VSコードデバッガが停止または切断されました。
+ __解像度：__ VSコードデバッガを再起動し、VSコードデバッグ出力コンソール(表示/デバッグコンソール)を見て接続を確認します。

#### ワーカーの実行開始後に添付されたVSコードデバッガ

+ __原因：__ 「開発ツールで __実行__ 」をタップする前に、VSコードデバッガーがアタッチされませんでした。
+ __解像度：__ VSコードのデバッグコンソール(表示/デバッグコンソール)を確認し、デバッガがアタッチされていることを確認してから、開発ツールからAsset Compute workerを再実行します。

### デバッグ中に作業者がタイムアウトする

+ __エラー__:Debug Consoleレポートに「Action will timeout in -XXX milliseconds」または [Asset Compute Development Toolの](../develop/development-tool.md) Renditionプレビューが無限にスピンするか、
+ __原因__:デバッグ中に [manifest.ymlで定義されているワーカーのタイムアウトを超えました](../develop/manifest.md) 。
+ __解像度__:ワーカーのタイムアウトを [manifest.ymlで一時的に増やすか](../develop/manifest.md) 、デバッグアクティビティを高速化します。

### デバッガープロセスを終了できません

+ __エラー__: `Ctrl-C` コマンドラインでデバッガプロセスが終了しない(`npx adobe-asset-compute devtool`)。
+ __原因__:1.3.xのバグ `@adobe/aio-cli-plugin-asset-compute` は、終了コマンドとして認識され `Ctrl-C` ません。
+ __解像度__:バージョン1.4.1以降 `@adobe/aio-cli-plugin-asset-compute` へのアップデート

   ```
   $ aio update
   ```

   ![トラブルシューティング — aioの更新](./assets/debug/troubleshooting__terminate.png)