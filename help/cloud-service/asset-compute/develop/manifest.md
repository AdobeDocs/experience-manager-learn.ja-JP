---
title: Asset Compute プロジェクトの manifest.yml の設定
description: Asset Compute プロジェクトの manifest.yml には、このプロジェクトでデプロイされるすべてのワーカーが記述されています。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
duration: 115
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '401'
ht-degree: 100%

---

# manifest.yml の設定

Asset Compute プロジェクトのルートにある `manifest.yml` には、このプロジェクトでデプロイされるすべてのワーカーが記述されています。

![manifest.yml](./assets/manifest/manifest.png)

## デフォルトのワーカー定義

ワーカーは、`actions` の下の Adobe I/O Runtimeアクションエントリとして定義され、一連の設定で構成されます。

他の Adobe I/O 統合にアクセスするワーカーは、`annotations -> require-adobe-auth` プロパティを `true` に設定する必要があります。これにより、`params.auth` オブジェクトを介して[ワーカーの Adobe I/O 資格情報が公開される](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html?lang=ja#access-adobe-apis)からです。 これは通常、ワーカーが Adobe Photoshop、Lightroom、Sensei API などの Adobe I/O API を呼び出すときに必要であり、ワーカーごとに切り替えることができます。

1. 自動生成されたワーカー `manifest.yml` を開いて確認します。複数の Asset Compute ワーカーを含むプロジェクトでは、`actions` 配列の下に各ワーカーのエントリを定義する必要があります。

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

各ワーカーは、Adobe I/O Runtime で実行コンテキストの[制限](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md)を設定できます。これらの値は、ワーカーが計算するアセットのボリューム、レート、タイプおよび実行するワークのタイプに基づいて、ワーカーに最適なサイズ設定を提供するように調整する必要があります。

制限を設定する前に、[アドビのサイズ設定ガイダンス](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html?lang=ja#sizing-workers)を確認してください。Asset Compute ワーカーは、アセットの処理中にメモリ不足になる可能性があり、結果として Adobe I/O Runtime の実行が強制終了する可能性があるので、すべての候補アセットを処理できるようにワーカーのサイズが適切に設定されます。

1. 新しい `wknd-asset-compute` アクションエントリに `inputs` セクションを追加します。これにより、Asset Compute ワーカーの全体的なパフォーマンスとリソース割り当てを調整できます。

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

## 完成した manifest.yml

最終的な `manifest.yml` は次のようになります。

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

## Github の manifest.yml

最終的な `.manifest.yml` は、次の Github で入手できます。

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## manifest.yml の検証

生成した Asset Compute `manifest.yml` が更新されたら、ローカルの開発ツールを実行し、更新された `manifest.yml` 設定で正常に起動することを確認します。

Asset Compute プロジェクトの Asset Compute 開発ツールを開始するには：

1. Asset Compute プロジェクトのルートでコマンドラインを開き（VS Code では、IDE で、ターミナル／新規ターミナルから直接開くことができます）、次のコマンドを実行します。

   ```
   $ aio app run
   ```

1. ローカルの Asset Compute 開発ツールが、デフォルトの web ブラウザー __http://localhost:9000__ で開きます。

   ![aio app run](assets/environment-variables/aio-app-run.png)

1. 開発ツール初期化時のエラーメッセージについては、コマンドライン出力と web ブラウザーで確認します。
1. Asset Compute 開発ツールを停止するには、`aio app run` を実行したウィンドウで `Ctrl-C` をタップしてプロセスを終了します。

## トラブルシューティング

+ [YAML インデントが正しくない](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize の上限の設定値が低すぎる](../troubleshooting.md#memorysize-limit-is-set-too-low)
