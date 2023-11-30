---
title: asset computeワーカーのテスト
description: asset computeプロジェクトは、Asset computeワーカーのテストを簡単に作成して実行するためのパターンを定義します。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
jira: KT-6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '629'
ht-degree: 2%

---

# asset computeワーカーのテスト

asset computeプロジェクトは、簡単に作成および実行できるパターンを定義します [asset compute労働者の試験](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html).

## ワーカーテストの分析

Asset computeワーカーのテストは、テストスイートに分類され、各テストスイート内で、テスト条件をアサートする 1 つ以上のテストケースが実行されます。

asset computeプロジェクトのテストの構造は次のとおりです。

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker, must match the yaml key for this worker in manifest.yml
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

各テストキャストには、以下のファイルを含めることができます。

+ `file.<extension>`
   + テストするソースファイル（拡張子は次の場合を除く任意のファイル） `.link`)
   + 必須
+ `rendition.<extension>`
   + 期待されるレンディション
   + 必須（エラーテストを除く）
+ `params.json`
   + 単一レンディションの JSON 命令
   + オプション
+ `validate`
   + 期待される実際のレンディションファイルパスを引数として取得し、結果に問題がない場合は終了コード 0 を返し、検証または比較に失敗した場合はゼロ以外の終了コードを返すスクリプト。
   + オプション。デフォルトは `diff` command
   + 異なる検証ツールを使用するために、Docker 実行コマンドをラップするシェルスクリプトを使用します。
+ `mock-<host-name>.json`
   + JSON 形式の HTTP 応答： [外部サービス呼び出しのモック](https://www.mock-server.com/mock_server/creating_expectations.html).
   + オプション。ワーカーコードが独自の HTTP リクエストを実行する場合にのみ使用されます。

## テストケースの書き込み

このテストケースは、パラメータ化された入力 (`params.json`) を入力ファイル (`file.jpg`) は、期待される PNG レンディション (`rendition.png`) をクリックします。

1. 最初に自動生成された `simple-worker` 次の場合のテストケース： `/test/asset-compute/simple-worker` これは無効なので、ワーカーはソースをレンディションにコピーするだけではなくなりました。
1. 次の場所に新しいテストケースフォルダーを作成します。 `/test/asset-compute/worker/success-parameterized` :PNG レンディションを生成するワーカーの正常な実行をテストします。
1. Adobe Analytics の `success-parameterized` フォルダーにテストを追加します。 [入力ファイル](./assets/test/success-parameterized/file.jpg) このテストケースの `file.jpg`.
1. Adobe Analytics の `success-parameterized` フォルダー、新しいファイルを追加します。 `params.json` ワーカーの入力パラメーターを定義する

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   これらは、 [開発ツールのAsset computeプロファイル定義](../develop/development-tool.md)、 `worker` キー。

1. 期待される [レンディションファイル](./assets/test/success-parameterized/rendition.png) このテストケースにを追加します。 `rendition.png`. このファイルは、指定された入力に対するワーカーの期待される出力を表します `file.jpg`.
1. コマンドラインから、次のコマンドを実行して、プロジェクトルートをテストします。 `aio app test`
   + 確認 [Docker Desktop](../set-up/development-environment.md#docker) と、サポートする Docker イメージがインストールされ、起動されている
   + 実行中の開発ツールインスタンスを終了します

![テスト — 成功 ](./assets/test/success-parameterized/result.png)

## テストケースのエラーチェックを書き込み中

このテストケースでは、 `contrast` パラメーターが無効な値に設定されています。

1. 次の場所に新しいテストケースフォルダーを作成します。 `/test/asset-compute/worker/error-contrast` 無効のための誤った実行をテストする `contrast` パラメーター値。
1. Adobe Analytics の `error-contrast` フォルダーにテストを追加します。 [入力ファイル](./assets/test/error-contrast/file.jpg) このテストケースの `file.jpg`. このファイルの内容はこのテストには重要ではありません。 `rendition.instructions` 有効性チェック：このテストケースが検証することを示します。
1. Adobe Analytics の `error-contrast` フォルダー、新しいファイルを追加します。 `params.json` これは、コンテンツを含むワーカーの入力パラメーターを定義します。

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + 設定 `contrast` パラメータ `10`に値を指定しない場合は、-1 ～ 1 の間のコントラストが無効です。 `RenditionInstructionsError`.
   + テストで適切なエラーをアサートするには、 `errorReason` キーを使用して、予期されるエラーに関連付けられた「reason」を取得します。 この無効なコントラストパラメーターは、 [カスタムエラー](../develop/worker.md#errors), `RenditionInstructionsError`、したがって、 `errorReason` このエラーの理由、または`rendition_instructions_error` こう言うと、

1. エラーの実行中にレンディションを生成しないので、レンディションは生成しない `rendition.<extension>` ファイルが必要です。
1. コマンドを実行して、プロジェクトのルートからテストスイートを実行します。 `aio app test`
   + 確認 [Docker Desktop](../set-up/development-environment.md#docker) と、サポートする Docker イメージがインストールされ、起動されている
   + 実行中の開発ツールインスタンスを終了します

![テスト — エラーコントラスト](./assets/test/error-contrast/result.png)

## Github のテストケース

最終的なテストケースは、GitHub で次の場所に公開されています。

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## トラブルシューティング

+ [テストの実行中にレンディションが生成されませんでした](../troubleshooting.md#test-no-rendition-generated)
+ [テストで誤ったレンディションが生成される](../troubleshooting.md#tests-generates-incorrect-rendition)
