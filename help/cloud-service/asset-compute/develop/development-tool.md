---
title: Asset Compute 開発ツール
description: Asset Compute 開発ツールは、Adobe I/O Runtime の Asset Compute リソースに対して AEM SDK のコンテキスト外で開発者が Asset Computer ワーカーをローカルに設定および実行できるようにするローカル web ハーネスです。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
duration: 171
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 100%

---

# Asset Compute 開発ツール

Asset Compute 開発ツールは、Adobe I/O Runtime の Asset Compute リソースに対して AEM SDK のコンテキスト外で開発者が Asset Computer ワーカーをローカルに設定および実行できるようにするローカル web ハーネスです。

## Asset Compute 開発ツールの実行

Asset Compute 開発ツールは、ターミナルコマンドを使用して、Asset Compute プロジェクトのルートから実行できます。

```
$ aio app run
```

これにより、開発ツールが __http://localhost:9000__ で起動され、自動的にブラウザーウィンドウで開きます。開発ツールを実行するには、[有効な自動生成 devToolToken をクエリパラメーターで指定する必要があります](#troubleshooting__devtooltoken)。

## Asset Compute 開発ツールインターフェイスについて{#interface}

![Asset Compute 開発ツール](./assets/development-tool/asset-compute-dev-tool.png)

1. __ソースファイル__：ソースファイルは次の目的で選択します。
   + Asset Compute ワーカーに渡される `source` バイナリとして機能するアセットバイナリの選択
   + ソースファイルのアップロード
1. __Asset Compute プロファイル定義__：パラメーター（ワーカーの URL エンドポイント、結果のレンディション名、任意のパラメーターなど）を含め、実行する Asset Compute ワーカーを定義します。
1. __実行__：「実行」ボタンは、Asset Compute 設定プロファイルエディターで定義された Asset Compute プロファイルを実行します。
1. __中止__：「中止」ボタンは、「実行」ボタンをタップして開始された実行をキャンセルします。
1. __リクエスト／応答__：Adobe I/O Runtime で動作している Asset Compute ワーカーへの HTTP リクエストとその応答を提供します。これはデバッグに役立ちます。
1. __起動ログ__：Asset Compute ワーカーの実行を記述したログです。発生したエラーも含まれます。この情報は、`aio app run` の標準出力でも入手できます。
1. __レンディション__：Asset Compute ワーカーの実行で生成されたすべてのレンディションを表示します。
1. __devToolToken クエリパラメーター__：Asset Compute 開発ツールトークンには、有効な `devToolToken` クエリパラメーターが存在する必要があります。このトークンは、新しい開発ツールが起動されるたびに自動的に生成されます。

### カスタムワーカーの実行

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_開発ツールで Asset Compute ワークを実行したときのクリックスルー（音声なし）_

1. `aio app run` コマンドを使用して Asset Compute 開発ツールがプロジェクトルートから起動されていることを確認します。
1. Asset Compute 開発ツールで、[サンプル画像ファイル](../assets/samples/sample-file.jpg)をアップロードまたは選択します。
   + そのファイルは、必ず&#x200B;__ソースファイル__&#x200B;ドロップダウンで選択してください。
1. 「__Asset Compute プロファイル定義__」テキストエリアを確認します。
   + `worker` キーは、デプロイされた Asset Compute ワーカーへの URL を定義します。
   + `name` キーは、生成するレンディションの名前を定義します。
   + その他のキーと値もこの JSON オブジェクトで指定でき、`rendition.instructions` オブジェクトのワーカーで利用できます。
      + オプションで `size`、`contrast` および `brightness` の値を追加できます。

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
1. __「レンディション」セクション__&#x200B;でレンディションのプレースホルダーに入力します
1. ワーカーが完了すると、生成されたレンディションがレンディションのプレースホルダーに表示されます

開発ツールの実行中にワーカーコードにコードを変更すると、変更が「ホットデプロイ」されます。「ホットデプロイ」には数秒かかるので、開発ツールからワーカーを再実行する前に、デプロイを完了させます。

## トラブルシューティング

+ [YAML インデントが正しくない](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize の上限の設定値が低すぎる](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [private.key が見つからず、開発ツールを開始できない場合](../troubleshooting.md#missing-private-key)
+ [ソースファイルのドロップダウンが正しくない](../troubleshooting.md#source-files-dropdown-incorrect)
+ [devToolToken クエリパラメーターがないか無効](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [ソースファイルを削除できない](../troubleshooting.md#unable-to-remove-source-files)
+ [返されたレンディションが部分的に描画されたか破損している](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
