---
title: Asset Compute ワーカーのデバッグ
description: Asset Compute ワーカーは、単純なデバッグログステートメント、リモートデバッガーとして添付された VS Code、AEM as a Cloud Service から開始された Adobe I/O Runtime でのアクティベーションログの取り込みなど、様々な方法でデバッグできます。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
duration: 229
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '618'
ht-degree: 100%

---

# Asset Compute ワーカーのデバッグ

Asset Compute ワーカーは、単純なデバッグログステートメント、リモートデバッガーとして添付された VS Code、AEM as a Cloud Service から開始された Adobe I/O Runtime でのアクティベーションログの取り込みなど、様々な方法でデバッグできます。

## ログ

Asset Compute ワーカーのデバッグで最も基本的な形式は、ワーカーコードで従来の `console.log(..)` ステートメントを使用することです。`console` JavaScript オブジェクトは暗黙的なグローバルオブジェクトであり、すべてのコンテキストに常に存在するので、読み込む必要はありません。

次のログステートメントは、Asset Compute ワーカーの実行方法に応じてレビューで様々に使用できます。

+ `aio app run` から、ログは標準出力と[開発ツール](../develop/development-tool.md)のアクティベーションログにプリント
  ![aio app run console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ `aio app test`から、ログは `/build/test-results/test-worker/test.log` にプリント
  ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ `wskdebug` を使用して、ログはステートメントを VS Code デバッグコンソール（表示／デバッグコンソール）、標準出力にプリント
  ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ `aio app logs` を使用して、ログはステートメントをアクティベーションログ出力にプリント

## アタッチされたデバッガーを介したリモートデバッグ

>[!WARNING]
>
>wskdebug との互換性を確保するには、Microsoft Visual Studio Code 1.48.0 以降を使用

[wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm モジュールは、Asset Compute ワーカーへのデバッガーのアタッチをサポートしています。これには、VS Code でブレークポイントを設定し、コードを順を追って設定する機能が含まれます。

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_wskdebug を使用した Asset Compute ワーカーのデバッグのクリックスルー（オーディオなし）_

1. [wskdebug](../set-up/development-environment.md#wskdebug) および [ngrok](../set-up/development-environment.md#ngork) npm モジュールがインストールされていることを確認します
1. [Docker Desktop とサポートする Docker イメージ](../set-up/development-environment.md#docker)がインストール、実行されていることを確認します
1. 開発ツールの実行中のアクティブなインスタンスをすべて閉じます。
1. `aio app deploy` を使用して最新のコードをデプロイし、デプロイ済みのアクション名（`[...]` の間にある名前）を記録します。これは、手順 8 の `launch.json` の更新に使用できます。

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. コマンド `npx adobe-asset-compute devtool` を使用して、Asset Compute 開発ツールの新しいインスタンスを開始します。 
1. VS Code で、左側のナビゲーションのデバッグアイコンをタップします。
   + プロンプトが表示されたら、 __launch.json ファイルを作成／Node.js__&#x200B;をタップして、新しい `launch.json` ファイルを作成します。
   + それ以外の場合は、「__起動プログラム__」ドロップダウンの右にある&#x200B;__歯車__&#x200B;アイコンをタップして、エディターで既存の `launch.json` を開きます。
1. `configurations` 配列に次の JSON オブジェクト設定を追加します。

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
1. __wskdebug__ ドロップダウンの左にある緑の「__実行__」ボタンをタップします。
1. `/actions/worker/index.js` を開いて、行番号の左側をタップして、ブレークポイント 1 を追加します。手順 6 で開いた、Asset Compute 開発ツール web ブラウザーウィンドウに移動します。
1. 「__実行__」ボタンをタップしてワーカーを実行します。
1. VS Code、`/actions/worker/index.js` に戻り、コードにアクセスします。
1. デバッグ可能な開発ツールを終了するには、 手順 6 で `npx adobe-asset-compute devtool` コマンドを実行したターミナルで `Ctrl-C` をタップします。

## Adobe I/O Runtime からのログへのアクセス{#aio-app-logs}

Adobe I/O Runtime で直接呼び出すことで、[AEM as a Cloud Service は、処理プロファイルを介して Asset Compute ワーカーを活用します](../deploy/processing-profiles.md)。これらの呼び出しはローカル開発を伴わないので、Asset Compute 開発ツールや wskdebug などのローカルツールを使用して実行をデバッグすることはできません。 代わりに、Adobe I/O CLI を使用して、Adobe I/O Runtime の特定のワークスペースで実行されたワーカーからログを取得できます。

1. デバッグが必要なワークスペースに基づいて、[ワークスペース固有の環境変数](../deploy/runtime.md)が `AIO_runtime_namespace` および `AIO_runtime_auth` を介して設定されいることを確認します。
1. コマンドラインから `aio app logs` を実行します。
   + ワークスペースで大量のトラフィックが発生する場合は、`--limit` フラグからアクティベーションログの数を増やします。
     `$ aio app logs --limit=25`
1. 最新のアクティベーションログ（最大は指定された `--limit`）は、レビュー用のコマンド出力として返されます。

   ![aio app logs](./assets/debug/aio-app-logs.png)

## トラブルシューティング

+ [デバッガーがアタッチしない](../troubleshooting.md#debugger-does-not-attach)
+ [ブレークポイントが一時停止しない](../troubleshooting.md#breakpoints-no-pausing)
+ [VS Code デバッガーがアタッチされていない](../troubleshooting.md#vs-code-debugger-not-attached)
+ [ワーカーの実行が開始した後にVS コードデバッガーがアタッチされる](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [デバッグ中にワーカーがタイムアウトする](../troubleshooting.md#worker-times-out-while-debugging)
+ [デバッガープロセスを終了できない](../troubleshooting.md#cannot-terminate-debugger-process)
