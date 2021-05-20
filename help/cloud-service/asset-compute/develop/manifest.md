---
title: asset computeプロジェクトのmanifest.ymlの設定
description: asset computeプロジェクトのmanifest.ymlには、このプロジェクトでデプロイされるすべてのワーカーが記述されています。
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
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 2%

---


# manifest.ymlの設定

asset computeプロジェクトのルートにある`manifest.yml`は、このプロジェクトでデプロイされるすべてのワーカーを記述します。

![manifest.yml](./assets/manifest/manifest.png)

## 既定の作業者定義

ワーカーは、`actions`の下のAdobe I/O Runtimeアクションエントリとして定義され、一連の設定で構成されます。

他のAdobe I/O統合にアクセスするワーカーは、`annotations -> require-adobe-auth`プロパティを`true`に設定する必要があります。これは、[が`params.auth`オブジェクトを介してワーカーのAdobe I/O資格情報](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis)を公開するためです。 これは、通常、ワーカーがAdobe Photoshop、Lightroom、Sensei APIなどのAdobe I/OAPIを呼び出す際に必要で、ワーカーごとに切り替えることができます。

1. を開き、自動生成ワーカー`manifest.yml`を確認します。 複数のAsset computeワーカーを含むプロジェクトでは、`actions`配列の下に各ワーカーのエントリを定義する必要があります。

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

各ワーカーは、Adobe I/O Runtimeでの実行コンテキストの[制限](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md)を設定できます。 これらの値は、計算するアセットのボリューム、レート、タイプ、およびワークのタイプに基づいて、ワーカーに最適なサイズ設定を提供するように調整する必要があります。

制限を設定する前に、[Adobeのサイズ設定に関するガイダンス](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers)を確認してください。 asset computeワーカーは、アセットを処理する際にメモリ不足になる可能性があり、Adobe I/O Runtimeの実行が強制終了するので、ワーカーのサイズがすべての候補アセットを処理するように適切に設定されます。

1. 新しい`wknd-asset-compute`アクションエントリに`inputs`セクションを追加します。 これにより、Asset computeワーカーの全体的なパフォーマンスとリソース割り当てを調整できます。

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

## GitHubのmanifest.yml

最終的な`.manifest.yml`は、GitHubで次の場所から入手できます。

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## manifest.ymlの検証

生成されたAsset compute`manifest.yml`が更新されたら、ローカル開発ツールを実行し、更新された`manifest.yml`設定で正常に起動するようにします。

asset computeプロジェクトのAsset compute開発ツールを開始するには：

1. asset computeプロジェクトのルート（VS Codeで、ターミナル/新しいターミナルからIDEで直接開くことができます）のコマンドラインを開き、次のコマンドを実行します。

   ```
   $ aio app run
   ```

1. ローカルAsset compute開発ツールが、デフォルトのWebブラウザー(__http://localhost:9000__)で開きます。

   ![aioアプリ実行](assets/environment-variables/aio-app-run.png)

1. 開発ツールの初期化時に、コマンドライン出力とWebブラウザでエラーメッセージを確認します。
1. asset compute開発ツールを停止するには、`aio app run`を実行したウィンドウで`Ctrl-C`をタップし、プロセスを終了します。

## トラブルシューティング

+ [YAMLインデントが正しくありません](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize制限が小さすぎます](../troubleshooting.md#memorysize-limit-is-set-too-low)
