---
title: AEMパブリッシュサービスを使用した実稼動のデプロイメント — AEMヘッドレスの概要 — GraphQL
description: AEMオーサーサービスとパブリッシュサービス、およびヘッドレスアプリケーション向けに推奨されるデプロイメントパターンについて説明します。 このチュートリアルでは、環境変数を使用して、ターゲット環境に基づいてGraphQLエンドポイントを動的に変更する方法を説明します。 クロスオリジンリソース共有(CORS)用にAEMを適切に設定する方法を説明します。
sub-product: アセット
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 7131
thumbnail: KT-7131.jpg
source-git-commit: 81626b8d853f3f43d9c51130acf02561f91536ac
workflow-type: tm+mt
source-wordcount: '2361'
ht-degree: 4%

---


# AEMパブリッシュサービスを使用した実稼動デプロイメント

このチュートリアルでは、オーサーインスタンスからパブリッシュインスタンスにコンテンツが配布されるシミュレートをおこなうローカル環境を設定します。 また、GraphQL APIを使用して、AEMパブリッシュ環境のコンテンツを使用するように設定されたReactアプリの実稼動ビルドを生成します。 途中で、環境変数を効果的に使用する方法と、AEM CORS設定を更新する方法について学習します。

## 前提条件

このチュートリアルは、複数のパートから成るチュートリアルの一部です。 前述の手順が完了したことを前提としています。

## 目的

以下の方法を説明します。

* AEMオーサーとパブリッシュのアーキテクチャを理解します。
* 環境変数を管理するためのベストプラクティスを説明します。
* クロスオリジンリソース共有(CORS)用にAEMを適切に設定する方法を説明します。

## オーサーパブリッシュのデプロイメントパターン{#deployment-pattern}

完全なAEM環境は、オーサー、パブリッシュ、ディスパッチャーで構成されます。 オーサーサービスは、内部ユーザーがコンテンツを作成、管理、プレビューする場所です。 パブリッシュサービスは「ライブ」環境と見なされ、通常はエンドユーザーがやり取りします。 コンテンツは、オーサーサービスで編集および承認された後、パブリッシュサービスに配信されます。

AEMヘッドレスアプリケーションで最も一般的なデプロイメントパターンは、実稼動版のアプリケーションをAEMパブリッシュサービスに接続させることです。

![デプロイメントの概要パターン](assets/publish-deployment/high-level-deployment.png)

上の図は、この一般的なデプロイメントパターンを示しています。

1. **コンテンツ作成者**&#x200B;は、AEMオーサーサービスを使用して、コンテンツを作成、編集および管理します。
2. **コンテンツ作成者**&#x200B;および他の内部ユーザーは、Authorサービスで直接コンテンツをプレビューできます。 オーサーサービスに接続するアプリケーションのプレビューバージョンを設定できます。
3. 承認されたコンテンツは、AEMパブリッシュサービスに&#x200B;**公開**&#x200B;できます。
4. **エンドユ** ーザーは、アプリケーションの実稼動バージョンを操作します。実稼動アプリケーションはパブリッシュサービスに接続し、GraphQL APIを使用してコンテンツを要求し、利用します。

このチュートリアルでは、AEMパブリッシュインスタンスを現在のセットアップに追加することで、上記のデプロイメントをシミュレートします。 以前の章では、React Appは、オーサーインスタンスに直接接続することでプレビューとして機能しました。 React Appの実稼動ビルドは、新しいパブリッシュインスタンスに接続する静的なNode.jsサーバーにデプロイされます。

最後に、3台のローカルサーバーが実行されます。

* http://localhost:4502 — オーサーインスタンス
* http://localhost:4503 — パブリッシュインスタンス
* http://localhost:5000 — 実稼動モードでのReactアプリ。パブリッシュインスタンスに接続します。

## AEM SDKのインストール — パブリッシュモード{#aem-sdk-publish}

現在、**Author**&#x200B;モードでSDKのインスタンスが実行されています。 SDKは、**パブリッシュ**&#x200B;モードで起動して、AEMパブリッシュ環境をシミュレートすることもできます。

ローカル開発環境の設定に関する詳細なガイドは[こちら](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja#local-development-environment-set-up)を参照してください。

1. ローカルファイルシステムで、パブリッシュインスタンスをインストールする専用のフォルダー(`~/aem-sdk/publish`)を作成します。
1. 前の章のオーサーインスタンスで使用したクイックスタートjarファイルをコピーし、`publish`ディレクトリに貼り付けます。 または、[ソフトウェア配布ポータル](https://experience.adobe.com/#/downloads/content/software-distribution/ja/aemcloud.html)に移動し、最新のSDKをダウンロードして、クイックスタートjarファイルを抽出します。
1. jarファイルの名前を`aem-publish-p4503.jar`に変更します。

   `publish`文字列は、クイックスタートjarがパブリッシュモードで開始することを指定します。 `p4503`は、クイックスタートサーバーがポート4503で実行されることを指定します。

1. 新しいターミナルウィンドウを開き、jarファイルを含むフォルダーに移動します。 AEMインスタンスをインストールして起動します。

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. 管理パスワードとして`admin`を指定します。 adminパスワードは使用できますが、追加の設定を避けるために、ローカル開発のデフォルトを使用することをお勧めします。
1. AEMインスタンスのインストールが完了すると、新しいブラウザーウィンドウが[http://localhost:4503/content.html](http://localhost:4503/content.html)に開きます。

   404 Not Foundページが返されることが予想されます。 これは新しいAEMインスタンスで、コンテンツがインストールされていません。

## サンプルコンテンツとGraphQLエンドポイント{#wknd-site-content-endpoints}のインストール

オーサーインスタンスと同様に、パブリッシュインスタンスではGraphQLエンドポイントを有効にし、サンプルコンテンツを必要とします。 次に、 WKND参照サイトをパブリッシュインスタンスにインストールします。

1. WKNDサイト用の最新のコンパイル済みAEMパッケージをダウンロードします。[aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest)

   >[!NOTE]
   >
   > AEM as aCloud Serviceとして互換性のある標準バージョンをダウンロードし、`classic`バージョンでは&#x200B;**ない**&#x200B;をダウンロードしてください。

1. 次の場所に直接移動して、パブリッシュインスタンスにログインします。[http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html)（ユーザー名`admin`とパスワード`admin`を含む）
1. 次に、[http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp)のパッケージマネージャーに移動します。
1. 「**パッケージをアップロード**」をクリックし、前の手順でダウンロードしたWKNDパッケージを選択します。 「**インストール**」をクリックして、パッケージをインストールします。
1. パッケージをインストールした後、WKNDリファレンスサイトが[http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html)から入手できるようになりました。
1. メニューバーの「サインアウト」ボタンをクリックして、`admin`ユーザーとしてサインアウトします。

   ![WKNDサインアウトリファレンスサイト](assets/publish-deployment/sign-out-wknd-reference-site.png)

   AEMオーサーインスタンスとは異なり、AEMパブリッシュインスタンスは、デフォルトで匿名の読み取り専用アクセスに設定されます。 Reactアプリケーションを実行する際の匿名ユーザーのエクスペリエンスをシミュレートします。

## 環境変数を更新して、パブリッシュインスタンス{#react-app-publish}を指すようにします。

次に、Reactアプリケーションが使用する環境変数を更新し、パブリッシュインスタンスを指すようにします。 Reactアプリは、実稼動モードでパブリッシュインスタンスに&#x200B;**のみ**&#x200B;接続する必要があります。

次に、新しいファイル`.env.production.local`を追加して、実稼動環境をシミュレートします。

1. IDEでWKND GraphQL Reactアプリを開きます。

1. `aem-guides-wknd-graphql/react-app`の下に、`.env.production.local`という名前のファイルを追加します。
1. `.env.production.local` に以下を入力します。

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![新しい環境変数ファイルを追加します](assets/publish-deployment/env-production-local-file.png)

   環境変数を使用すると、アプリケーションコード内にロジックを追加することなく、オーサー環境とパブリッシュ環境の間でGraphQLエンドポイントを簡単に切り替えることができます。 Reactの[カスタム環境変数の詳細については、](https://create-react-app.dev/docs/adding-custom-environment-variables)を参照してください。

   >[!NOTE]
   >
   > パブリッシュ環境は、デフォルトでコンテンツへの匿名アクセスを提供するので、認証情報は含まれていないことを確認します。

## 静的ノードサーバ{#static-server}の展開

Reactアプリはwebpackサーバーを使用して起動できますが、開発専用です。 次に、[serve](https://github.com/vercel/serve)を使用して、Node.jsを使用してReactアプリの実稼動ビルドをホストすることで、実稼動デプロイメントをシミュレートします。

1. 新しいターミナルウィンドウを開き、`aem-guides-wknd-graphql/react-app`ディレクトリに移動します。

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. [serve](https://github.com/vercel/serve)を次のコマンドでインストールします。

   ```shell
   $ npm install serve --save-dev
   ```

1. `package.json`（`react-app/package.json`）ファイルを開きます。`serve`という名前のスクリプトを追加します。

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   `serve`スクリプトは、2つのアクションを実行します。 まず、React Appの実稼動ビルドが生成されます。 次に、Node.jsサーバーが起動し、実稼動ビルドを使用します。

1. ターミナルに戻り、コマンドを入力して静的サーバーを起動します。

   ```shell
   $ npm run serve
   
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.111:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

1. 新しいブラウザーを開き、[http://localhost:5000/](http://localhost:5000/)に移動します。 Reactアプリが提供されていることを確認します。

   ![Reactアプリが提供されました](assets/publish-deployment/react-app-served-port5000.png)

   GraphQLクエリは、ホームページで動作しています。 Inspect **XHR**&#x200B;リクエストを開発者ツールを使用して実行します。 GraphQLPOSTが`http://localhost:4503/content/graphql/global/endpoint.json`のパブリッシュインスタンスに送信されることを確認します。

   ただし、ホームページ上のすべての画像が壊れます。

1. アドベンチャーの詳細ページの1つをクリックします。

   ![アドベンチャー詳細エラー](assets/publish-deployment/adventure-detail-error.png)

   `adventureContributor`に対してGraphQLエラーがスローされることを確認します。 次の演習では、壊れた画像と`adventureContributor`の問題を修正します。

## 絶対画像参照{#absolute-image-references}

`<img src`属性が相対パスに設定され、`http://localhost:5000/`にあるNode静的サーバーを指しているので、画像が壊れているように見えます。 代わりに、これらの画像はAEMパブリッシュインスタンスを指す必要があります。 これに対して、いくつかの解決策が考えられます。 webpack開発サーバーを使用する場合、ファイル`react-app/src/setupProxy.js`は、`/content`に対する要求に対して、webpackサーバーとAEMオーサーインスタンスの間にプロキシを設定します。 プロキシ設定は、実稼動環境で使用できますが、Webサーバーレベルで設定する必要があります。 例えば、[Apacheのプロキシモジュール](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html)です。

環境変数`REACT_APP_HOST_URI`を使用して、絶対URLを含むようにアプリを更新できます。 代わりに、AEM GraphQL APIの機能を使用して、画像の絶対URLを要求します。

1. Node.jsサーバーを停止します。
1. IDEに戻り、`react-app/src/components/Adventures.js`にある`Adventures.js`ファイルを開きます。
1. `_publishUrl`プロパティを`allAdventuresQuery`内の`ImageRef`に追加します。

   ```diff
   const allAdventuresQuery = `
   {
       adventureList {
       items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
           }
           }
       }
       }
   }
   `;
   ```

   `_publishUrl` と `_authorUrl` は、絶対URLを含めやすくす `ImageRef` るために、オブジェクトに組み込まれた値です。

1. 上記の手順を繰り返して、`filterQuery(activity)`関数で使用するクエリを変更し、`_publishUrl`プロパティを含めます。
1. `<img src=''>`タグを作成する際に、`function AdventureItem(props)`の`AdventureItem`コンポーネントを変更し、`_path`プロパティではなく`_publishUrl`を参照します。

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. `AdventureDetail.js`（`react-app/src/components/AdventureDetail.js`）ファイルを開きます。
1. 同じ手順を繰り返してGraphQLクエリを変更し、アドベンチャー用に`_publishUrl`プロパティを追加します

   ```diff
    adventureByPath (_path: "${_path}") {
       item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
           adventureContributor {
               fullName
               occupation
               pictureReference {
                   ...on ImageRef {
                       _path
   +                   _publishUrl
                   }
               }
           }
       }
       }
   } 
   ```

1. `AdventureDetail.js`のアドベンチャープライマリ画像とコントリビューター画像の参照用の2つの`<img>`タグを変更します。

   ```diff
   /* AdventureDetail.js */
   ...
   <img className="adventure-detail-primaryimage"
   -       src={adventureData.adventurePrimaryImage._path} 
   +       src={adventureData.adventurePrimaryImage._publishUrl} 
           alt={adventureData.adventureTitle}/>
   ...
   pictureReference =  <img className="contributor-image" 
   -                        src={props.pictureReference._path}
   +                        src={props.pictureReference._publishUrl} 
                            alt={props.fullName} />
   ```

1. ターミナルに戻り、静的サーバーを起動します。

   ```shell
   $ npm run serve
   ```

1. [http://localhost:5000/](http://localhost:5000/)に移動し、画像が表示され、`<img src''>`属性が`http://localhost:4503`を指していることを確認します。

   ![壊れた画像を修正](assets/publish-deployment/broken-images-fixed.png)

## コンテンツの公開をシミュレート{#content-publish}

アドベンチャーの詳細ページが要求されると、`adventureContributor`に対してGraphQLエラーがスローされることを思い出してください。 **コントリビューター**&#x200B;コンテンツフラグメントモデルは、パブリッシュインスタンスにまだ存在しません。 **アドベンチャー**&#x200B;コンテンツフラグメントモデルに対する更新は、パブリッシュインスタンスでは利用できません。 これらの変更はオーサーインスタンスに直接加えられたので、パブリッシュインスタンスに配布する必要があります。

これは、コンテンツフラグメントまたはコンテンツフラグメントモデルの更新を利用するアプリケーションに対して新しい更新を展開する際に考慮すべき点です。

次に、ローカルのオーサーインスタンスとパブリッシュインスタンス間でのコンテンツの公開をシミュレートします。

1. オーサーインスタンスを起動し（まだ起動していない場合）、パッケージマネージャー( [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp) )に移動します。
1. パッケージ[EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip)をダウンロードし、パッケージマネージャーを使用してインストールします。

   このパッケージでは、オーサーインスタンスがパブリッシュインスタンスにコンテンツを公開できる設定がインストールされます。 [この設定の手順は、](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution)で確認できます。

   >[!NOTE]
   >
   > AEM as a Publish環境では、オーサー層は、コンテンツをパブリッシュ層に配布するように自動的に設定されます。

1. **AEM Start**&#x200B;メニューから、**ツール** / **アセット** / **コンテンツフラグメントモデル**&#x200B;に移動します。

1. **WKND Site**&#x200B;フォルダーをクリックします。

1. 3つのモデルをすべて選択し、「**公開**」をクリックします。

   ![コンテンツフラグメントモデルの公開](assets/publish-deployment/publish-contentfragment-models.png)

   確認ダイアログが表示され、「**公開**」をクリックします。

1. [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)にあるBali Surf Camp Content Fragmentに移動します。

1. 上部のメニューバーにある「**公開**」ボタンをクリックします。

   ![コンテンツフラグメントエディターの「公開」ボタンをクリックします。](assets/publish-deployment/publish-bali-content-fragment.png)

1. 公開ウィザードに、公開する必要がある依存アセットが表示されます。 この場合、参照されるフラグメント&#x200B;**stacey-roswells**&#x200B;が一覧表示され、複数の画像も参照されます。 参照元のアセットがフラグメントと共に公開されます。

   ![公開する参照元のアセット](assets/publish-deployment/referenced-assets.png)

   「**公開**」ボタンをもう一度クリックして、コンテンツフラグメントと依存アセットを公開します。

1. [http://localhost:5000/](http://localhost:5000/)で実行されているReactアプリに戻ります。 バリサーフキャンプをクリックして、冒険の詳細を見ることができます。

1. [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)にあるAEMオーサーインスタンスに戻り、フラグメントの&#x200B;**タイトル**&#x200B;を更新します。 **フラグメントを** 保存して閉じます。次に、フラグメントを&#x200B;**公開**&#x200B;します。
1. [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)に戻り、公開された変更を確認します。

   ![バリサーフキャンプパブリッシュアップデート](assets/publish-deployment/bali-surf-camp-update.png)

## COR設定の更新

AEMはデフォルトで保護されており、AEM以外のWebプロパティによるクライアント側呼び出しは許可されていません。 AEM Cross-Origin Resource Sharing(CORS)設定を使用すると、特定のドメインでAEMを呼び出すことができます。

次に、AEMパブリッシュインスタンスのCORS設定を試します。

1. React Appが実行されているターミナルウィンドウに戻り、コマンド`npm run serve`を指定します。

   ```shell
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.205:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

   2つのURLが提供されることを確認します。 1つは`localhost`を使用し、もう1つはローカルネットワークIPアドレスを使用します。

1. [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000)で始まるアドレスに移動します。 アドレスは、ローカルコンピューターごとに少し異なります。 データを取得する際に、CORSエラーが発生することを確認します。 これは、現在のCORS設定では`localhost`からのリクエストのみが許可されているからです。

   ![CORSエラー](assets/publish-deployment/cors-error-not-fetched.png)

   次に、 AEMパブリッシュCORSの設定を更新して、ネットワークIPアドレスからの要求を許可します。

1. [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html)に移動し、ユーザー名`admin`とパスワード`admin`を使用してログインします。

1. [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr)に移動し、`com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`にあるWKND GraphQL設定を探します。

1. **許可されたオリジン**&#x200B;フィールドを更新して、ネットワークIPアドレスを含めます。

   ![CORS設定の更新](assets/publish-deployment/cors-update.png)

   また、特定のサブドメインからのすべてのリクエストを許可する正規表現を含めることもできます。 変更内容を保存します。

1. **Apache Sling Referrer Filter**&#x200B;を検索し、設定を確認します。 外部ドメインからのGraphQL要求を有効にするには、**Allow Empty**&#x200B;設定も必要です。

   ![Sling Referrer Filter](assets/publish-deployment/sling-referrer-filter.png)

   これらは、WKNDリファレンスサイトの一部として設定されています。 OSGi設定の完全なセットは、[GitHubリポジトリ](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig)を通じて表示できます。

   >[!NOTE]
   >
   > OSGi設定は、ソース管理にコミットされたAEMプロジェクトで管理されます。 AEMプロジェクトは、Cloud Managerを使用して、AEM環境としてCloud Serviceにデプロイできます。 [AEMプロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)は、特定の実装のプロジェクトを生成するのに役立ちます。

1. [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000)で始まるReactアプリに戻り、アプリケーションがCORSエラーをスローしないことを確認します。

   ![CORSエラー修正](assets/publish-deployment/cors-error-corrected.png)

## バリデーターが {#congratulations}

バリデーターがこれで、AEMパブリッシュ環境を使用した実稼動デプロイメントの完全なシミュレーションが完了しました。 また、AEMでのCORS設定の使用方法も学習しました。

## その他のリソース

コンテンツフラグメントとGraphQLについて詳しくは、次のリソースを参照してください。

* [GraphQL のコンテンツフラグメントを使用したヘッドレスコンテンツ配信](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html?lang=ja)
* [コンテンツフラグメントと共に使用する AEM GraphQL API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=ja)
* [トークンベースの認証](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=ja#authentication)
* [コードのAEMへのCloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
