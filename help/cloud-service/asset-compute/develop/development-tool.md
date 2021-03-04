---
title: asset compute開発ツール
description: asset compute開発ツールは、Adobe I/O RuntimeのAsset computeリソースに対するAEM SDKのコンテキストを除き、開発者がローカルでAsset Computer Workerを設定および実行できるローカルWebハーネスです。
feature: asset computeマイクロサービス
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: 統合、開発
role: デベロッパー
level: 中級、経験豊富
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 0%

---


# asset compute開発ツール

asset compute開発ツールは、Adobe I/O RuntimeのAsset computeリソースに対するAEM SDKのコンテキストを除き、開発者がローカルでAsset Computer Workerを設定および実行できるローカルWebハーネスです。

## asset compute開発ツールの実行

asset compute開発ツールは、ターミナルコマンドを使用して、Asset computeプロジェクトのルートから実行できます。

```
$ aio app run
```

これにより、__http://localhost:9000__&#x200B;に開発ツールが開始され、自動的にブラウザーウィンドウで開きます。 開発ツールを実行するには、[有効な自動生成devToolTokenをクエリパラメーター](#troubleshooting__devtooltoken)を介して指定する必要があります。

## asset compute開発ツールのインターフェイスを理解する{#interface}

![asset compute開発ツール](./assets/development-tool/asset-compute-dev-tool.png)

1. __ソースファイル：ソ__ ースファイルの選択は、次の目的に使用されます。
   + asset computeワーカーに渡される`source`バイナリとなるアセットバイナリを選択しました
   + ソースファイルのアップロード
1. __asset computeプロファイルの定義：実行するAsset computeワーカーを__ 定義し、次のパラメーターを含めます。ワーカーのURLエンドポイント、結果のレンディション名、および任意のパラメーターを含めます。
1. __実行：「実行」ボタン__ は、Asset compute設定プロファイルエディターの定義に従ってAsset computeプロファイルを実行します
1. __中止：「中止」ボタン__ をクリックすると、「実行」ボタンをタップしたときに開始された実行がキャンセルされます
1. __リクエスト/レスポンス：Adobe I/O Runtimeで実行しているAsset computeワーカーに対するHTTPリクエストおよびレスポンスを__ 提供します。これは、デバッグに役立ちます。
1. __アクティベーションログ：Asset compute__ ワーカーの実行とエラーを説明するログ。この情報は`aio app run`規格外でも入手できます
1. __レンディション：Asset computeワーカーの実行によって生成されたすべてのレンディションを__ 表示します
1. __devToolTokenクエリパラメーター：Asset compute開発ツ__ ールトークンには、有効な `devToolToken` クエリパラメーターが必要です。このトークンは、新しい開発ツールが起動されるたびに自動的に生成されます。

### カスタムワーカーの実行

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_開発ツールでのAsset compute作業のクリックスルー（オーディオなし）_

1. `aio app run`コマンドを使用して、Asset compute開発ツールがプロジェクトルートから起動していることを確認します。
1. asset compute開発ツールで、[サンプル画像ファイル](../assets/samples/sample-file.jpg)をアップロードまたは選択します。
   + __「ソースファイル__」ドロップダウンでファイルが選択されていることを確認します
1. __Asset computeプロファイル定義__&#x200B;のテキスト領域を確認します。
   + `worker`キーは、デプロイされたAsset computeワーカーへのURLを定義します
   + `name`キーは生成するレンディションの名前を定義します
   + 他のキー/値は、このJSONオブジェクトに指定でき、`rendition.instructions`オブジェクトの下のワーカーで使用できます
      + 必要に応じて、`size`、`contrast`、`brightness`の値を追加します。

         ```json
         {
             "renditions": [
                 {
                     "worker": "...",
                     "name": "rendition.png",
                     "size":"800",
                     "contrast": "0.30",
                     "brightness": "-0.15"
                 }
             ]
         }
         ```

1. 「__実行__」ボタンをタップします
1. __レンディションセクション__&#x200B;には、レンディションのプレースホルダーが設定されます
1. ワーカーが完了すると、レンディションのプレースホルダーに生成されたレンディションが表示されます

開発ツールの実行中にワーカーコードにコード変更を加えると、変更内容が「ホットデプロイ」されます。 「ホットデプロイ」は数秒かかるので、開発ツールからワーカーを再実行する前にデプロイを完了できます。

## トラブルシューティング

+ [誤ったYAMLインデント](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize制限が低すぎます](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [private.keyが見つからないため、開発ツールは開始できません](../troubleshooting.md#missing-private-key)
+ [ソースファイルのドロップダウンが正しくない](../troubleshooting.md#source-files-dropdown-incorrect)
+ [devToolTokenクエリパラメーターがないか、無効です](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [ソースファイルを削除できません](../troubleshooting.md#unable-to-remove-source-files)
+ [レンディションが部分的に描画または破損して返されました](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
