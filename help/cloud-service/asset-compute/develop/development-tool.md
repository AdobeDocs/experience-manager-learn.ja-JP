---
title: asset compute開発ツール
description: asset compute開発ツールは、Adobe I/O RuntimeのAsset computeリソースに対するAEM SDK のコンテキスト外で、開発者が Asset Computer ワーカーをローカルで設定および実行できる、ローカル Web ハーネスです。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '535'
ht-degree: 0%

---

# asset compute開発ツール

asset compute開発ツールは、Adobe I/O RuntimeのAsset computeリソースに対するAEM SDK のコンテキスト外で、開発者が Asset Computer ワーカーをローカルで設定および実行できる、ローカル Web ハーネスです。

## asset compute開発ツールの実行

asset compute開発ツールは、ターミナルコマンドを使用して、Asset computeプロジェクトのルートから実行できます。

```
$ aio app run
```

これにより、次の場所で開発ツールが起動します： __http://localhost:9000__&#x200B;ブラウザーウィンドウで自動的に開きます。 開発ツールを実行するには、次の手順に従います。 [有効な自動生成 devToolToken をクエリパラメーターで指定する必要があります](#troubleshooting__devtooltoken).

## asset compute開発ツールインターフェイスについて{#interface}

![asset compute開発ツール](./assets/development-tool/asset-compute-dev-tool.png)

1. __ソースファイル：__ ソースファイルの選択は、次の目的で使用されます。
   + として機能するアセットバイナリを選択しました `source` バイナリがAsset computeワーカーに渡されました
   + ソースファイルのアップロード
1. __asset computeプロファイルの定義：__ 実行するAsset computeワーカーを定義します。パラメーターは次のとおりです。ワーカーの URL エンドポイント、結果のレンディション名、および任意のパラメーターを含めます。
1. __実行：__ 「実行」ボタンは、設定プロファイルエディターで定義したAsset computeプロファイルをAsset computeします
1. __中止：__ 「中止」ボタンをクリックすると、「実行」ボタンをタップして開始された実行がキャンセルされます
1. __リクエスト/レスポンス：__ Adobe I/O Runtimeで動作しているAsset computeワーカーに対する HTTP リクエストと応答を提供します。 これはデバッグに役立つ場合があります
1. __アクティベーションログ：__ エラーと共にAsset computeワーカーの実行を説明するログ。 この情報は、 `aio app run` 標準的に
1. __レンディション：__ asset compute・ワーカーの実行によって生成されたすべてのレンディションを表示
1. __devToolToken クエリパラメーター：__ asset compute開発ツールトークンには有効な `devToolToken` クエリーパラメーターが存在する必要があります。 このトークンは、新しい開発ツールが生成されるたびに自動的に生成されます

### カスタムワーカーの実行

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_開発ツールでのAsset computeの実行のクリックスルー（音声なし）_

1. asset compute開発ツールが、 `aio app run` コマンドを使用します。
1. asset compute開発ツールで、 [サンプル画像ファイル](../assets/samples/sample-file.jpg)
   + ファイルが __ソースファイル__ ドロップダウン
1. 以下を確認します。 __asset computeプロファイル定義__ テキスト領域
   + この `worker` キーは、デプロイ済みのAsset computeワーカーへの URL を定義します
   + この `name` キー：生成するレンディションの名前を定義します。
   + その他のキー/値は、この JSON オブジェクトに指定でき、 `rendition.instructions` object
      + オプションで次の値を追加します。 `size`, `contrast` および `brightness`:

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

1. 次をタップします。 __実行__ ボタン
1. この __レンディションセクション__ レンディションプレースホルダーで入力されます
1. ワーカーが完了すると、レンディションプレースホルダーに生成されたレンディションが表示されます

開発ツールの実行中にワーカーコードにコードを変更すると、変更が「ホットデプロイ」されます。 「ホットデプロイ」には数秒かかるので、開発ツールからワーカーを再実行する前に、デプロイを完了させます。

## トラブルシューティング

+ [YAML インデントが正しくありません](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize の上限が低すぎます](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [private.key が見つからないため、開発ツールを開始できません](../troubleshooting.md#missing-private-key)
+ [ソースファイルのドロップダウンが正しくありません](../troubleshooting.md#source-files-dropdown-incorrect)
+ [devToolToken クエリパラメーターがないか、無効です](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [ソースファイルを削除できません](../troubleshooting.md#unable-to-remove-source-files)
+ [レンディションが部分的に描画されたか破損しています](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
