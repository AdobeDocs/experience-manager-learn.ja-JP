---
title: asset computeワーカーのデバッグ
description: asset computeワーカーは、簡単なデバッグログ文から、接続されたVS Code（リモートデバッガ）、AEMでCloud Serviceとして開始されたAdobe I/O Runtimeのアクティベーションのログを取り込むまで、様々な方法でデバッグできます。
feature: asset computeマイクロサービス
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
topic: 統合、開発
role: デベロッパー
level: 中級、経験豊富
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 0%

---


# asset computeワーカーのデバッグ

asset computeワーカーは、簡単なデバッグログ文から、接続されたVS Code（リモートデバッガ）、AEMでCloud Serviceとして開始されたAdobe I/O Runtimeのアクティベーションのログを取り込むまで、様々な方法でデバッグできます。

## ログ

asset computeワーカーのデバッグで最も基本的な形式は、ワーカーコード内で従来の`console.log(..)`文を使用します。 `console` JavaScriptオブジェクトは暗黙的なグローバルオブジェクトなので、読み込みや要求を行う必要はありません。これは、すべてのコンテキストで常に存在するためです。

これらのログ文は、Asset computeワーカーの実行方法に応じて、様々な方法でレビューできます。

+ `aio app run`から、ログは標準出力に出力され、[開発ツールの](../develop/development-tool.md)アクティベーションログに出力されます。
   ![aioアプリ実行console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ `aio app test`から`/build/test-results/test-worker/test.log`にログを印刷
   ![aioアプリテストコンソール.log(...)](./assets/debug/console-log__aio-app-test.png)
+ `wskdebug`を使用すると、ログ文はVSコードデバッグコンソール(表示/デバッグコンソール)に出力され、標準出力されます。
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ `aio app logs`を使用して、ログ文をアクティベーションログ出力に出力します。

## 添付されたデバッガーを使用したリモートデバッグ

>[!WARNING]
>
>wskdebugとの互換性を確保するために、Microsoft Visual Studio Code 1.48.0以降を使用する

[wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npmモジュールは、VSコードにブレークポイントを設定し、コードをステップスルーする機能など、Asset computeワーカーへのデバッガの接続をサポートしています。

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_wskdebugを使用したAsset computeワーカーのデバッグのクリックスルー（オーディオなし）_

1. [wskdebug](../set-up/development-environment.md#wskdebug)と[ngrok](../set-up/development-environment.md#ngork) npmモジュールがインストールされていることを確認します。
1. [Docker DesktopとサポートするDockerイメージ](../set-up/development-environment.md#docker)がインストールされ、実行されていることを確認します。
1. 開発ツールのアクティブな実行中のインスタンスをすべて閉じます。
1. `aio app deploy`を使用して最新のコードをデプロイし、デプロイ済みのアクション名（`[...]`の間の名前）を記録します。 これは、手順8の`launch.json`を更新する際に使用します。

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```
1. `npx adobe-asset-compute devtool`コマンドを使用して、Asset compute開発ツールの新しいインスタンスを開始する
1. VSコードで、左側のナビゲーションのデバッグアイコンをタップします
   + プロンプトが表示されたら、__launch.jsonファイルを作成>Node.js__&#x200B;をタップして、新しい`launch.json`ファイルを作成します。
   + それ以外の場合は、「__プログラムを起動__」ドロップダウンの右にある&#x200B;__ギア__&#x200B;アイコンをタップして、エディター内の既存の`launch.json`を開きます。
1. 追加`configurations`配列に対する次のJSONオブジェクトの設定：

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

1. ドロップダウンから新しい&#x200B;__wskdebug__&#x200B;を選択します
1. __wskdebug__&#x200B;ドロップダウンの左にある緑色の&#x200B;__Run__&#x200B;ボタンをタップします
1. `/actions/worker/index.js`を開き、行番号の左側をタップして、ブレークポイント1を追加します。 手順6で開いたAsset compute開発ツールのWebブラウザ・ウィンドウに移動します。
1. 「__実行__」ボタンをタップして、ワーカーを実行します
1. VSコードに戻り、`/actions/worker/index.js`に戻り、コードを順に実行します
1. デバッグ可能な開発ツールを終了するには、手順6で`npx adobe-asset-compute devtool`コマンドを実行したターミナルで`Ctrl-C`をタップします

## Adobe I/O Runtime{#aio-app-logs}からログにアクセス中

[AEMをCloud Serviceとして使用する場合は、Adobe I/O Runtimeで直接呼び出すことで、処理プロファイルを介してAsset compute](../deploy/processing-profiles.md) ワーカーを活用します。これらの呼び出しにはローカル開発は関与しないので、Asset compute開発ツールやwskdebugなどのローカルツールを使用して実行をデバッグすることはできません。 代わりに、Adobe I/OCLIを使用して、Adobe I/O Runtimeの特定のワークスペースで実行されるワーカーからログを取得できます。

1. デバッグが必要なワークスペースに基づいて、[ワークスペース固有の環境変数](../deploy/runtime.md)が`AIO_runtime_namespace`と`AIO_runtime_auth`を介して設定されていることを確認します。
1. コマンドラインで`aio app logs`を実行します。
   + ワークスペースに大量のトラフィックが発生している場合は、`--limit`フラグを使用してアクティベーションログの数を増やします。
      `$ aio app logs --limit=25`
1. 最新（指定された`--limit`まで）のアクティベーションログは、レビュー用のコマンドの出力として返されます。

   ![aioアプリログ](./assets/debug/aio-app-logs.png)

## トラブルシューティング

+ [デバッガがアタッチしません](../troubleshooting.md#debugger-does-not-attach)
+ [ブレークポイントが一時停止されない](../troubleshooting.md#breakpoints-no-pausing)
+ [VSコードデバッガがアタッチされていません](../troubleshooting.md#vs-code-debugger-not-attached)
+ [ワーカーの実行開始後に添付されたVSコードデバッガ](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [デバッグ中に作業者がタイムアウトする](../troubleshooting.md#worker-times-out-while-debugging)
+ [デバッガープロセスを終了できません](../troubleshooting.md#cannot-terminate-debugger-process)
