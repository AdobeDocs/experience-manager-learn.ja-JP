---
title: SPAの統合 | AEM SPAエディタとAngularの使い始めに
description: angularで記述された単一ページアプリ(SPA)のソースコードを、Adobe Experience Manager(AEM)プロジェクトと統合する方法を理解します。 angularのCLIツールなど、最新のフロントエンドツールを使用して、AEM JSONモデルAPIに対するSPAを迅速に開発する方法を学びます。
sub-product: サイト
feature: SPAエディタ
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
source-wordcount: '2204'
ht-degree: 3%

---


# SPAを統合する{#integrate-spa}

angularで記述された単一ページアプリ(SPA)のソースコードを、Adobe Experience Manager(AEM)プロジェクトと統合する方法を理解します。 AEM JSONモデルAPIに対してSPAを迅速に開発するために、webpack開発サーバーなどの最新のフロントエンドツールを使用する方法を説明します。

## 目的

1. SPAプロジェクトがAEMとクライアント側ライブラリを統合する方法を理解します。
2. ローカル開発サーバーを専用のフロントエンド開発に使用する方法を説明します。
3. AEM JSONモデルAPIに対する開発に&#x200B;**プロキシ**&#x200B;と静的&#x200B;**モック**&#x200B;ファイルの使用を調べます。

## 作成する内容

この章では、SPAに単純な`Header`コンポーネントを追加します。 この静的な`Header`コンポーネントを構築する過程で、AEM SPA開発に対するいくつかのアプローチが使用されます。

![AEMの新しいヘッダー](./assets/integrate-spa/final-header-component.png)

*SPAが拡張され、静的 `Header` コンポーネントが追加されます*

## 前提条件

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。

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

   [AEM 6.x](overview.md#compatibility)を使用している場合は、`classic`プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution)に常に表示できます。また、ブランチ`Angular/integrate-spa-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。

## 統合アプローチ{#integration-approach}

2つのモジュールがAEMプロジェクトの一部として作成されました。`ui.apps`と`ui.frontend`。

`ui.frontend`モジュールは、SPAソースコードのすべてを含む[webpack](https://webpack.js.org/)プロジェクトです。 SPAの開発およびテストの大部分は、WebPackプロジェクトで行われます。 実稼働版ビルドがトリガーされると、SPAはWebPackを使用して構築およびコンパイルされます。 コンパイルされたアーティファクト（CSSとJavaScript）が`ui.apps`モジュールにコピーされ、AEMランタイムにデプロイされます。

![ui.frontendハイレベルアーキテクチャ](assets/integrate-spa/ui-frontend-architecture.png)

*SPA統合の高レベルな描写。*

フロントエンドビルドに関する追加情報は、[](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)を参照してください。

## InspectとSPAの統合{#inspect-spa-integration}

次に、`ui.frontend`モジュールを調べて、[AEMプロジェクトのアーキタイプ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)によって自動生成されたSPAを確認します。

1. 選択したIDEで、WKND SPA用のAEMプロジェクトを開きます。 このチュートリアルでは、[Visual StudioコードIDE](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)を使用します。

   ![VSCode - AEM WKND SPAプロジェクト](./assets/integrate-spa/vscode-ide-openproject.png)

2. `ui.frontend`フォルダーを展開し、調査します。 ファイル`ui.frontend/package.json`を開きます

3. `dependencies`の下に、`@angular`に関連するいくつかの要素が表示されます。

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

   `ui.frontend`モジュールは、ルーティングを含む[AngularCLIツール](https://angular.io/cli)を使用して生成された[Angularアプリケーション](https://angular.io)です。

4. また、`@adobe`というプリフィックスが付いた3つの依存関係もあります。

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   上記のモジュールは、[AEM SPA Editor JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html)を構成し、SPAコンポーネントをAEMコンポーネントにマッピングする機能を提供します。

5. `package.json`ファイルには、次のように複数の`scripts`が定義されています。

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   これらのスクリプトは、一般的な[AngularCLIコマンド](https://angular.io/cli/build)に基づいていますが、大規模なAEMプロジェクトで動作するように若干変更されています。

   `start` -AngularアプリをローカルWebサーバーを使用してローカルで実行します。ローカルAEMインスタンスのコンテンツをプロキシするように更新されました。

   `build` -Angular版アプリをコンパイルして実稼働版を配布します。`&& clientlib`を追加すると、ビルド中にコンパイルされたSPAをクライアント側ライブラリとして`ui.apps`モジュールにコピーします。 npmモジュール[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)を使用して、この処理を容易にします。

   使用可能なスクリプトの詳細は、[](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)を参照してください。

6. `ui.frontend/clientlib.config.js` ファイルを検査します。この設定ファイルは、[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs)でクライアントライブラリの生成方法を決定するために使用されます。

7. `ui.frontend/pom.xml` ファイルを検査します。このファイルは、`ui.frontend`フォルダーを[Mavenモジュール](http://maven.apache.org/guides/mini/guide-multiple-modules.html)に変換します。 `pom.xml`ファイルは、Mavenのビルド中に、[フロントエンド —maven-plugin](https://github.com/eirslett/frontend-maven-plugin)を&#x200B;**test**&#x200B;に、**build**&#x200B;に使用するように更新されました。

8. ファイル`app.component.ts`を`ui.frontend/src/app/app.component.ts`にInspect:

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

   `app.component.js` はSPAのエントリポイントです。`ModelManager` は、AEM SPAエディターJS SDKで提供されます。`pageModel`（JSONコンテンツ）を呼び出し、アプリケーションに挿入します。

## 追加ヘッダーコンポーネント{#header-component}

次に、SPAに新しいコンポーネントを追加し、変更をローカルAEMインスタンスにデプロイして統合を確認します。

1. 新しいターミナルウィンドウを開き、`ui.frontend` フォルダーに移動します。

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. [AngularCLI](https://angular.io/cli#installing-angular-cli)をグローバルにインストールします。これは、Angularコンポーネントの生成、および&#x200B;**ng**&#x200B;コマンドを使用したAngularアプリケーションの構築と提供に使用します。

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > このプロジェクトで使用される&#x200B;**@angular/cli**&#x200B;のバージョンは&#x200B;**9.1.7**&#x200B;です。 angularのCLIのバージョンを同期させておくことをお勧めします。

3. 新しい`Header`コンポーネントを作成するには、`ui.frontend`フォルダ内からAngularCLI `ng generate component`コマンドを実行します。

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   これにより、`ui.frontend/src/app/components/header`に新しいAngularヘッダコンポーネント用のスケルトンが作成されます。

4. 任意のIDEで`aem-guides-wknd-spa`プロジェクトを開きます。 `ui.frontend/src/app/components/header` フォルダーに移動し、

   ![IDEのヘッダーコンポーネントのパス](assets/integrate-spa/header-component-path.png)

5. ファイル`header.component.html`を開き、内容を次の内容に置き換えます。

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   これは静的コンテンツを表示するので、このAngularコンポーネントでは、デフォルトで生成された`header.component.ts`を調整する必要はありません。

6. `ui.frontend/src/app/app.component.html`にある&#x200B;**app.component.html**&#x200B;を開きます。 追加`app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   これには、すべてのページコンテンツの上に`header`コンポーネントが含まれます。

7. 新しいターミナルを開き、`ui.frontend`フォルダーに移動して`npm run build`コマンドを実行します。

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. `ui.apps` フォルダーに移動し、`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular`の下に、コンパイル済みのSPAファイルが`ui.frontend/build`フォルダーからコピーされているのが見えます。

   ![ui.appsで生成されたクライアントライブラリ](assets/integrate-spa/compiled-spa-uiapps.png)

9. ターミナルに戻り、`ui.apps`フォルダーに移動します。 次のMavenコマンドを実行します。

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

   これにより、AEMのローカルで実行されているインスタンスに`ui.apps`パッケージが展開されます。

10. ブラウザーのタブを開き、[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)に移動します。 これで、SPAに`Header`コンポーネントの内容が表示されます。

   ![最初のヘッダー実装](assets/integrate-spa/initial-header-implementation.png)

   手順&#x200B;**7-9**&#x200B;は、プロジェクトのルートからMavenビルドをトリガーするときに自動的に実行されます(`mvn clean install -PautoInstallSinglePackage`)。 これで、SPAとAEMのクライアント側ライブラリの統合の基本を理解する必要があります。 AEMでは、`Text`コンポーネントの編集と追加は可能ですが、`Header`コンポーネントは編集できません。

## Webpack Dev Server - JSON APIのプロキシ{#proxy-json}

前の練習で見たように、クライアントライブラリを構築してAEMのローカルインスタンスに同期する場合は、数分かかります。 これは、最終テストでは許容されますが、SPA開発の大部分には適していません。

[webpack dev server](https://webpack.js.org/configuration/dev-server/)は、SPAを迅速に開発するのに使用できます。 SPAは、AEMで生成されたJSONモデルによって駆動されます。 この演習では、AEMの実行中のインスタンスのJSONコンテンツを&#x200B;**Angularプロジェクト](https://angular.io/guide/build)によって設定された開発サーバーに[プロキシ**&#x200B;します。

1. IDEに戻り、**proxy.conf.json** (`ui.frontend/proxy.conf.json`)ファイルを開きます。

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

   [Angularアプリ](https://angular.io/guide/build#proxying-to-a-backend-server)は、APIリクエストをプロキシする簡単なメカニズムを提供します。 `context`で指定されたパターンは、ローカルのAEM quickstartである`localhost:4502`を通してプロキシされます。

2. `ui.frontend/src/index.html`にある&#x200B;**index.html**&#x200B;ファイルを開きます。 これは、開発サーバーで使用されるルートHTMLファイルです。

   `base href="/"`にはエントリがあることに注意してください。 [baseタグ](https://angular.io/guide/deployment#the-base-tag)は、アプリが相対URLを解決するのに重要です。

   ```html
   <base href="/">
   ```

3. ターミナルウィンドウを開き、`ui.frontend`フォルダーに移動します。 `npm start` コマンドを実行します。

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

4. 新しいブラウザータブを開き（まだ開いていない場合）、[http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)に移動します。

   ![Webpack Devサーバー — プロキシjson](assets/integrate-spa/webpack-dev-server-1.png)

   AEMと同じコンテンツが表示されますが、有効になっているオーサリング機能が1つもない場合は表示されます。

5. IDEに戻り、`img`という名前の新しいフォルダを`ui.frontend/src/assets`に作成します。
6. 次のWKNDロゴをダウンロードして`img`フォルダーに追加します。

   ![WKNDロゴ](./assets/integrate-spa/wknd-logo-dk.png)

7. **header.component.html**&#x200B;を`ui.frontend/src/app/components/header/header.component.html`で開き、ロゴを含めます。

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   **header.component.html**&#x200B;に変更を保存します。

8. ブラウザーに戻ります。 アプリに対する変更がすぐに反映されていることを確認します。

   ![ヘッダーに追加されたロゴ](assets/integrate-spa/added-logo-localhost.png)

   コンテンツをプロキシしているので、**AEM**&#x200B;でコンテンツを更新し続けて、**webpack dev server**&#x200B;に反映されているのを確認できます。 内容の変更は、**webpack dev server**&#x200B;にのみ表示されます。

9. ターミナルで`ctrl+c`を使用してローカルWebサーバーを停止します。

## Webpack Dev Server - Mock JSON API {#mock-json}

迅速な開発のもう1つのアプローチは、静的なJSONファイルを使用してJSONモデルとして機能させることです。 JSONを「モッキング」することで、ローカルのAEMインスタンスへの依存関係を削除します。 また、フロントエンド開発者がJSONモデルを更新して機能をテストし、変更をJSON APIに導き、後でバックエンド開発者が実装することになります。

モックJSONの最初の設定では、**ローカルのAEMインスタンス**&#x200B;が必要です。

1. ブラウザーで、[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)に移動します。

   これは、アプリケーションを実行している AEM から書き出した JSON です。JSON 出力をコピーします。

2. IDEに戻り、`ui.frontend/src`に移動し、**mocks**&#x200B;と&#x200B;**json**&#x200B;という名前の新しいフォルダを追加して、次のフォルダ構造に一致させます。

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. `ui.frontend/public/mocks/json`の下に、**en.model.json**&#x200B;という名前の新しいファイルを作成します。 **手順1**&#x200B;のJSON出力をここに貼り付けます。

   ![モックモデルJSONファイル](assets/integrate-spa/mock-model-json-created.png)

4. `ui.frontend`の下に、新しいファイル&#x200B;**proxy.mock.conf.json**&#x200B;を作成します。 ファイルに以下のように入力します。

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

   このプロキシ設定は、`/content/wknd-spa-angular/us`と`/mocks/json`の開始を書き換え、対応する静的JSONファイルを提供します。例：

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. ファイル&#x200B;**angular.json**&#x200B;を開きます。 新し追加い&#x200B;**dev**&#x200B;設定で、**mocks**&#x200B;フォルダーを参照する更新された&#x200B;**assets**&#x200B;配列が作成されます。

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

   ![AngularJSON開発アセットの更新フォルダー](assets/integrate-spa/dev-assets-update-folder.png)

   専用の&#x200B;**dev**&#x200B;設定を作成すると、**mocks**&#x200B;フォルダーは開発時にのみ使用され、実稼働ビルドではAEMにはデプロイされません。

6. **angular.json**&#x200B;ファイルで、次に&#x200B;**browserTarget**&#x200B;の設定を更新し、新しい&#x200B;**dev**&#x200B;の設定を使用します。

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

   ![AngularJSONビルド開発の更新](assets/integrate-spa/angular-json-build-dev-update.png)

7. ファイル`ui.frontend/package.json`を開き、新しい&#x200B;**開始:mock**&#x200B;コマンドを追加して、**proxy.mock.conf.json**&#x200B;ファイルを参照します。

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

8. 現在実行中の場合は、**webpack devサーバ**&#x200B;を停止します。 **webpack dev server**&#x200B;を&#x200B;**開始:mock**&#x200B;スクリプトを使用して開始します。

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)に移動すると、同じSPAが表示されますが、コンテンツが&#x200B;**mock** JSONファイルから取り込まれます。

9. 先ほど作成した&#x200B;**en.model.json**&#x200B;ファイルに小さな変更を加えます。 更新されたコンテンツは、すぐに&#x200B;**webpack dev server**&#x200B;に反映されます。

   ![モデルのjsonの更新](./assets/integrate-spa/webpack-mock-model.gif)

   JSONモデルを操作して、本番用SPA上での影響を確認できることは、開発者がJSONモデルAPIを理解するのに役立ちます。 また、フロントエンドとバックエンドの両方の開発を並行して行うこともできます。

## Sassで追加のスタイル

次に、更新された一部のスタイルがプロジェクトに追加されます。 このプロジェクトは、変数のような役に立つ機能を[Sass](https://sass-lang.com/)に追加します。

1. ターミナルウィンドウを開き、起動した場合は&#x200B;**webpack dev server**&#x200B;を停止します。 `ui.frontend`フォルダー内から次のコマンドを入力し、**.scss**&#x200B;ファイルを処理するようにAngularアプリを更新します。

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   これにより、`angular.json`ファイルが更新され、ファイルの下部に新しいエントリが追加されます。

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. `normalize-scss`をインストールして、スタイルをブラウザー間で標準化します。

   ```shell
   $ npm install normalize-scss --save
   ```

3. IDEに戻り、`ui.frontend/src`の下に`styles`という名前の新しいフォルダを作成します。
4. `ui.frontend/src/styles`の下に`_variables.scss`という名前の新しいファイルを作成し、次の変数を設定します。

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

5. `ui.frontend/src/styles.css`にある&#x200B;**styles.css**&#x200B;ファイルの拡張子をから&#x200B;**styles.scss**&#x200B;に変更します。 内容を次の内容に置き換えます。

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

6. **angular.json**&#x200B;を更新し、**style.css**&#x200B;へのすべての参照を&#x200B;**styles.scss**&#x200B;で再命名します。 参考文献は3つあります。

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## ヘッダースタイルを更新

次に、Sassを使用して&#x200B;**ヘッダー**&#x200B;コンポーネントにブランド固有のスタイルを追加します。

1. **webpack dev server**&#x200B;に開始し、スタイルの更新をリアルタイムで確認します。

   ```shell
   $ npm run start:mock
   ```

2. `ui.frontend/src/app/components/header`の下で、**header.component.css**&#x200B;を&#x200B;**header.component.scss**&#x200B;に名前変更します。 ファイルに以下のように入力します。

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

3. **header.component.js**&#x200B;を更新して、**header.component.scss**&#x200B;を参照します。

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

4. ブラウザに戻り、**webpack dev server**:

   ![スタイル設定ヘッダー — webpack devサーバー](assets/integrate-spa/styled-header.png)

   これで、**ヘッダー**&#x200B;コンポーネントに更新されたスタイルが追加されます。

## SPAアップデートをAEMに展開する

**ヘッダ**&#x200B;に加えられた変更は、現在、**webpack devサーバ**&#x200B;を通してのみ表示されます。 更新したSPAをAEMにデプロイして、変更を確認します。

1. **webpack dev server**&#x200B;を停止します。
2. プロジェクト`/aem-guides-wknd-spa`のルートに移動し、Mavenを使用してAEMにプロジェクトをデプロイします。

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)に移動します。 更新された&#x200B;**ヘッダー**&#x200B;が表示され、ロゴとスタイルが適用されています。

   ![AEMの更新されたヘッダー](assets/integrate-spa/final-header-component.png)

   更新されたSPAがAEMになったので、オーサリングを続行できます。

## これで完了です! {#congratulations}

SPAを更新し、AEMとの統合を確認しました。 これで、**webpack dev server**&#x200B;を使用してAEM JSONモデルAPIに対してSPAを開発する2つの異なる方法がわかります。

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution)に常に表示できます。また、ブランチ`Angular/integrate-spa-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。

### 次の手順 {#next-steps}

[SPAコンポーネントのAEMコンポーネントへのマッピング](map-components.md) - AEMSPA  Editor JS SDKを使用して、AngularコンポーネントをAdobe Experience Manager(AEM)コンポーネントにマッピングする方法について説明します。コンポーネントマッピングを使用すると、従来のAEMオーサリングと同様、AEM SPAエディタ内でSPAコンポーネントに対して動的な更新を行うことができます。
