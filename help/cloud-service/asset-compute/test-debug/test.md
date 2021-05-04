---
title: asset computeワーカーのテスト
description: asset computeプロジェクトは、Asset computeワーカーのテストを容易に作成し、実行するためのパターンを定義する。
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
translation-type: tm+mt
source-git-commit: 2efb7050b0b0c783c5f34c1f2e375cf21fa7a6cd
workflow-type: tm+mt
source-wordcount: '636'
ht-degree: 1%

---


# asset computeワーカーのテスト

asset computeプロジェクトは、Asset computeワーカー](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html)の[テストを容易に作成し、実行するためのパターンを定義する。

## ワーカーテストの解析

asset computeワーカーのテストはテストスイートに分類され、各テストスイート内で、テスト条件をアサートする1つ以上のテストケースが1つずつ示されます。

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

各テストキャストには次のファイルを含めることができます。

+ `file.<extension>`
   + テストするソースファイル（拡張子は`.link`以外の任意のファイル）
   + 必須
+ `rendition.<extension>`
   + 期待されるレンディション
   + 必須（エラーテストを除く）
+ `params.json`
   + 単一レンディションのJSONの手順
   + オプション
+ `validate`
   + 期待される実際のレンディションファイルパスを引数として取得し、結果が正常である場合は終了コード0を返し、検証または比較に失敗した場合は0以外の終了コードを返す必要があるスクリプト。
   + （オプション）デフォルトでは`diff`コマンドが使用されます
   + 異なる検証ツールを使用する場合に、dockerの実行コマンドをラップするシェルスクリプトを使用する
+ `mock-<host-name>.json`
   + [外部サービスの呼び出し](https://www.mock-server.com/mock_server/creating_expectations.html)に対するJSON形式のHTTP応答。
   + （オプション）ワーカーコードが独自のHTTP要求を行う場合にのみ使用します。

## テストケースの作成

このテストケースでは、入力ファイル(`file.jpg`)のパラメータ化入力(`params.json`)が、期待されるPNGレンディション(`rendition.png`)を生成することをアサートします。

1. まず、`/test/asset-compute/simple-worker`で自動生成された`simple-worker`テストのケースを削除します。これは無効なためです。ワーカーは、ソースをレンディションに単純にコピーするのではなくなります。
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

   これらは[開発ツールのAsset computeプロファイル定義](../develop/development-tool.md)に渡されるキー/値と同じで、`worker`キーより小さい値です。

1. このテ追加ストケースに対して[レンディションファイル](./assets/test/success-parameterized/rendition.png)が必要です。`rendition.png`という名前を付けます。 このファイルは、指定された入力`file.jpg`に対するワーカーの期待される出力を表します。
1. コマンドラインで`aio app test`を実行し、プロジェクトルートをテストします
   + [Docker Desktop](../set-up/development-environment.md#docker)およびサポートするDockerイメージがインストールされ、起動されていることを確認します。
   + 実行中の開発ツールインスタンスを終了します

![テスト — 成功  ](./assets/test/success-parameterized/result.png)

## テストケースのエラーチェックの書き込み

このテストケースでは、`contrast`パラメーターが無効な値に設定されている場合に、ワーカーが適切なエラーをスローするかどうかをテストします。

1. `/test/asset-compute/worker/error-contrast`に新しいテストケースフォルダーを作成し、`contrast`パラメーター値が無効なため、ワーカーの誤った実行をテストします。
1. `error-contrast`フォルダーに、このテストケースのテスト[入力ファイル](./assets/test/error-contrast/file.jpg)を追加し、`file.jpg`という名前を付けます。 このファイルの内容はこのテストには重要ではありません。「破損したソース」チェックを通り過ぎて`rendition.instructions`の有効性チェックに到達し、このテストケースが検証されるように、存在する必要があります。
1. `error-contrast`フォルダーに、`params.json`という名前の新しいファイルを追加します。このファイルに、ワーカーの入力パラメーターとコンテンツが定義されています。

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + `contrast`パラメーターを`10`に設定します。コントラストは —1 ～ 1の範囲である必要があります。この場合、`RenditionInstructionsError`がスローされます。
   + `errorReason`キーを、期待されたエラーに関連付けられた「reason」に設定することで、テストで適切なエラーがスローされることをアサートします。 この無効なコントラストパラメーターは、[カスタムエラー](../develop/worker.md#errors)、`RenditionInstructionsError`をスローするので、`errorReason`をこのエラーの理由に設定するか、`rendition_instructions_error`をスローするとアサートします。

1. 実行中にレンディションを生成する必要がないので、`rendition.<extension>`ファイルは不要です。
1. `aio app test`コマンドを実行して、プロジェクトのルートからテストスイートを実行します
   + [Docker Desktop](../set-up/development-environment.md#docker)およびサポートするDockerイメージがインストールされ、起動されていることを確認します。
   + 実行中の開発ツールインスタンスを終了します

![テスト — エラーコントラスト](./assets/test/error-contrast/result.png)

## Githubのテストケース

最終的なテストケースは、次の場所でGithubで入手できます。

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## トラブルシューティング

+ [テストの実行中にレンディションが生成されませんでした](../troubleshooting.md#test-no-rendition-generated)
+ [テストで誤ったレンディションが生成される](../troubleshooting.md#tests-generates-incorrect-rendition)
