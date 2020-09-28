---
title: SPAの統合 | AEM SPAエディターとAngularの使用の手引き
description: Angularで記述された単一ページアプリケーション(SPA)のソースコードを、Adobe Experience Manager(AEM)プロジェクトと統合する方法を理解します。 AngularのCLIツールなど、最新のフロントエンドツールを使用して、AEM JSONモデルAPIに対するSPAの開発を迅速に行う方法を学びます。
sub-product: サイト
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5310
thumbnail: 5310-spa-angular.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '2202'
ht-degree: 2%

---


# SPAの統合 {#integrate-spa}

Angularで記述された単一ページアプリケーション(SPA)のソースコードを、Adobe Experience Manager(AEM)プロジェクトと統合する方法を理解します。 WebPack開発サーバーなど、最新のフロントエンドツールを使用して、AEM JSONモデルAPIに対してSPAを迅速に開発する方法を説明します。

## 目的

1. SPAプロジェクトがAEMとクライアント側ライブラリを統合する方法を理解します。
2. ローカル開発サーバーを専用のフロントエンド開発に使用する方法を説明します。
3. AEM JSONモデルAPIに対する開発に **プロキシ** および静的 **モック** ファイルの使用を調査します。

## 作成する内容

この章では、SPAに単純な `Header` コンポーネントを追加します。 この静的コンポーネントを構築する過程で、AEM SPA開発に対するいくつかのアプローチが使用されます。 `Header`

![AEMの新しいヘッダー](./assets/integrate-spa/final-header-component.png)

*SPAが拡張され、静的コンポー`Header`ネントが追加されます*

## 前提条件

Review the required tooling and instructions for setting up a [local development environment](overview.md#local-dev-environment).

### コードの取得

1. Gitを介して、このチュートリアルのスタートポイントをダウンロードします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/integrate-spa-start
   ```

2. Mavenを使用して、ローカルのAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   AEM 6.x [を使用している場合](overview.md#compatibility) 、 `classic` プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `Angular/integrate-spa-solution`ます。

## Integration approach {#integration-approach}

2つのモジュールがAEMプロジェクトの一部として作成されました。 `ui.apps` と `ui.frontend`。

この `ui.frontend` モジュールは、すべてのSPAソースコードを含む [webpack](https://webpack.js.org/) プロジェクトです。 SPAの開発およびテストの大部分は、WebPackプロジェクトで行われます。 実稼働版ビルドがトリガーされると、SPAはWebPackを使用して構築およびコンパイルされます。 コンパイルされたアーティファクト（CSSとJavaScript）が `ui.apps` モジュールにコピーされ、モジュールがAEMランタイムにデプロイされます。

![ui.frontendハイレベルアーキテクチャ](assets/integrate-spa/ui-frontend-architecture.png)

*SPA統合の高レベルな描写。*

フロントエンドビルドの詳細については、こちらを [参照してください](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)。

## InspectSPA統合 {#inspect-spa-integration}

次に、 `ui.frontend` モジュールを調べ、AEM [Projectアーキタイプによって自動生成されたSPAを確認します](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)。

1. 選択したIDEで、WKND SPA用のAEMプロジェクトを開きます。 このチュートリアルでは、 [Visual StudioコードIDEを使用します](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)。

   ![VSCode - AEM WKND SPAプロジェクト](./assets/integrate-spa/vscode-ide-openproject.png)

2. フォルダを展開し、 `ui.frontend` フォルダを調べます。 Open the file `ui.frontend/package.json`

3. の下に、次に関連す `dependencies` るいくつかの項目が表示され `@angular`ます。

   ```json
   "@angular/animations": "~9.1.11",
   "@angular/common": "~9.1.11",
   "@angular/compiler": "~9.1.11",
   "@angular/core": "~9.1.11",
   "@angular/forms": "~9.1.10",
   "@angular/platform-browser": "~9.1.10",
   "@angular/platform-browser-dynamic": "~9.1.10",
   "@angular/router": "~9.1.10",
   ```

   この `ui.frontend` モジュールは、ルーティングを含む [Angular CLIツールを使用して](https://angular.io) 生成されるAngularアプリケーション [](https://angular.io/cli) です。

4. また、次の3つの依存関係がプリフィックス付けされてい `@adobe`ます。

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   上記のモジュールは、 [AEM SPA Editor JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) 、およびSPAコンポーネントをAEMコンポーネントにマッピングできる機能を提供します。

5. ファイルには、次のように定義さ `package.json``scripts` れています。

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   これらのスクリプトは、一般的な [Angular CLIコマンドに基づいていますが](https://angular.io/cli/build) 、大規模なAEMプロジェクトで動作するように若干修正されています。

   `start` - AngularアプリをローカルWebサーバーを使用してローカルで実行します。 ローカルAEMインスタンスのコンテンツをプロキシするように更新されました。

   `build` - Angularアプリケーションをコンパイルして実稼働環境に配布します。 の追加 `&& clientlib` は、ビルド時に、コンパイル済みのSPAをクライアント側ライブラリとして `ui.apps` モジュールにコピーする役割を持ちます。 npmモジュール [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) を使用して、この作業を容易に行うことができます。

   使用可能なスクリプトの詳細については、 [こちらを参照してください](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)。

6. ファイルをInspect `ui.frontend/clientlib.config.js`。 この設定ファイルは、 [aem-clientlib-generatorによって](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) 、クライアントライブラリの生成方法を決定するために使用されます。

7. ファイルをInspect `ui.frontend/pom.xml`。 このファイルは、 `ui.frontend` フォルダーを [Mavenモジュールに変換します](http://maven.apache.org/guides/mini/guide-multiple-modules.html)。 この `pom.xml` ファイルは、Mavenの構築中にSPAの [テストと](https://github.com/eirslett/frontend-maven-plugin) 構築を行うために、フ **ロントエンドMavenプラグインを使用するように更新されました****** 。

8. Inspectのファイル `app.component.ts` の場所 `ui.frontend/src/app/app.component.ts`:

   ```js
   import { Constants } from '@adobe/cq-angular-editable-components';
   import { ModelManager } from '@adobe/cq-spa-page-model-manager';
   import { Component } from '@angular/core';
   
   @Component({
   selector: '#spa-root', // tslint:disable-line
   styleUrls: ['./app.component.css'],
   templateUrl: './app.component.html'
   })
   export class AppComponent {
       ...
   
       constructor() {
           ModelManager.initialize().then(this.updateData);
       }
   
       private updateData = pageModel => {
           this.path = pageModel[Constants.PATH_PROP];
           this.items = pageModel[Constants.ITEMS_PROP];
           this.itemsOrder = pageModel[Constants.ITEMS_ORDER_PROP];
       }
   }
   ```

   `app.component.js` は、SPAのエントリポイントです。 `ModelManager` は、AEM SPAエディターJS SDKによって提供されます。 アプリケーションに対して呼び出し、 `pageModel` （JSONコンテンツ）を挿入します。

## ヘッダー追加コンポーネント {#header-component}

次に、SPAに新しいコンポーネントを追加し、変更をローカルのAEMインスタンスにデプロイして統合を確認します。

1. Open a new terminal window and navigate to the `ui.frontend` folder:

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. Install [Angular CLI](https://angular.io/cli#installing-angular-cli) globall:Angularコンポーネントの生成、および **ng** コマンドを使用したAngularアプリケーションの構築と処理に使用します。

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > このプロジェクトで **使用される@angular/cli** のバージョンは **9.1.7です**。 Angular CLIのバージョンを同期させておくことを推奨します。

3. フォルダ内からAngular CLI `Header` コマンドを実行して、新しい `ng generate component` コンポーネントを作成し `ui.frontend` ます。

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   これにより、に新しいAngular Headerコンポーネントのスケルトンが作成され `ui.frontend/src/app/components/header`ます。

4. 選択したIDEで `aem-guides-wknd-spa` プロジェクトを開きます。 `ui.frontend/src/app/components/header` フォルダーに移動し、

   ![IDEのヘッダーコンポーネントのパス](assets/integrate-spa/header-component-path.png)

5. ファイルを開き、内容 `header.component.html` を次の内容に置き換えます。

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   これにより、静的なコンテンツが表示されるので、このAngularコンポーネントは、生成されたデフォルトの調整を必要としません `header.component.ts`。

6. にある **app.component.htmlファイルを開き**`ui.frontend/src/app/app.component.html`ます。 Add the `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   これには、すべてのページコンテンツの上に `header` コンポーネントが含まれます。

7. 新しいターミナルを開き、 `ui.frontend` フォルダーに移動して、次の `npm run build` コマンドを実行します。

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. `ui.apps` フォルダーに移動し、の下 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` に、コンパイル済みのSPAファイルがフォルダーからコピーされたことが確認できます`ui.frontend/build` 。

   ![ui.appsで生成されたクライアントライブラリ](assets/integrate-spa/compiled-spa-uiapps.png)

9. ターミナルに戻り、 `ui.apps` フォルダーに移動します。 次のMavenコマンドを実行します。

   ```shell
   $ cd ../ui.apps
   $ mvn clean install -PautoInstallPackage
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  9.629 s
   [INFO] Finished at: 2020-05-04T17:48:07-07:00
   [INFO] ------------------------------------------------------------------------
   ```

   これにより、AEMのローカルで実行されている `ui.apps` インスタンスにパッケージが展開されます。

10. ブラウザーのタブを開き、http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.htmlに移動し [ます](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。 これで、SPAにコンポー `Header` ネントの内容が表示されます。

   ![最初のヘッダー実装](assets/integrate-spa/initial-header-implementation.png)

   手順 **7 ～ 9** は、プロジェクトのルートからMavenビルドをトリガーするときに自動的に実行されます(例 `mvn clean install -PautoInstallSinglePackage`)。 これで、SPAとAEMクライアント側ライブラリの統合の基本を理解する必要があります。 AEMでは、コンポーネントの編集や追加は可能ですが、 `Text` コンポー `Header` ネントを編集することはできません。

## Webpack Dev Server - JSON APIのプロキシ {#proxy-json}

前の練習で見たように、クライアントライブラリを構築してAEMのローカルインスタンスに同期する場合は、数分かかります。 これは、最終テストでは許容されますが、SPA開発の大部分には適していません。

SPAの開発を迅速に行うには、 [webpack開発サーバ](https://webpack.js.org/configuration/dev-server/) を使用できます。 SPAは、AEMで生成されたJSONモデルによって駆動されます。 この演習では、実行中のAEMインスタンスのJSONコンテンツが **、** Angularプロジェクトによって設定された開発サーバーに [プロキシされます](https://angular.io/guide/build)。

1. IDEに戻り、 **proxy.conf.jsonファイルを開き**`ui.frontend/proxy.conf.json`ます。

   ```json
   [
       {
           "context": [
                       "/content/**/*.(jpg|jpeg|png|model.json)",
                       "/etc.clientlibs/**/*"
                   ],
           "target": "http://localhost:4502",
           "auth": "admin:admin",
           "logLevel": "debug"
       }
   ]
   ```

   Angularアプリ [は](https://angular.io/guide/build#proxying-to-a-backend-server) 、APIリクエストをプロキシする簡単なメカニズムを提供します。 で指定されたパターン `context` は、ローカルのAEM quickstart `localhost:4502`を介してプロキシされます。

2. 次の場所にあるファイル **index.html** を開き `ui.frontend/src/index.html`ます。 これは、開発サーバーで使用されるルートHTMLファイルです。

   のエントリがあることに注意してくだ `base href="/"`さい。 アプリで相対URLを解決するには [](https://angular.io/guide/deployment#the-base-tag) 、baseタグが重要です。

   ```html
   <base href="/">
   ```

3. Open a terminal window and navigate to the `ui.frontend` folder. Run the command `npm start`:

   ```shell
   $ cd ui.frontend
   $ npm start
   
   > wknd-spa-angular@0.1.0 start /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.conf.json
   
   10% building 3/3 modules 0 active[HPM] Proxy created: [ '/content/**/*.(jpg|jpeg|png|model.json)', '/etc.clientlibs/**/*' ]  ->  http://localhost:4502
   [HPM] Subscribed to http-proxy events:  [ 'error', 'close' ]
   ℹ ｢wds｣: Project is running at http://localhost:4200/webpack-dev-server/
   ℹ ｢wds｣: webpack output is served from /
   ℹ ｢wds｣: 404s will fallback to //index.html
   ```

4. 新しいブラウザータブを開き（まだ開いていない場合）、http://localhost:4200/content/wknd-spa-angular/us/en/home.htmlに移動し [ます](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)。

   ![Webpack Devサーバー — プロキシjson](assets/integrate-spa/webpack-dev-server-1.png)

   AEMと同じコンテンツが表示されますが、有効になっているオーサリング機能が1つもない場合は表示されます。

5. IDEに戻り、に示す新しいフォルダを作成 `img` し `ui.frontend/src/assets`ます。
6. 次のWKNDロゴをダウンロードし、 `img` フォルダに追加します。

   ![WKNDロゴ](./assets/integrate-spa/wknd-logo-dk.png)

7. 次の場所に **ある** header.component.htmlを開き `ui.frontend/src/app/components/header/header.component.html` 、ロゴを含めます。

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   変更を **header.component.htmlに保存します**。

8. ブラウザーに戻ります。 アプリに対する変更がすぐに反映されていることを確認します。

   ![ヘッダーに追加されたロゴ](assets/integrate-spa/added-logo-localhost.png)

   アドビがコンテンツをプロキシしているので、 **AEMでコンテンツを更新し続けて** 、 **Webpack Devサーバーに反映されているのを確認できます**。 コンテンツの変更は、 **webpackデベロッパーサーバーでのみ表示されます**。

9. ターミナルで、ローカルWebサーバー `ctrl+c` を停止します。

## Webpack Dev Server — モックJSON API {#mock-json}

迅速な開発のもう1つのアプローチは、静的なJSONファイルを使用してJSONモデルとして機能させることです。 JSONを「モッキング」することで、ローカルのAEMインスタンスへの依存関係を削除します。 また、フロントエンド開発者がJSONモデルを更新して機能をテストし、変更をJSON APIに導き、後でバックエンド開発者が実装することになります。

The initial set up of the mock JSON does **require a local AEM instance**.

1. ブラウザーで、http://localhost:4502/content/wknd-spa-angular/us/en.model.jsonに移動し [ます](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。

   これは、アプリケーションを実行している AEM から書き出した JSON です。JSON 出力をコピーします。

2. IDEに戻り、mocks `ui.frontend/src` and ******** jsonという名前の新しいフォルダに移動して追加し、次のフォルダ構造に一致するようにします。

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. 下に **en.model.jsonという名前の新しいファイルを作成し**`ui.frontend/public/mocks/json`ます。 手 **順1のJSON出力をここに貼り付けます** 。

   ![モックモデルJSONファイル](assets/integrate-spa/mock-model-json-created.png)

4. 下に新しい **proxy.mock.conf.json** ファイルを作成 `ui.frontend`します。 ファイルに以下のように入力します。

   ```json
   [
       {
       "context": [
           "/content/**/*.model.json"
       ],
       "pathRewrite": { "^/content/wknd-spa-angular/us" : "/mocks/json"} ,
       "target": "http://localhost:4200",
       "logLevel": "debug"
       }
   ]
   ```

   このプロキシ設定は、開始に対応する静的JSONファイル `/content/wknd-spa-angular/us` を `/mocks/json` 含むリクエストを書き換え、提供します。例：

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. Open the file **angular.json**. 新し追加い **開発設定です。作成したモック** フォルダーを参照する **assets****** 配列が更新されました。

   ```json
    "dev": {
             "assets": [
               "src/mocks",
               "src/assets",
               "src/favicon.ico",
               "src/logo192.png",
               "src/logo512.png",
               "src/manifest.json"
             ]
       },
   ```

   ![Angular JSON開発アセットの更新フォルダー](assets/integrate-spa/dev-assets-update-folder.png)

   専用の **開発** 設定を作成すると、開発時にのみMocks **** フォルダーが使用され、実稼働ビルドではAEMにはデプロイされません。

6. 次に、 **angular.json** ファイルで、 **browserTarget** 設定を更新し、新しい **開発** 設定を使用します。

   ```diff
     ...
     "serve": {
         "builder": "@angular-devkit/build-angular:dev-server",
         "options": {
   +       "browserTarget": "angular-app:build:dev"
   -       "browserTarget": "angular-app:build"
         },
     ...
   ```

   ![Angular JSONビルド開発の更新](assets/integrate-spa/angular-json-build-dev-update.png)

7. ファイルを開き、新しい `ui.frontend/package.json` 開始:mock **コマンドを追加して、** proxy.mock.conf.json **** ファイルを参照します。

   ```diff
       "scripts": {
           "start": "ng serve --open --proxy-config ./proxy.conf.json",
   +       "start:mock": "ng serve --open --proxy-config ./proxy.mock.conf.json",
           "build": "ng lint && ng build && clientlib",
           "build:production": "ng lint && ng build --prod && clientlib",
           "test": "ng test",
           "sync": "aemsync -d -w ../ui.apps/src/main/content"
       }
   ```

   新しいコマンドを追加すると、プロキシ設定を簡単に切り替えることができます。

8. 現在実行中の場合は、 **webpack Devサーバを停止します**。 **開始:mock** スクリプトを使用したWebPack開発サーバーの開始 **** :

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   http://localhost:4200/content/wknd-spa-angular/us/en/home.html [に移動すると](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) 、同じSPAが表示されますが、コンテンツが **モック** JSONファイルから取り込まれるようになりました。

9. 以前に作成した **en.model.json** ファイルに小さな変更を加えます。 更新されたコンテンツは、すぐに **webpack開発サーバに反映されます**。

   ![モデルのjsonの更新](./assets/integrate-spa/webpack-mock-model.gif)

   JSONモデルを操作し、実稼働中のSPAへの影響を確認できることは、開発者がJSONモデルAPIを理解するのに役立ちます。 また、フロントエンドとバックエンドの両方の開発を並行して行うこともできます。

## Sassで追加のスタイル

次に、更新された一部のスタイルがプロジェクトに追加されます。 このプロジェクトでは、変数などの便利な機能を [Sass](https://sass-lang.com/) でサポートするようになります。

1. ターミナルウィンドウを開き、 **webpack dev server** （起動している場合）を停止します。 フォルダー内から次のコマンドを入力し、Angularアプリケーションを更新して `ui.frontend` .scss **** ファイルを処理します。

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   これにより、 `angular.json` ファイルの下部に新しいエントリが追加され、ファイルが更新されます。

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. ブラウザー `normalize-scss` 全体でスタイルを正規化するには、をインストールします。

   ```shell
   $ npm install normalize-scss --save
   ```

3. IDEに戻り、次にという名前の新しいフォルダ `ui.frontend/src` を作成し `styles`ます。
4. 名前の下に新しいファイルを作成し、次の変数 `ui.frontend/src/styles``_variables.scss` を使用して入力します。

   ```scss
   //_variables.scss
   
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #FFFFFF;
   $yellow:                 #FFEA00;
   $blue:                   #0045FF;
   
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   $font-size-base:          18px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base));
   
   // Functional Colors
   $brand-primary:             $yellow;
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   $link-color:                $blue;
   
   //Layout
   $max-width: 1024px;
   $header-height: 75px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

5. styles.cssのファイル **styles.css** の拡張子 `ui.frontend/src/styles.css` の名前を **styles.scssに変更します**。 内容を次の内容に置き換えます。

   ```scss
   /* styles.scss * /
   
   /* Normalize */
   @import '~normalize-scss/sass/normalize';
   
   @import './styles/variables';
   
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   }
   
   body.page {
       max-width: $max-width;
       margin: 0 auto;
       padding: $gutter-padding;
       padding-top: $header-height;
   }
   ```

6. **angular.jsonを更新し** 、 **style.css** へのすべての参照に **styles.scssという名前を付け直します**。 参考文献は3つあります。

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## ヘッダースタイルを更新

次に、Sassを使用して、 **Header** コンポーネントにブランド固有のスタイルを追加します。

1. WebPack Dev **サーバーに開始し** 、スタイルの更新をリアルタイムで確認する：

   ```shell
   $ npm run start:mock
   ```

2. 「 `ui.frontend/src/app/components/header` re-name **header.component.css** 」の下の「 **header.component.scss**」に移動します。 ファイルに以下のように入力します。

   ```scss
   @import "~src/styles/variables";
   
   .header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .logo-img {
       width: 100px;
   }
   ```

3. header. **component.js** を更新して、 **header.component.scssを参照します**。

   ```diff
   ...
     @Component({
       selector: 'app-header',
       templateUrl: './header.component.html',
   -   styleUrls: ['./header.component.css']
   +   styleUrls: ['./header.component.scss']
     })
   ...
   ```

4. ブラウザに戻り、 **webpack devサーバ**:

   ![スタイル設定ヘッダー — webpack devサーバー](assets/integrate-spa/styled-header.png)

   これで、 **Header** コンポーネントに更新されたスタイルが追加されます。

## SPA更新をAEMに展開する

現在、 **ヘッダー** (Header **)に加えられた変更は、** Webpack開発サーバからのみ表示されます。 更新したSPAをAEMにデプロイして、変更を確認します。

1. WebPack Devサーバーを停止 **します**。
2. プロジェクトのルートに移動し、Mavenを使用してAEM `/aem-guides-wknd-spa` にプロジェクトをデプロイします。

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.htmlに移動し [ます](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。 ロゴとスタイルが適用された更新された **ヘッダ** ーが表示されます。

   ![AEMの更新されたヘッダー](assets/integrate-spa/final-header-component.png)

   更新されたSPAがAEMになったので、オーサリングを続行できます。

## バリデーターが{#congratulations}

SPAを更新し、AEMとの統合を確認しました。 現在は、 **webpack開発サーバーを使用してAEM JSONモデルAPIに対してSPAを開発する2つの異なる方法があります**。

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `Angular/integrate-spa-solution`ます。

### 次の手順 {#next-steps}

[SPAコンポーネントのAEMコンポーネントへのマッピング](map-components.md) - AEM SPA Editor JS SDKを使用して、AngularコンポーネントをAdobe Experience Manager(AEM)コンポーネントにマッピングする方法について説明します。 コンポーネントマッピングを使用すると、従来のAEMオーサリングと同様、AEM SPAエディタ内でSPAコンポーネントに対して動的な更新を行うことができます。
