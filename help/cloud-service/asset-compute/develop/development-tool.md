---
title: Asset Compute Development Tool
description: Asset Compute Development Toolは、ローカルWebハーネスです。開発者は、AEM SDKのコンテキスト外で、Adobe I/O RuntimeのAsset Computeリソースに対して、Asset Compute Workerをローカルで設定および実行できます。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---


# Asset Compute Development Tool

Asset Compute Development Toolは、ローカルWebハーネスです。開発者は、AEM SDKのコンテキスト外で、Adobe I/O RuntimeのAsset Computeリソースに対して、Asset Compute Workerをローカルで設定および実行できます。

## アセット計算開発ツールの実行

アセット計算開発ツールは、端末コマンドを使用して、アセット計算プロジェクトのルートから実行できます。

```
$ aio app run
```

これにより、http://localhost:9000に開発ツールが開始され __、自動的にブラウザーウィンドウで開きます__。 開発ツールを実行するには、有効な自動生成devToolToken [をクエリパラメーターを介して提供する必要があります](#troubleshooting__devtooltoken)。

## Asset Compute Development Toolsインターフェイスの理解{#interface}

![Asset Compute Development Tool](./assets/development-tool/asset-compute-dev-tool.png)

1. __ソースファイル：__ ソースファイルの選択は、次の目的に使用されます。
   + アセット計算ワーカーに渡される `source` バイナリとなるアセットバイナリを選択しました
   + ソースファイルのアップロード
1. __アセット計算プロファイルの定義：__ 実行するアセット計算ワーカーを定義します。パラメータは次のとおりです。ワーカーのURLエンドポイント、結果のレンディション名、および任意のパラメーターを含めます。
1. __実行：__ 「実行」ボタンは、アセット計算設定プロファイルエディタで定義された「アセット計算」プロファイルを実行します
1. __中止：__ 「Abort」ボタンをクリックすると、「Run」ボタンをタップして開始した実行がキャンセルされます
1. __リクエスト/応答：__ Adobe I/O Runtimeで実行されているAsset Compute Workerに対するHTTP要求と応答を提供します。 これは、デバッグに役立ちます。
1. __アクティベーションログ：__ アセット計算ワーカーの実行とエラーを説明するログ。 この情報は、 `aio app run` 標準出力でも利用できます
1. __レンディション：__ アセット計算ワーカーの実行によって生成されたすべてのレンディションを表示します
1. __devToolTokenクエリパラメーター：__ Asset Compute Development Toolトークンには、有効な `devToolToken` クエリパラメーターが必要です。 このトークンは、新しい開発ツールが起動されるたびに自動的に生成されます。

### カスタムワーカーの実行

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_開発ツールでのアセット計算作業の実行のクリックスルー（オーディオなし）_

1. コマンドを使用して、プロジェクトルートからアセット計算開発ツールが起動していることを確認し `aio app run` ます。
1. アセット計算開発ツールで、 [サンプル画像ファイルをアップロードまたは選択します](../assets/samples/sample-file.jpg)
   + 「 __Source file__ 」ドロップダウンでファイルが選択されていることを確認します。
1. 「 __アセット計算プロファイル定義__ 」テキスト領域を確認します。
   + キーは、デプロイ済みのアセット計算ワーカーのURLを定義します `worker`
   + キーは生成するレンディションの名前を定義し `name` ます
   + このJSONオブジェクトには他のキーや値を指定でき、オブジェクトの下のワーカーで使用でき `rendition.instructions` ます
      + オプションで、、 `size`およびの値を追加 `contrast` し `brightness`ます。

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

1. Tap the __Run__ button
1. 「 __レンディション」セクションに__ 、レンディションのプレースホルダーが設定されます
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
