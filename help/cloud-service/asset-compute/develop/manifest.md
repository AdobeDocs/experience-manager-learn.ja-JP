---
title: アセット計算プロジェクトのmanifest.ymlの設定
description: Asset Computeプロジェクトのmanifest.ymlは、このプロジェクトの展開対象となるすべてのワーカーを説明します。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '550'
ht-degree: 0%

---


# manifest.ymlの設定

アセット計算プロジェクトのルートにある `manifest.yml`は、このプロジェクトのすべてのワーカーがデプロイされることを示します。

![manifest.yml](./assets/manifest/manifest.png)

## 既定の作業者定義

作業者は、の下でAdobe I/O Runtimeの活動の入口と定義され `actions`、一連の構成で構成されます。

他のAdobeI/O統合にアクセスするワーカーは、 `annotations -> require-adobe-auth` プロパティをに設定する必要があります。これにより、 `true` オブジェクトを介してワーカーのAdobeI/O秘密鍵証明書 [が](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis)`params.auth` 公開されます。 これは、通常、作業者がAdobe Photoshop、Lightroom、先生のAPIなどのAdobeI/O APIを呼び出す際に必要となり、作業者ごとに切り替えることができます。

1. 自動生成ワーカーを開き、確認し `manifest.yml`ます。 複数のアセット計算ワーカーを含むプロジェクトでは、 `actions` アレイの下の各ワーカーに対してエントリを定義する必要があります。

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

各ワーカーは、Adobe I/O Runtimeでの実行コンテキストの [制限を設定できます](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) 。 これらの値は、計算するアセットの量、率、タイプ、および実行する作業のタイプに基づいて、作業者に最適なサイズを提供するように調整する必要があります。

制限を設定する前に [、](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers) Adobeのサイズ設定に関するガイダンスを確認します。 アセット計算ワーカーは、アセットの処理時にメモリ不足になる場合があり、結果としてAdobe I/O Runtimeの実行が停止するので、ワーカーのサイズが適切に調整され、すべての候補アセットが処理されるようにします。

1. 新しいアク追加ションエントリのセクション。 `inputs``wknd-asset-compute` これにより、アセット計算ワーカーの全体的なパフォーマンスとリソース割り当てを調整できます。

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

最終的な結果は次のよう `manifest.yml` になります。

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

最終版 `.manifest.yml` は次の場所でGithubで入手できます。

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## manifest.ymlを検証しています

生成されたアセット計算が更新さ `manifest.yml` れたら、ローカルの開発ツールを実行し、開始が更新された `manifest.yml` 設定を正常に使用できることを確認します。

開始資産計算プロジェクトの資産計算開発ツールを作成するには：

1. 「アセット計算」プロジェクトルート（VSコード内）のコマンドラインを開き（IDEで端末/新しい端末を使用して直接開くことができます）、次のコマンドを実行します。

   ```
   $ aio app run
   ```

1. ローカルのAsset Compute Development ToolがデフォルトのWebブラウザー(http://localhost:9000)で開き __ます__。

   ![aioアプリの実行](assets/environment-variables/aio-app-run.png)

1. 開発ツールの初期化時に、コマンドライン出力とWebブラウザーでエラーメッセージが表示されないかを確認します。
1. アセット計算開発ツールを停止するには、実行したウィンドウ `Ctrl-C` でをタップし、プロセス `aio app run` を終了します。

## トラブルシューティング

### 誤ったYAMLインデント

+ __エラー：__ YAMLException:行X、列Y:(標準out from `aio app run` コマンドを使用)のマッピングエントリのインデントが正しくありません
+ __原因：__ Yamlファイルは空白が区別されます。インデントが正しくない可能性があります。
+ __解像度：__ を確認し `manifest.yml` 、すべてのインデントが正しいことを確認します。

### memorySize制限が低すぎます

+ __エラー：__ ローカルデベロッパーサーバーOpenWiskError:PUThttps://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=trueがHTTP 400を返しました（無効な要求） —> &quot;要求内容が正しくありません：要件が失敗しました：メモリ64 MBが許容しきい値134217728 Bインチを下回っています。
+ __原因：__ マニフェスト内の `memorySize` 制限が、エラーメッセージで報告される最小許容しきい値（バイト単位）を下回るように設定されました。
+ __解像度：__ の `memorySize` 制限を確認し、すべて許容され `manifest.yml` る最小しきい値より大きいことを確認します。