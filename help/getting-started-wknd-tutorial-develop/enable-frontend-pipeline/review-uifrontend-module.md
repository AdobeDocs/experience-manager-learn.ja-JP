---
title: フルスタックプロジェクトの ui.frontend モジュールを確認する
description: Maven ベースのフルスタックAEM Sitesプロジェクトのフロントエンド開発、デプロイメント、配信のライフサイクルを確認します。
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 2e3615e9e9305165ca9c3c93b38ac7e9bdcc51fb
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 2%

---


# フルスタックAEMプロジェクトの「ui.frontend」モジュールを確認する {#aem-full-stack-ui-frontent}

この章では、フルスタックAEMプロジェクトのフロントエンドアーティファクトの開発、デプロイメント、および配信について、 __WKND Sites プロジェクト__.


## 目的 {#objective}

* AEMフルスタックプロジェクトでのフロントエンドアーティファクトのビルドおよびデプロイメントフローの理解
* AEMフルスタックプロジェクトの `ui.frontend` モジュール [webpack](https://webpack.js.org/) configs
* AEMクライアントライブラリ（clientlibs とも呼ばれます）の生成プロセス

## AEMフルスタックおよびクイックサイト作成プロジェクトのフロントエンドデプロイメントフロー

>[!IMPORTANT]
>
>このビデオでは、両方のフロントエンドフローについて説明し、説明します **フルスタックとクイックサイト作成** プロジェクトを使用して、フロントエンドリソースの構築、デプロイ、配信モデルにおける微妙な違いについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3409344/)

## 前提条件 {#prerequisites}


* のクローン [AEM WKND Sites プロジェクト](https://github.com/adobe/aem-guides-wknd)
* クローンされたAEM WKND Sites プロジェクトを作成し、AEM as a Cloud Serviceにデプロイしました。

AEM WKND Site プロジェクトを参照する [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) を参照してください。

## AEMフルスタックプロジェクトのフロントエンドアーティファクトフロー {#flow-of-frontend-artifacts}

以下に、 __開発、デプロイメント、配信__ フルスタックAEMプロジェクトでのフロントエンドアーティファクトのフロー。

![フロントエンドアーティファクトの開発、デプロイメント、配信](assets/Dev-Deploy-Delivery-AEM-Project.png)


開発フェーズでは、スタイル設定やリブランディングなどのフロントエンドの変更は、CSS、JS ファイルを `ui.frontend/src/main/webpack` フォルダー。 次に、ビルド時に、 [webpack](https://webpack.js.org/) module-bundler と maven プラグインは、これらのファイルを、以下で最適化されたAEM clientlib に変換します。 `ui.apps` モジュール。

フロントエンドの変更は、 [__フルスタック__ Cloud Manager のパイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html).

フロントエンドリソースは、 `/etc.clientlibs/`にキャッシュされ、通常はAEM Dispatcher および CDN でキャッシュされます。


>[!NOTE]
>
> 同様に、 __AEMクイックサイト作成ジャーニー__、 [フロントエンドの変更](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html) を実行することで、AEMas a Cloud Service環境にデプロイする __フロントエンド__ パイプラインについては、 [パイプラインの設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### WKND Sites プロジェクトでの WebPack 設定の確認 {#development-frontend-webpack-clientlib}

* 三つある __webpack__ WKND サイトのフロントエンドリソースのバンドルに使用される設定ファイル。

   1. `webpack.common`  — これには、 __共通__ WKND リソースのバンドル化と最適化を指示する設定 この __出力__ プロパティは、統合ファイル (JavaScript バンドルとも呼ばれますが、AEM OSGi バンドルと混同しないように ) を生成する場所を指定します。 デフォルトの名前はに設定されています。 `clientlib-site/js/[name].bundle.js`.

   ```javascript
       ...
       output: {
               filename: 'clientlib-site/js/[name].bundle.js',
               path: path.resolve(__dirname, 'dist')
           }
       ...    
   ```

   1. `webpack.dev.js` に __開発__ webpack-dev-serve の設定と、使用するHTMLテンプレートの指定。 また、で実行されているAEMインスタンスへのプロキシ設定も含まれます。 `localhost:4502`.

   ```javascript
       ...
       devServer: {
           proxy: [{
               context: ['/content', '/etc.clientlibs', '/libs'],
               target: 'http://localhost:4502',
           }],
       ...    
   ```

   1. `webpack.prod.js` に __実稼動__ 設定およびプラグインを使用して、開発ファイルを最適化されたバンドルに変換します。

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


* バンドルされたリソースは、 `ui.apps` 使用するモジュール [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) プラグイン、で管理される設定を使用 `clientlib.config.js` ファイル。

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

* この __frontend-maven-plugin__ から `ui.frontend/pom.xml` AEMプロジェクトのビルド中に webpack のバンドルと clientlib の生成を調整します。

`$ mvn clean install -PautoInstallSinglePackage`

### AEMへのデプロイメントas a Cloud Service {#deployment-frontend-aemaacs}

この [__フルスタック__ パイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline) ：これらの変更をAEM as a Cloud Service環境にデプロイします。


### AEMからの配信as a Cloud Service {#delivery-frontend-aemaacs}

フルスタックパイプラインを介してデプロイされたフロントエンドリソースは、AEM Site から Web ブラウザーに、 `/etc.clientlibs` ファイル。 これを確認するには、 [公開 WKND サイト](https://wknd.site/content/wknd/us/en.html) および Web ページのソースを表示する

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

次の章では、 [フロントエンドパイプラインを使用するようにプロジェクトを更新](update-project.md)を更新して、AEM WKND Sites Project をフロントエンドパイプライン契約に対して有効にします。
