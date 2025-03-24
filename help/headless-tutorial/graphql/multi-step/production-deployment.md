---
title: AEM パブリッシュサービスを使用した実稼動のデプロイメント - AEM ヘッドレス入門 - GraphQL
description: AEM オーサーサービスとパブリッシュサービス、ヘッドレスアプリケーションで推奨されるデプロイメントパターンについて説明します。このチュートリアルでは、環境変数を使用して、ターゲット環境に基づいて GraphQL エンドポイントを動的に変更する方法を説明します。クロスオリジンリソース共有（CORS）用に AEM を適切に設定する方法を説明します。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-7131
thumbnail: KT-7131.jpg
exl-id: 8c8b2620-6bc3-4a21-8d8d-8e45a6e9fc70
duration: 486
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2137'
ht-degree: 100%

---

# AEM パブリッシュサービスを使用した実稼動のデプロイメント

このチュートリアルでは、作成者インスタンスからパブリッシュインスタンスに配信されるコンテンツをシミュレートするローカル環境を設定します。また、GraphQL API を使用して、AEM パブリッシュ環境からコンテンツを使用するように設定された React アプリの実稼動ビルドを生成します。この過程で、環境変数を効果的に使用する方法と、AEM CORS 設定を更新する方法について学びます。

## 前提条件

このチュートリアルは、マルチパートチュートリアルの一部です。前述の手順が完了したと仮定します。

## 目的

次の方法を学びます。

* AEM 作成者とパブリッシュのアーキテクチャを理解します。
* 環境変数を管理するためのベストプラクティスを学びます。
* クロスオリジンリソース共有（CORS）用に AEM を適切に設定する方法を学びます。

## オーサーパブリッシュデプロイメントパターン {#deployment-pattern}

完全な AEM 環境は、オーサー、パブリッシュ、Dispatcher で構成されます。オーサーサービスでは、内部ユーザーがコンテンツの作成、管理、プレビューを行います。パブリッシュサービスは「実稼働」環境と考えられ、通常はエンドユーザーがやり取りする場所になります。コンテンツは、オーサーサービスで編集および承認された後、パブリッシュサービスに配信されます。

AEM ヘッドレスアプリケーションで最も一般的なデプロイメントパターンは、実稼動版のアプリケーションを AEM パブリッシュサービスに接続させることです。

![大まかなデプロイメントパターン](assets/publish-deployment/high-level-deployment.png)

上の図は、この一般的なデプロイメントパターンを示しています。

1. **コンテンツ作成者**&#x200B;は、AEM 作成者サービスを使用して、コンテンツを作成、編集および管理します。
2. **コンテンツ作成者**&#x200B;とその他の内部ユーザーは、コンテンツをオーサーサービスで直接プレビューできます。オーサーサービスに接続する、アプリケーションのプレビューバージョンをセットアップできます。
3. 承認されたコンテンツは、AEM パブリッシュサービスに&#x200B;**公開**&#x200B;できます。
4. **エンドユーザー**&#x200B;は、アプリケーションの実稼動バージョンを操作します。実稼動アプリケーションは、Dispatcher を介してパブリッシュサービスに接続し、GraphQL API を使用してコンテンツをリクエストおよび消費します。

このチュートリアルでは、AEM パブリッシュインスタンスを現在の設定に追加することで、上記のデプロイメントをシミュレートします。 以前の章では、React アプリは、オーサーインスタンスに直接接続することで、プレビューとして機能しました。React App の実稼動ビルドは、新しいパブリッシュインスタンスに接続する静的な Node.js サーバーにデプロイされます。

最後に、3 台のローカルサーバーが実行されています。

* http://localhost:4502 - オーサーインスタンス
* http://localhost:4503 - パブリッシュインスタンス
* http://localhost:5000 - 実稼動モードでの React アプリ、パブリッシュインスタンスに接続します。

## AEM SDK のインストール - パブリッシュモード {#aem-sdk-publish}

現在、SDK の実行インスタンスを&#x200B;**オーサー**&#x200B;モードで使用しています。SDK は、**パブリッシュ**&#x200B;モードを使用して、AEM パブリッシュ環境をシミュレートします。

ローカル開発環境の設定に関する詳細なガイドは、[こちら](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja#local-development-environment-set-up)を参照してください。

1. ローカルファイルシステムで、パブリッシュインスタンスをインストールする専用のフォルダーを作成します。つまり、`~/aem-sdk/publish` と名前を付けます。
1. 以前の章のオーサーインスタンスで使用した Quickstart Jar ファイルをコピーし、`publish` ディレクトリに貼り付けます。または、[ソフトウェア配布ポータル](https://experience.adobe.com/#/downloads/content/software-distribution/jp/aemcloud.html)に移動し、最新の SDK をダウンロードして、Quickstart jar ファイルを抽出します。
1. jar ファイルの名前を `aem-publish-p4503.jar` に変更します。

   `publish` 文字列は、Quickstart jar がパブリッシュモードで開始することを指定します。この `p4503` は、Quickstart サーバーがポート 4503 で実行されることを指定します。

1. 新しいターミナルウィンドウを開き、jar ファイルを含むフォルダーに移動します。AEM インスタンスをインストールして開始します。

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. `admin` として管理者パスワードを入力します。任意の管理者パスワードを使用できますが、追加の設定を避けるために、ローカル開発にはデフォルトを使用します。
1. AEM インスタンスのインストールが完了すると、新しいブラウザーウィンドウが [http://localhost:4503/content.html](http://localhost:4503/content.html) で開きます

   404 Not Found というページが返されることが予想されます。これは新しい AEM インスタンスであり、コンテンツはインストールされていません。

## サンプルコンテンツと GraphQL エンドポイントのインストール {#wknd-site-content-endpoints}

オーサーインスタンスと同様に、パブリッシュインスタンスも GraphQL エンドポイントを有効にして、サンプルコンテンツを必要とします。次に、WKND 参照サイトをパブリッシュインスタンスにインストールします。

1. WKND サイト用に最新のコンパイル済み AEM パッケージ（[aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest)）をダウンロードします。

   >[!NOTE]
   >
   > `classic` バージョン&#x200B;**ではなく**、AEM as a Cloud Service と互換性のある標準バージョンをダウンロードします。

1. [http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html) に直接移動し、ユーザー名 `admin` とパスワード `admin` を使用して、パブリッシュインスタンスにログインします。
1. 次に、パッケージマネージャー（[http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp)）に移動します。
1. 「**パッケージをアップロード**」をクリックし、前の手順でダウンロードした WKND パッケージを選択します。「**インストール**」をクリックして、パッケージをインストールします。
1. パッケージをインストールすると、WKND リファレンスサイト（[http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/acme/us/.html)）が利用できるようになります。
1. メニューバーの「ログアウト」ボタンをクリックして、`admin` ユーザーとしてログアウトします。

   ![WKND ログアウトリファレンスサイト](assets/publish-deployment/sign-out-wknd-reference-site.png)

   AEM オーサーインスタンスとは異なり、AEM パブリッシュインスタンスにはデフォルトで匿名の読み取り専用アクセス権があります。React アプリケーションの実行時に、匿名ユーザーのエクスペリエンスをシミュレートします。

## パブリッシュインスタンスを指すように環境変数を更新する {#react-app-publish}

次に、React アプリケーションで使用される環境変数を更新し、パブリッシュインスタンスを指すようにします。React アプリは、実稼働モードのパブリッシュインスタンスに&#x200B;**のみ**&#x200B;接続する必要があります。

次に、新規ファイル `.env.production.local` を追加して、実稼働環境をシミュレートします。

1. IDE で WKND GraphQL React アプリを開きます。

1. `aem-guides-wknd-graphql/react-app` の下に、`.env.production.local` という名前のファイルを追加します。
1. `.env.production.local` に以下を入力します。

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![新しい環境変数ファイルを追加](assets/publish-deployment/env-production-local-file.png)

   環境変数を使用すると、アプリケーションコード内にロジックを追加することなく、GraphQL エンドポイントをオーサー環境とパブリッシュ環境の間で簡単に切り替えることができます。[React のカスタム環境変数の詳細については、こちら](https://create-react-app.dev/docs/adding-custom-environment-variables)を参照してください。

   >[!NOTE]
   >
   > パブリッシュ環境はデフォルトでコンテンツへの匿名アクセスを提供するので、認証情報は含まれていません。

## 静的ノードサーバのデプロイ {#static-server}

React アプリは webpack サーバーを使用して開始できますが、これは開発専用です。次に、[serve](https://github.com/vercel/serve) を使用して実稼動環境のデプロイをシミュレートし、Node.js を使用して React アプリの実稼動ビルドをホストします。

1. 新しいターミナルウィンドウを開き、`aem-guides-wknd-graphql/react-app` ディレクトリに移動します

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. 次のコマンドで [serve](https://github.com/vercel/serve) をインストールします。

   ```shell
   $ npm install serve --save-dev
   ```

1. `react-app/package.json` の `package.json` ファイルを開きます。`serve` という名前のスクリプトを追加します。

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   `serve` スクリプトは 2 つのアクションを実行します。まず、React App の実稼動ビルドが生成されます。次に、Node.js サーバーが起動し、実稼動ビルドを使用します。

1. ターミナルに戻り、次のコマンドを入力して静的サーバーを開始します。

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

1. 新しいブラウザーを開き、[http://localhost:5000/](http://localhost:5000/) に移動します。React アプリが提供されていることを確認します。

   ![React アプリの提供](assets/publish-deployment/react-app-served-port5000.png)

   GraphQL クエリがホームページで機能していることを確認します。開発者ツールを使用して **XHR** リクエストを調べます。GraphQL POST が `http://localhost:4503/content/graphql/global/endpoint.json` のパブリッシュインスタンスに送信されていることを確認します。

   ただし、ホームページ上のすべての画像が壊れます。

1. Adventure Detail ページの 1 つをクリックします。

   ![Adventure Detail エラー](assets/publish-deployment/adventure-detail-error.png)

   `adventureContributor` のGraphQLエラーが発生したことがわかります。次の演習では、壊れた画像と `adventureContributor` 問題が修正されました。

## 画像絶対参照 {#absolute-image-references}

画像が破損したように見えるのは、`<img src` 属性が相対パスに設定され、最終的に `http://localhost:5000/` のノード静的サーバーを指すようになるからです。代わりに、これらの画像は AEM パブリッシュインスタンスを指す必要があります。 これに対して、いくつかの解決策が考えられます。 webpack 開発サーバーを使用する場合、ファイル `react-app/src/setupProxy.js` は、`/content` へのすべてのリクエストに対して、webpack サーバーと AEM オーサー インスタンスの間にプロキシを設定します。プロキシ設定は、実稼動環境で使用できますが、web サーバーレベルで設定する必要があります。例： [Apache のプロキシモジュール](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html)。

アプリを更新し、`REACT_APP_HOST_URI` 環境変数を使用して絶対 URL を含むようにできます。代わりに、AEM GraphQL API の機能を使用して、画像の絶対 URL をリクエストします。

1.  Node.js サーバーを停止します。
1. IDE に戻り、`react-app/src/components/Adventures.js` でファイル `Adventures.js` を開きます。
1. `_publishUrl` プロパティを `allAdventuresQuery` 内で `ImageRef` に追加します。

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

   `_publishUrl` および `_authorUrl` は、絶対 URL を簡単に含めることができるように `ImageRef` オブジェクトに内蔵されています。

1. 上記の手順を繰り返して、`filterQuery(activity)` 関数で使用されているクエリを修正し `_publishUrl` プロパティを含みます。
1. `function AdventureItem(props)` の `AdventureItem` コンポーネントを変更して、`<img src=''>` タグを作成するときに `_path` プロパティではなく `_publishUrl` を参照するようにします。

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. `react-app/src/components/AdventureDetail.js` の `AdventureDetail.js` ファイルを開きます。
1. 同じ手順を繰り返して、GraphQL クエリを変更し、アドベンチャーの `_publishUrl` プロパティを追加します。

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

1. `AdventureDetail.js` で Adventure のメイン画像と、寄稿者画像参照の 2 つの `<img>` タグを変更します。

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

1. [http://localhost:5000/](http://localhost:5000/) に移動して、画像が現れ、`<img src''>` 属性が `http://localhost:4503` を指すことを確認します。

   ![壊れた画像を修正](assets/publish-deployment/broken-images-fixed.png)

## コンテンツ公開をシミュレート {#content-publish}

Adventure Details ページがリクエストされたとき、`adventureContributor` の GraphQL エラーが発生することを思い出してください。**投稿者**&#x200B;コンテンツフラグメントモデルがパブリッシュインスタンスにまだ存在しません。 また、**Adventure** コンテンツフラグメントモデルに追加された更新は、パブリッシュインスタンスでは使用できません。 これらの変更はオーサーインスタンスに直接加えられたので、パブリッシュインスタンスに配布する必要があります。

これは、コンテンツフラグメントまたはコンテンツフラグメントモデルの更新に依存するアプリケーションに対して新しい更新を展開する際に考慮すべきことです。

次に、ローカルのオーサーインスタンスとパブリッシュインスタンス間でのコンテンツの公開をシミュレートします。

1. オーサーインスタンスを起動し（まだ起動していない場合）、パッケージマネージャー（ [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)）に移動します。
1. パッケージ（[EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip)）をダウンロードし、パッケージマネージャーを使用してインストールします。

   このパッケージでは、オーサーインスタンスがコンテンツをパブリッシュインスタンスに公開できるようにする設定がインストールされます。 この設定を手動で行う手順については、[こちら](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ja#content-distribution)を参照してください。

   >[!NOTE]
   >
   > AEM as a Cloud Service 環境では、オーサー層は、コンテンツをパブリッシュ層に配布するように自動的に設定されます。

1.  **AEM 開始**&#x200B;メニューから、**ツール**／**アセット**／**コンテンツフラグメントモデル**&#x200B;に移動します。

1.  **WKND サイト**&#x200B;フォルダーをクリックします。

1. 3 つのモデルをすべて選択し、「**公開**」をクリックします。

   ![コンテンツフラグメントモデルの公開](assets/publish-deployment/publish-contentfragment-models.png)

   確認ダイアログが表示されるので、「**公開**」をクリックします。

1. Bali Surf Camp コンテンツフラグメント（[http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)) に移動します。

1. 上部メニューバーの「**公開**」ボタンをクリックします。

   ![コンテンツフラグメントエディターでの「公開」ボタンのクリック](assets/publish-deployment/publish-bali-content-fragment.png)

1. 公開ウィザードに、公開すべき依存アセットが表示されます。 この場合、参照先のフラグメント **stacey-roswells** が一覧表示され、複数の画像も参照されます。 参照されるアセットがフラグメントと共に公開されます。

   ![公開する参照先のアセット](assets/publish-deployment/referenced-assets.png)

   「**公開**」ボタンを再度クリックして、コンテンツフラグメントと依存アセットを公開します。

1. [http://localhost:5000/](http://localhost:5000/) で実行中の React アプリに戻ります。Bali Surf Camp 内をクリックして、アドベンチャーの詳細を確認することができます。

1. AEM オーサーインスタンス（[http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)）に戻り、フラグメントの **タイトル** をアップデートします。フラグメントを **保存して閉じます**。 次に、フラグメントを&#x200B;**公開**&#x200B;します。
1. [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) に戻り、公開された変更を確認します。

   ![Bali Surf Camp パブリッシュの更新](assets/publish-deployment/bali-surf-camp-update.png)

## COR 設定の更新

AEM はデフォルトで保護されており、AEM 以外の web プロパティがクライアントサイドの呼び出しを行うことはできません。AEM のクロスオリジンリソース共有（CORS）設定を使用すると、特定のドメインで AEM を呼び出すことができます。

次に、AEM パブリッシュインスタンスの CORS 設定を使用して実験します。

1. コマンド `npm run serve` を使用して、React アプリが動作しているターミナルウィンドウに戻ります。

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

   2 つの URL が指定されていることを確認します。 1 つは `localhost` を使用しており、もう 1 つはローカルネットワークの IP アドレスを使用しています。

1. [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) で始まるアドレスに移動します。このアドレスは、ローカルコンピューターごとに少し異なります。 データを取得する際に、CORS エラーが発生することを確認します。 これは、現在の CORS 設定では `localhost` からのリクエストのみが許可されているからです。

   ![CORS エラー](assets/publish-deployment/cors-error-not-fetched.png)

   次に、AEM パブリッシュ CORS の設定を更新して、ネットワーク IP アドレスからのリクエストを許可します。

1. [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html) に移動し、ユーザー名 `admin` およびパスワード `admin` でログインします。

1. [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr) に移動し、WKND GraphQL 設定を `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql` で見つけます。

1. 「**許可されたオリジン**」フィールドを更新し、ネットワーク IP アドレスを追加します。

   ![CORS 設定の更新](assets/publish-deployment/cors-update.png)

   また、特定のサブドメインからのすべてのリクエストを許可する正規表現を含めることもできます。 変更を保存します。

1. **Apache Sling Referrer Filter** を検索し、設定を確認します。外部ドメインからの GraphQL リクエストを有効にするには、「**Allow Empty**」設定も必要です。

   ![Sling Referrer Filter](assets/publish-deployment/sling-referrer-filter.png)

   これらは、WKND リファレンスサイトの一部として設定されています。 OSGi 設定の完全なセットは、[GitHub リポジトリ](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig)を使用して確認することができます。

   >[!NOTE]
   >
   > OSGi 設定は、ソース管理にコミットされた AEM プロジェクトで管理されます。AEM プロジェクトは、Cloud Manager を使用して、AEM as a Cloud Service 環境にデプロイすることができます。[AEM プロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)は、特定の実装のプロジェクトを生成するのに役立ちます。

1. [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) で始まる React アプリに戻り、アプリケーションが CORS エラーをスローしなくなったことを確認します。

   ![修正された CORS エラー](assets/publish-deployment/cors-error-corrected.png)

## おめでとうございます。 {#congratulations}

おめでとうございます。これで、AEM パブリッシュ環境を使用した実稼働デプロイメント全体のシミュレーションが完了しました。また、AEM での CORS 設定の使用方法についても学びました。

## その他のリソース

コンテンツフラグメントと GraphQL について詳しくは、以下のリソースを参照してください。

* [コンテンツフラグメントと GraphQL を使用したヘッドレスコンテンツ配信](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html?lang=ja)
* [コンテンツフラグメントと共に使用する AEM GraphQL API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=ja)
* [トークンベースの認証](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=ja#authentication)
* [AEM as a Cloud Service へのコードのデプロイ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=ja#cloud-manager)
