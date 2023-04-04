---
title: フルスタックAEMプロジェクトを更新して、フロントエンドパイプラインを使用する
description: フルスタックAEMプロジェクトを更新して、フロントエンドパイプラインで有効にする方法を説明します。これにより、フロントエンドアーティファクトの構築とデプロイのみがおこなわれます。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---


# フルスタックAEMプロジェクトを更新して、フロントエンドパイプラインを使用する {#update-project-enable-frontend-pipeline}

この章では、 __WKND Sites プロジェクト__ を使用すると、完全なフルスタックパイプライン実行が必要なく、フロントエンドパイプラインを使用して JavaScript と CSS をデプロイできます。 これにより、フロントエンドアーティファクトとバックエンドアーティファクトの開発とデプロイメントのライフサイクルが切り離され、より迅速で反復的な開発プロセス全体を実現します。

## 目的 {#objectives}

* フルスタックプロジェクトを更新してフロントエンドパイプラインを使用する

## フルスタックAEMプロジェクトの設定変更の概要

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## 前提条件 {#prerequisites}

これは複数のパートから成るチュートリアルで、 [「ui.frontend」モジュール](./review-uifrontend-module.md).


## フルスタックAEMプロジェクトの変更

テスト実行には、プロジェクト関連の設定の変更とスタイルの変更が 3 つあります。そのため、WKND プロジェクトでは、フロントエンドパイプライン契約に対して設定を有効にするために、合計 4 つの具体的な変更がおこなわれます。

1. を削除します。 `ui.frontend` フルスタックビルドサイクルのモジュール

   * では、 WKND Sites プロジェクトのルートです。 `pom.xml` コメントを入力 `<module>ui.frontend</module>` サブモジュールエントリ。

   ```xml
       ...
       <modules>
       <module>all</module>
       <module>core</module>
       <!--
       <module>ui.frontend</module>
       -->                
       <module>ui.apps</module>
       ...
   ```

   * また、 `ui.apps/pom.xml`

   ```xml
       ...
       <!-- ====================================================================== -->
       <!-- D E P E N D E N C I E S                                                -->
       <!-- ====================================================================== -->
           ...
       <!--
           <dependency>
               <groupId>com.adobe.aem.guides</groupId>
               <artifactId>aem-guides-wknd.ui.frontend</artifactId>
               <version>${project.version}</version>
               <type>zip</type>
           </dependency>
       -->    
       ...
   ```

1. を準備する `ui.frontend` 2 つの新しい webpack 設定ファイルを追加することで、フロントエンドパイプライン契約のモジュール。

   * 既存の `webpack.common.js` as `webpack.theme.common.js`、および変更 `output` プロパティと `MiniCssExtractPlugin`, `CopyWebpackPlugin` プラグイン設定パラメーターを次に示します。

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'theme/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * 既存の `webpack.prod.js` as `webpack.theme.prod.js`、および `common` 変数の上記のファイルへの場所を、

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >上記の 2 つの「webpack」設定の変更は、異なる出力ファイル名とフォルダー名を持つようになったので、clientlib（フルスタック）と、生成された（フロントエンド）パイプラインフロントエンドアーティファクトを簡単に区別できます。
   >
   >上記の変更は、既存の webpack 設定を使用するためにスキップすることもできますが、以下の変更が必要です。
   >
   >名前の設定や整理の方法は、ユーザー次第です。


   * 内 `package.json` ファイル、確かに  `name` プロパティの値が `/conf` ノード。 また、 `scripts` プロパティ `build` このモジュールからフロントエンドファイルを構築する方法を説明するスクリプト

   ```javascript
       {
       "name": "wknd",
       "version": "1.0.0",
       ...
   
       "scripts": {
           "build": "webpack --config ./webpack.theme.prod.js"
       }
   
       ...
       }
   ```

1. を準備する `ui.content` 2 つの Sling 設定を追加することで、フロントエンドパイプラインのモジュール。

   * にファイルを作成します。 `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig`  — これには、 `ui.frontend` モジュールは、 `dist` webpack のビルドプロセスを使用するフォルダ。

   ```xml
   ...
       <css
       jcr:primaryType="nt:unstructured"
       element="link"
       location="header">
       <attributes
           jcr:primaryType="nt:unstructured">
           <as
               jcr:primaryType="nt:unstructured"
               name="as"
               value="style"/>
           <href
               jcr:primaryType="nt:unstructured"
               name="href"
               value="/theme/site.css"/>
   ...
   ```

   >[!TIP]
   >
   >    詳しくは、 [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) 内 __AEM WKND Sites プロジェクト__.


   * 2 番目の `com.adobe.aem.wcm.site.manager.config.SiteConfig` と `themePackageName` 値が `package.json` および `name` プロパティの値と `siteTemplatePath` を指すさま `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` スタブパスの値。

   ```xml
   ...
       <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
               jcr:primaryType="nt:unstructured"
               siteTemplatePath="/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0"
               themePackageName="wknd">
       </jcr:root>
   ...
   ```

   >[!TIP]
   >
   >    詳しくは、 [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) 内 __AEM WKND Sites プロジェクト__.

1. テスト実行のフロントエンドパイプラインを介してデプロイするために、テーマまたはスタイルが変更されます。 `text-color` を赤にAdobe（または独自のを選択）するには、 `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

最後に、これらの変更をプログラムのAdobegit リポジトリにプッシュします。


>[!AVAILABILITY]
>
> これらの変更は、GitHub の [__フロントエンドパイプライン__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) 分岐 __AEM WKND Sites プロジェクト__.


## 注意 — _フロントエンドパイプラインの有効化_ ボタン

この [パネルセレクター](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) &#39;s [サイト](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) オプションには **フロントエンドパイプラインの有効化** ボタンをクリックして、サイトのルートまたはサイトページを選択します。 クリック **フロントエンドパイプラインの有効化** ボタンは上記の **Sling 設定**、必ず **クリックしない** このボタンをクリックします。

![「フロントエンドパイプラインを有効にする」ボタン](assets/enable-front-end-Pipeline-button.png)

誤ってクリックされた場合は、パイプラインを再実行して、フロントエンドパイプラインが契約し、変更が復元されていることを確認する必要があります。

## おめでとうございます。 {#congratulations}

これで、WKND Sites プロジェクトが、フロントエンドパイプライン契約に対して有効になるように更新されました。

## 次の手順 {#next-steps}

次の章では、 [フロントエンドパイプラインを使用したデプロイ](create-frontend-pipeline.md)で、フロントエンドパイプラインを作成して実行し、 __移動して__ 「/etc.clientlibs」ベースのフロントエンドリソース配信から。
