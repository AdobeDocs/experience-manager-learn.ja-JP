---
title: asset computeワーカーのテスト
description: asset computeプロジェクトは、Asset computeワーカーのテストを簡単に作成し、実行するためのパターンを定義します。
feature: asset computeマイクロサービス
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
topic: 統合、開発
role: Developer
level: Intermediate, Experienced
source-git-commit: dbc0a35ae96594fec1e10f411d57d2a3812c1cf2
workflow-type: tm+mt
source-wordcount: '636'
ht-degree: 1%

---


# asset computeワーカーのテスト

asset computeプロジェクトは、Asset computeワーカー](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html)の[テストを簡単に作成し、実行するためのパターンを定義します。

## ワーカーテストの分析

asset computeワーカーのテストはテストスイートに分類され、各テストスイート内で、テスト条件をアサートする1つ以上のテストケースが実行されます。

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

各テストキャストには、次のファイルを含めることができます。

+ `file.<extension>`
   + テストするソースファイル（拡張子は`.link`以外の任意のファイル）
   + 必須
+ `rendition.<extension>`
   + 予想されるレンディション
   + 必須（エラーテストを除く）
+ `params.json`
   + 単一レンディションのJSON命令
   + オプション
+ `validate`
   + 期待される実際のレンディションファイルパスを引数として取得し、結果に問題がない場合は終了コード0を、検証または比較に失敗した場合はゼロ以外の終了コードを返すスクリプト。
   + オプション。デフォルトは`diff`コマンドです。
   + 様々な検証ツールを使用するために、Docker実行コマンドをラップするシェルスクリプトを使用します。
+ `mock-<host-name>.json`
   + [モック外部サービス呼び出し](https://www.mock-server.com/mock_server/creating_expectations.html)に対するJSON形式のHTTP応答。
   + オプション。ワーカーコードが独自のHTTPリクエストをおこなう場合にのみ使用されます

## テストケースの記述

このテストケースでは、入力ファイル(`file.jpg`)のパラメーター化された入力(`params.json`)をアサートし、期待されるPNGレンディション(`rendition.png`)を生成します。

1. `/test/asset-compute/simple-worker`にある自動生成`simple-worker`テストケースは、無効なので削除します。ワーカーは、ソースをレンディションにコピーするだけではなくなるからです。
1. `/test/asset-compute/worker/success-parameterized`に新しいテストケースフォルダーを作成し、PNGレンディションを生成するワーカーの正常な実行をテストします。
1. `success-parameterized`フォルダーに、このテストケースのテスト[入力ファイル](./assets/test/success-parameterized/file.jpg)を追加し、`file.jpg`という名前を付けます。
1. `success-parameterized`フォルダーに、ワーカーの入力パラメーターを定義する`params.json`という名前の新しいファイルを追加します。

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   これらは、[開発ツールのAsset computeプロファイル定義](../develop/development-tool.md)に渡されるキーと値（`worker`キーより小さい）と同じです。

1. 必要な[レンディションファイル](./assets/test/success-parameterized/rendition.png)をこのテストケースに追加し、`rendition.png`という名前を付けます。 このファイルは、指定された入力`file.jpg`に対してワーカーが期待する出力を表します。
1. コマンドラインから、`aio app test`を実行して、プロジェクトルートをテストします。
   + [Docker Desktop](../set-up/development-environment.md#docker)とサポートするDockerイメージがインストールされ、起動されていることを確認します。
   + 実行中の開発ツールインスタンスを終了します

![テスト — 成功  ](./assets/test/success-parameterized/result.png)

## エラーチェックのテストケースの書き込み

このテストケースでは、`contrast`パラメーターが無効な値に設定されている場合に、ワーカーが適切なエラーをスローするかどうかをテストします。

1. `/test/asset-compute/worker/error-contrast`に新しいテストケースフォルダーを作成し、`contrast`パラメーター値が無効なためにワーカーの誤った実行をテストします。
1. `error-contrast`フォルダーに、このテストケースのテスト[入力ファイル](./assets/test/error-contrast/file.jpg)を追加し、`file.jpg`という名前を付けます。 このファイルの内容はこのテストには重要ではありません。`rendition.instructions`の有効性チェックに到達するには、「破損したソース」チェックを超えて存在する必要があります。このテストケースが検証します。
1. `error-contrast`フォルダーに、ワーカーの入力パラメーターとコンテンツを定義する`params.json`という名前の新しいファイルを追加します。

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + `contrast`パラメーターを`10`に設定します。この値は無効です。コントラストは —1から1の間である必要があり、`RenditionInstructionsError`をスローします。
   + `errorReason`キーを、予想されるエラーに関連する「理由」に設定することで、テストで適切なエラーをアサートします。 この無効なコントラストパラメーターは、[カスタムエラー](../develop/worker.md#errors)、`RenditionInstructionsError`をスローするので、`errorReason`をこのエラーの理由に設定するか、`rendition_instructions_error`をスローするように設定します。

1. エラーの実行中にレンディションを生成しないので、`rendition.<extension>`ファイルは不要です。
1. コマンド`aio app test`を実行して、プロジェクトのルートからテストスイートを実行します。
   + [Docker Desktop](../set-up/development-environment.md#docker)とサポートするDockerイメージがインストールされ、起動されていることを確認します。
   + 実行中の開発ツールインスタンスを終了します

![テスト — エラーコントラスト](./assets/test/error-contrast/result.png)

## Githubのテストケース

最終的なテストケースは、GitHubで次の場所から入手できます。

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## トラブルシューティング

+ [テストの実行中にレンディションが生成されない](../troubleshooting.md#test-no-rendition-generated)
+ [テストで誤ったレンディションが生成される](../troubleshooting.md#tests-generates-incorrect-rendition)
