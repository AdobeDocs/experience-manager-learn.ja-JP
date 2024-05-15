---
title: Asset Compute ワーカーのテスト
description: Asset Compute プロジェクトは、Asset compute ワーカーのテストを素早く作成して実行するためのパターンを定義します。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
duration: 142
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 100%

---

# Asset Compute ワーカーのテスト

Asset Compute プロジェクトは、[Asset Compute ワーカー](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html?lang=ja)のテストを素早く作成して実行するためのパターンを定義します。

## ワーカーテストの分析

Asset Compute ワーカーのテストはテストスイートに分けられ、各テストスイート内では、テスト条件をアサートする 1 つまたは複数のテストケースがあります。

Asset Compute プロジェクトのテストの構造は次のとおりです。

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
   + テストするソースファイル（`.link` 以外の任意の拡張子）
   + 必須
+ `rendition.<extension>`
   + 期待されるレンディション
   + 必須（エラーテストの場合を除く）
+ `params.json`
   + 単一レンディションの JSON 命令
   + オプション
+ `validate`
   + 期待される実際のレンディションファイルのパスを引数として取得し、結果に問題がない場合は終了コード 0 を返し、検証または比較に失敗した場合は 0 以外の終了コードを返すスクリプト。
   + オプション。デフォルトは `diff` コマンド
   + 様々な検証ツールを使用するために、Docker run コマンドをラップするシェルスクリプトを使用
+ `mock-<host-name>.json`
   + [外部サービス呼び出しをモックする](https://www.mock-server.com/mock_server/creating_expectations.html)ための JSON 形式の HTTP 応答。
   + オプション。ワーカーコードが独自の HTTP リクエストを実行する場合にのみ使用

## テストケースの記述

このテストケースでは、入力ファイル（`file.jpg`）のパラメータ化された入力（`params.json`）から期待される PNG レンディション（`rendition.png`）が生成されるとアサートします。

1. 最初に、ワーカーはソースをレンディションにコピーするだけではなくなったので、`/test/asset-compute/simple-worker` にある自動生成されたテストケース `simple-worker` を削除します。
1. PNG レンディションを生成するワーカーの正常な実行をテストするための、新しいテストケースフォルダーを `/test/asset-compute/worker/success-parameterized` に作成します。
1. `success-parameterized` フォルダーに、このテストケースのテスト[入力ファイル](./assets/test/success-parameterized/file.jpg)を追加し、ファイル名を `file.jpg` とします。
1. `success-parameterized` フォルダーに、ワーカーの入力パラメーターを定義する `params.json` という名前の新しいファイルを追加します。

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   これらは、[開発ツールの Asset Compute プロファイル定義](../develop/development-tool.md)に渡される同じキーと値から、`worker` キーを除いたものです。

1. 期待される[レンディションファイル](./assets/test/success-parameterized/rendition.png)をこのテストケースに追加し、ファイル名を `rendition.png` とします。このファイルは、指定された入力 `file.jpg` に対するワーカーの期待される出力を表します。
1. コマンドラインから、`aio app test` を実行してプロジェクトルートをテストします。
   + [Docker Desktop](../set-up/development-environment.md#docker) とサポートする Docker イメージがインストール、実行されていることを確認します
   + 実行中の開発ツールインスタンスを終了します

![テスト - 成功](./assets/test/success-parameterized/result.png)

## テストケースのエラーチェックの記述

このテストケースでは、`contrast` パラメーターが無効な値に設定された場合に、ワーカーが適切なエラーをスローすることをテストします。

1. `contrast` パラメーター値が無効なためにワーカーがエラーを実行するかどうかをテストする、新しいテストケースフォルダーを `/test/asset-compute/worker/error-contrast` に作成します。
1. `error-contrast` フォルダーに、このテストケースのテスト[入力ファイル](./assets/test/error-contrast/file.jpg)を追加し、ファイル名を `file.jpg` とします。このファイルの内容は、このテストには重要ではありません。このテストケースで検証する `rendition.instructions` の有効性チェックに到達するための「破損したソース」チェックの通過に必要です。
1. `error-contrast` フォルダーに、次のコンテンツを含むワーカーの入力パラメーターを定義する `params.json` という名前の新しいファイルを追加します。

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + コントラストは -1 ～ 1 の範囲である必要があるので、`contrast` パラメーターに無効な値「`10`」を設定し、`RenditionInstructionsError` がスローされるようにします。
   + `errorReason` キーを、予期されるエラーに関連付けられた「理由」に設定し、テストで適切なエラーがスローされるとアサートします。この無効なコントラストパラメーターは、[カスタムエラー](../develop/worker.md#errors) `RenditionInstructionsError` をスローするので、`errorReason` にこのエラーの理由 `rendition_instructions_error` を設定し、これがスローされるとアサートします。

1. エラーの実行中にレンディションは生成されないので、`rendition.<extension>` ファイルは必要ありません。
1. `aio app test` コマンドを実行して、プロジェクトのルートからテストスイートを実行します。
   + [Docker Desktop](../set-up/development-environment.md#docker) とサポートする Docker イメージがインストール、実行されていることを確認します
   + 実行中の開発ツールインスタンスを終了します

![テスト - コントラストのエラー](./assets/test/error-contrast/result.png)

## Github のテストケース

最終的なテストケースは GitHub で次の場所から入手できます。

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## トラブルシューティング

+ [テストの実行中にレンディションが生成されませんでした](../troubleshooting.md#test-no-rendition-generated)
+ [テストで、間違ったレンディションが生成されます](../troubleshooting.md#tests-generates-incorrect-rendition)
