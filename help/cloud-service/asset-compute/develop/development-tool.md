---
title: asset compute開発ツール
description: asset compute開発ツールは、AEM SDKのコンテキスト外で、Adobe I/O RuntimeのAsset computeリソースに対してSDKのコンテキストを使用せずに、開発者がAsset Computerワーカーをローカルで設定および実行できる、ローカルWebハーネスです。
feature: asset computeマイクロサービス
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: 統合、開発
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '541'
ht-degree: 0%

---


# asset compute開発ツール

asset compute開発ツールは、AEM SDKのコンテキスト外で、Adobe I/O RuntimeのAsset computeリソースに対してSDKのコンテキストを使用せずに、開発者がAsset Computerワーカーをローカルで設定および実行できる、ローカルWebハーネスです。

## asset compute開発ツールの実行

asset compute開発ツールは、ターミナルコマンドを使用して、Asset computeプロジェクトのルートから実行できます。

```
$ aio app run
```

__http://localhost:9000__&#x200B;で開発ツールが起動し、自動的にブラウザーウィンドウで開きます。 開発ツールを実行するには、[有効な自動生成devToolTokenをクエリパラメーター](#troubleshooting__devtooltoken)を介して提供する必要があります。

## asset compute開発ツールインターフェイスについて{#interface}

![asset compute開発ツール](./assets/development-tool/asset-compute-dev-tool.png)

1. __ソースファイル：__ ソースファイルの選択は、次の目的で使用されます。
   + asset computeワーカーに渡される`source`バイナリとなるアセットバイナリを選択
   + ソースファイルのアップロード
1. __asset computeプロファイルの定義：__ 実行するAsset computeワーカーを定義します。パラメーターは次のとおりです。ワーカーのURLエンドポイント、結果のレンディション名、任意のパラメーターを含める
1. __実行：__ 「実行」ボタンをクリックすると、Asset compute設定プロファイルエディターで定義したAsset computeプロファイルが実行されます
1. __中止：__ 「中止」ボタンをクリックすると、「実行」ボタンをタップして開始した実行がキャンセルされます
1. __リクエスト/レスポンス：__ Adobe I/O Runtimeで実行されているAsset computeワーカーに対するHTTPリクエストとレスポンスを提供します。これはデバッグに役立ちます。
1. __アクティベーションログ：__ Asset computeワーカーの実行を説明するログとエラー。この情報は、`aio app run`標準でも入手できます
1. __レンディション：__ Asset computeワーカーの実行によって生成されたすべてのレンディションを表示します
1. __devToolTokenクエリパラメーター：__ Asset compute開発ツールトークンには、有効なクエリ `devToolToken` パラメーターが必要です。このトークンは、新しい開発ツールが生成されるたびに自動的に生成されます

### カスタムワーカーの実行

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_開発ツールでのAsset compute作業のクリックスルー（オーディオなし）_

1. `aio app run`コマンドを使用して、Asset compute開発ツールがプロジェクトのルートから起動されていることを確認します。
1. asset compute開発ツールで、[サンプル画像ファイル](../assets/samples/sample-file.jpg)をアップロードまたは選択します。
   + 「__ソースファイル__」ドロップダウンでファイルが選択されていることを確認します。
1. __Asset computeプロファイル定義__&#x200B;のテキスト領域を確認します。
   + `worker`キーは、デプロイ済みのAsset computeワーカーのURLを定義します
   + `name`キーは、生成するレンディションの名前を定義します
   + このJSONオブジェクトには他のキーと値を指定でき、`rendition.instructions`オブジェクトの下のワーカーで使用できます
      + オプションで、`size`、`contrast`および`brightness`の値を追加します。

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

1. 「__実行__」ボタンをタップします。
1. __レンディションセクション__&#x200B;には、レンディションプレースホルダーが設定されます
1. ワーカーが完了すると、レンディションプレースホルダーに生成されたレンディションが表示されます

開発ツールの実行中にワーカーコードにコード変更を加えると、変更が「ホットデプロイ」されます。 「ホットデプロイ」には数秒かかるので、開発ツールからワーカーを再実行する前にデプロイを完了できます。

## トラブルシューティング

+ [YAMLインデントが正しくありません](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize制限が小さすぎます](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [private.keyが見つからないため、開発ツールを開始できません](../troubleshooting.md#missing-private-key)
+ [ソースファイルのドロップダウンが正しくない](../troubleshooting.md#source-files-dropdown-incorrect)
+ [devToolTokenクエリパラメーターが見つからないか、無効です](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [ソースファイルを削除できません](../troubleshooting.md#unable-to-remove-source-files)
+ [レンディションが部分的に描画されたか破損しています](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
