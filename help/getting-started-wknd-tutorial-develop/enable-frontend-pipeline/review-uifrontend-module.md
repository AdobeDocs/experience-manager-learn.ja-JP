---
title: フルスタックプロジェクトの ui.frontend モジュールを確認する
description: Maven ベースのフルスタック AEM Sites プロジェクトのフロントエンド開発、デプロイメント、配信のライフサイクルを確認します。
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 65e8d41e-002a-4d80-a050-5366e9ebbdea
duration: 364
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 100%

---

# フルスタック AEM プロジェクトの「ui.frontend」モジュールを確認する {#aem-full-stack-ui-frontent}

この章では、__WKND サイトプロジェクト__&#x200B;の「ui.frontend」モジュールに焦点を当てて、フルスタック AEM プロジェクトのフロントエンドアーティファクトの開発、デプロイおよび配信について確認します。


## 目的 {#objective}

* AEM フルスタックプロジェクトでのフロントエンドアーティファクトのビルドおよびデプロイメントフローについて
* AEM フルスタックプロジェクトの `ui.frontend` モジュールの [webpack](https://webpack.js.org/) 設定
* AEM クライアントライブラリ（clientlibs とも呼ばれる）の生成プロセス

## AEM フルスタックおよびクイックサイト作成プロジェクトのフロントエンドデプロイメントフロー

>[!IMPORTANT]
>
>このビデオでは、**フルスタックプロジェクトとクイックサイト作成**&#x200B;プロジェクトの両方のフロントエンドフローを説明および実演し、フロントエンドリソースのビルド、デプロイ、配信モデルの微妙な違いを概説します。

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## 前提条件 {#prerequisites}


* [AEM WKND Sites プロジェクト](https://github.com/adobe/aem-guides-wknd)のクローン
* AEM WKND Sites プロジェクトのクローンを AEM as a Cloud Service にビルドしてデプロイしてあること。

詳しくは、AEM WKND Site プロジェクトの [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) を参照してください。

## AEM フルスタックプロジェクトのフロントエンドアーティファクトフロー {#flow-of-frontend-artifacts}

以下は、フルスタック AEM プロジェクトにおけるフロントエンドアーティファクトの&#x200B;__開発、デプロイ、配信__&#x200B;フローの概要です。

![フロントエンドアーティファクトの開発、デプロイメント、配信](assets/Dev-Deploy-Delivery-AEM-Project.png)


開発段階では、`ui.frontend/src/main/webpack` フォルダーの CSS ファイルと JS ファイルを更新することで、スタイリングやブランド変更などのフロントエンドの変更が行われます。次に、ビルド時に、[webpack](https://webpack.js.org/) module-bundler と maven プラグインは、これらのファイルを `ui.apps` モジュールの下で最適化された AEM clientlibs に変換します。

Cloud Manager で&#x200B;[__フルスタック__&#x200B;パイプラインを実行すると、フロントエンドの変更が AEM as a Cloud Service 環境にデプロイされます](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=ja)。

フロントエンドのリソースは、`/etc.clientlibs/` で始まる URI パスを介して web ブラウザーに配信され、通常は AEM Dispatcher および CDN にキャッシュされます。


>[!NOTE]
>
> 同様に、__AEM クイックサイト作成ジャーニー__&#x200B;では、[フロントエンドの変更](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html?lang=ja)は&#x200B;__フロントエンド__&#x200B;パイプラインを実行することで、AEM as a Cloud Service 環境にデプロイされます。[パイプラインの設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html?lang=ja)を参照してください

### WKND Sites プロジェクトでの WebPack 設定の確認 {#development-frontend-webpack-clientlib}

* WKND Sites のフロントエンドリソースをバンドルするために使用される __webpack__ 設定ファイルは 3 つあります。

   1. `webpack.common` - これには、WKND リソースのバンドルと最適化を指示する&#x200B;__共通__&#x200B;設定が含まれます。__output__ プロパティは、作成する統合ファイル（JavaScript バンドルとも呼ばれますが、AEM OSGi バンドルと混同しないように）を出力する場所を指定します。デフォルトの名前は `clientlib-site/js/[name].bundle.js` に設定されています。

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js` には、webpack-dev-serve の&#x200B;__開発__&#x200B;設定が含まれており、使用する HTML テンプレートを指しています。また、`localhost:4502` で実行されている AEM インスタンスへのプロキシ設定も含まれます。

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js` には&#x200B;__実稼働__&#x200B;設定が含まれており、プラグインを使用して開発ファイルを最適化されたバンドルに変換します。

  ```javascript
      ...
      module.exports = merge(common, {
          mode: 'production',
          optimization: {
              minimize: true,
              minimizer: [
                  new TerserPlugin(),
                  new CssMinimizerPlugin({ ...})
          }
      ...    
  ```


* バンドルされたリソースは、[aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) プラグインを使用して、`clientlib.config.js` ファイルで管理されている設定を使用して `ui.apps` モジュールに移動されます。

```javascript
    ...
    const BUILD_DIR = path.join(__dirname, 'dist');
    const CLIENTLIB_DIR = path.join(
    __dirname,
    '..',
    'ui.apps',
    'src',
    'main',
    'content',
    'jcr_root',
    'apps',
    'wknd',
    'clientlibs'
    );
    ...
```

* `ui.frontend/pom.xml` の __frontend-maven-plugin__ は、AEM プロジェクトのビルド中に webpack のバンドルと clientlib の生成を調整します。

`$ mvn clean install -PautoInstallSinglePackage`

### AEM as a Cloud Service へのデプロイメント {#deployment-frontend-aemaacs}

[__フルスタック__&#x200B;パイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=ja#full-stack-pipeline)は、これらの変更を AEM as a Cloud Service 環境にデプロイします


### AEM as a Cloud Service からの配信 {#delivery-frontend-aemaacs}

フルスタックパイプラインを介してデプロイされたフロントエンドリソースは、AEM サイトから web ブラウザーに `/etc.clientlibs` ファイルとして配信されます。これは、[公開されている WKND サイト](https://wknd.site/content/wknd/us/en.html)にアクセスして、web ページのソースを表示することで確認できます。

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## おめでとうございます。 {#congratulations}

これで、フルスタックプロジェクトの ui.frontend モジュールを確認しました

## 次の手順 {#next-steps}

次の章では、[フロントエンドパイプラインを使用するようにプロジェクトを更新](update-project.md)して、AEM WKND サイトプロジェクトをフロントエンドパイプライン契約に対して有効にします。
