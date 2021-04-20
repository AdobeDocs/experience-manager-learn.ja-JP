---
title: asset computeプロジェクトのmanifest.ymlの設定
description: asset computeプロジェクトのmanifest.ymlは、このプロジェクトの展開対象となるすべてのワーカーを説明します。
feature: asset computeマイクロサービス
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
topic: 統合、開発
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 2%

---


# manifest.ymlの設定

asset computeプロジェクトのルートにある`manifest.yml`は、このプロジェクトの展開対象となるすべてのワーカーを示します。

![manifest.yml](./assets/manifest/manifest.png)

## 既定の作業者定義

ワーカーは`actions`の下でAdobe I/O Runtimeのアクションエントリと定義され、一連の構成で構成されます。

他のAdobe I/O統合にアクセスするワーカーは、`annotations -> require-adobe-auth`プロパティを`true`に設定する必要があります。この[は、`params.auth`オブジェクトを介してワーカーのAdobe I/O資格情報](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis)を公開します。 これは通常、ワーカーがAdobe PhotoshopAPI、LightroomAPI、先生APIなどのAdobe I/OAPIを呼び出す際に必要となり、ワーカーごとに切り替えることができます。

1. を開き、自動生成ワーカー`manifest.yml`を確認します。 複数のAsset computeワーカーを含むプロジェクトでは、`actions`配列の下で各ワーカーのエントリを定義する必要があります。

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: # the array of workers, since we have a single worker there is only one entry beneath actions
      worker: # the auto-generated worker definition
        function: actions/worker/index.js # the entry point to the worker 
        web: 'yes'  # as our worker is invoked over HTTP from AEM Author service
        runtime: 'nodejs:12' # the target nodejs runtime (only 10 and 12 are supported)
        limits:
          concurrency: 10
        annotations:
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, Lightroom or Sensei APIs.
```

## 制限の定義

各ワーカーは、Adobe I/O Runtimeでの実行コンテキストに対して[制限](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md)を構成できます。 これらの値は、計算するアセットの量、率、タイプ、および実行する作業のタイプに基づいて、作業者に最適なサイズを提供するように調整する必要があります。

制限を設定する前に、[Adobeサイズガイダンス](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers)を確認してください。 asset computeワーカーは、アセットの処理時にメモリ不足になる場合があり、結果としてAdobe I/O Runtimeの実行が中止されるので、すべての候補アセットを処理できるように適切なサイズに設定します。

1. 新しい追加`wknd-asset-compute`アクションエントリの`inputs`セクション。 これにより、Asset computeワーカーの全体的なパフォーマンスとリソース割り当てを調整できます。

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits: # Allows for the tuning of the worker's performance
          timeout: 60000 # timeout in milliseconds (1 minute)
          memorySize: 512 # memory allocated in MB; if the worker offloads heavy computational work to other Web services this number can be reduced
          concurrency: 10 # adjust based on expected concurrent processing and timeout 
        annotations:
          require-adobe-auth: true
           
```

## 完成したmanifest.yml

最終的な`manifest.yml`は次のようになります。

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
```

## manifest.yml on Github

最終版`.manifest.yml`はGithubで次の場所で入手できます。

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## manifest.ymlを検証しています

生成されたAsset compute`manifest.yml`が更新されたら、ローカル開発ツールを実行し、更新された`manifest.yml`設定で開始が正常に実行されることを確認します。

asset computeプロジェクト用の開始Asset compute開発ツールを作成するには：

1. asset computeプロジェクトのルート（VSコード）でコマンドラインを開き（IDEで端末/新しい端末を介して直接開くことができます）、次のコマンドを実行します。

   ```
   $ aio app run
   ```

1. ローカルAsset compute開発ツールがデフォルトのWebブラウザー(__http://localhost:9000__)で開きます。

   ![aioアプリの実行](assets/environment-variables/aio-app-run.png)

1. 開発ツールの初期化時に、コマンドライン出力とWebブラウザーでエラーメッセージが表示されないかを確認します。
1. asset compute開発ツールを停止するには、`aio app run`を実行したウィンドウで`Ctrl-C`をタップし、プロセスを終了します。

## トラブルシューティング

+ [誤ったYAMLインデント](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize制限が低すぎます](../troubleshooting.md#memorysize-limit-is-set-too-low)
