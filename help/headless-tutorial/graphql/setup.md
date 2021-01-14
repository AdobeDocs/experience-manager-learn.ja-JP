---
title: クイックセットアップ — AEMヘッドレスの使い始めに — GraphQL
description: Adobe Experience Manager(AEM)とGraphQLの使い始めに AEM SDKをインストールし、サンプルコンテンツを追加し、GraphQL APIを使用してAEMのコンテンツを使用するアプリケーションをデプロイします。 AEMがオムニチャネルエクスペリエンスを強化する方法を確認します。
sub-product: サイト
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
translation-type: tm+mt
source-git-commit: eb2b556c5947b15a31a74a86dadd525fb06bcf14
workflow-type: tm+mt
source-wordcount: '1599'
ht-degree: 3%

---


# クイックセットアップ{#setup}

>[!CAUTION]
>
> AEM GraphQL API for Content Fragments配信は、リクエストに応じて使用できます。
> お使いのAEM用のAPIをCloud Serviceプログラムとして有効にするには、Adobeサポートにお問い合わせください。

この章では、AEM GraphQL APIを使用して、外部環境がAEMのコンテンツを使用するのを確認するための、ローカルアプリケーションの簡単なセットアップについて説明します。 チュートリアルの後半の章では、この設定をまとめます。

## 前提条件 {#prerequisites}

次のツールをローカルにインストールする必要があります。

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+JDK%7E+1%7E&amp;orderby=%40jcr3Content%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=リスト&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10 以降](https://nodejs.org/ja/)
* [npm 6 以降](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## 目的 {#objectives}

1. AEM SDKをダウンロードしてインストールします。
1. WKNDリファレンスサイトからサンプルコンテンツをダウンロードしてインストールします。
1. GraphQL APIを使用して、コンテンツを利用するためのサンプルアプリをダウンロードしてインストールします。

## AEM SDK{#aem-sdk}のインストール

このチュートリアルでは、[AEMをCloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#aem-as-a-cloud-service-sdk)として使用し、AEM GraphQL APIを調べます。 この節では、AEM SDKをインストールして作成者モードで実行するクイックガイドを示します。 ローカル開発環境[の設定に関する詳細なガイドは、](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up)にあります。

>[!NOTE]
>
> Cloud Service環境としてAEMを使用して、チュートリアルに従うこともできます。 クラウド環境の使用に関するその他の注意事項は、このチュートリアル全体で説明しています。

1. **[Cloud Service](https://experience.adobe.com/#/downloads/content/software-distribution/jp/aemcloud.html)** > **AEMに移動して**、**AEM SDK**&#x200B;の最新バージョンをダウンロードします。

   ![ソフトウェア配布ポータル](assets/setup/software-distribution-portal-download.png)

1. ダウンロードを解凍し、Quickstart jar(`aem-sdk-quickstart-XXX.jar`)を専用のフォルダー(`~/aem-sdk/author`)にコピーします。
1. jarファイルの名前を`aem-author-p4502.jar`に変更します。
1. 新しいターミナルウィンドウを開き、jarファイルを含むフォルダに移動します。 次のコマンドを実行して、AEMインスタンスをインストールし、開始します。

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. 管理者パスワードを`admin`として指定します。 任意の管理者パスワードを使用できますが、再設定の必要性を減らすために、ローカル開発でデフォルトを使用することをお勧めします。
1. 数分後、AEMインスタンスがインストールを終了し、新しいブラウザーウィンドウが[http://localhost:4502](http://localhost:4502)に開きます。
1. ユーザー名`admin`とパスワード`admin`を使用してログインします。

>[!CAUTION]
>
> セットアップを続行するには、Quickstart SDKでGraphQL機能を手動で有効にする必要があります。 詳しい手順については、Adobeの担当者にお問い合わせください。 この手動の手順は、2021年に機能をリリースするまでのみ必要です。

## サンプルコンテンツ{#wknd-site}のインストール

WKNDリファレンスサイトのサンプルコンテンツが、チュートリアルの迅速化のためにインストールされます。 WKNDは架空のライフスタイルのブランドで、AEMトレーニングと組み合わせてよく使用されます。

1. WKND用に最新のコンパイル済みAEMパッケージをダウンロードします。[aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest)

   >[!NOTE]
   >
   > AEMと互換性のある標準バージョンをCloud Serviceとしてダウンロードし、`classic`バージョンでは&#x200B;**なく**&#x200B;してください。

1. **AEM開始**&#x200B;メニューから&#x200B;**ツール**/**展開**/**パッケージ**&#x200B;に移動します。

   ![「パッケージ」に移動します。](assets/setup/navigate-to-packages.png)

1. 「**パッケージをアップロード**」をクリックし、前の手順でダウンロードしたWKNDパッケージを選択します。 **「**&#x200B;をインストール」をクリックして、パッケージをインストールします。

1. **AEM開始**&#x200B;メニューで、**アセット** > **ファイル**&#x200B;に移動します。
1. フォルダーをクリックして、**WKNDサイト** > **英語** > **アドベンチャー**&#x200B;に移動します。

   ![Adventuresのフォルダ表示](assets/setup/folder-view-adventures.png)

   これは、WKNDブランドによって推進される様々なアドベンチャーを構成するすべてのアセットのフォルダーです。 例えば、画像やビデオなどの従来のメディアタイプと、**コンテンツフラグメント**&#x200B;のようなAEM専用のメディアが含まれます。

1. **Downhill Skiing Wyoming**&#x200B;フォルダーをクリックし、**Downhill Skiing Wyoming Content Fragment**&#x200B;カードをクリックします。

   ![スキーの内容をフラグメントカードでダウンロード](assets/setup/downhill-skiing-cntent-fragment.png)

1. コンテンツフラグメントエディターのUIが開き、ダウンヒルスキーワイオミングのアドベンチャー向けに開きます。

   ![下り坂スキーの内容の断片](assets/setup/down-hillskiing-fragment.png)

   **タイトル**、**説明**、**アクティビティ**&#x200B;のような様々なフィールドがフラグメントを定義していることに注意してください。

   **コンテンツ** フラグメントは、AEMでコンテンツを管理する方法の1つです。コンテンツフラグメントは、テキスト、リッチテキスト、日付、他のコンテンツフラグメントへの参照などの構造化されたデータ要素で構成される、再利用可能でプレゼンテーションにとらわれないコンテンツです。 コンテンツフラグメントについては、このチュートリアルの後半で詳しく説明します。

1. 「**キャンセル**」をクリックして、フラグメントを閉じます。 その他のフォルダーの一部に自由に移動して、その他のアドベンチャーコンテンツを調べます。

>[!NOTE]
>
> Cloud Service環境を使用している場合は、WKNDリファレンスサイトのようなコードベースをCloud Service環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=ja#deploying)に[導入する方法に関するドキュメントを参照してください。

## GraphQLエンドポイントのインストール{#graphql-endpoint}

GraphQLエンドポイントを設定する必要があります。 これにより、GraphQL APIが公開される正確なエンドポイントを柔軟に決定できます。 [CORS](#cors-config)は、外部アプリケーションへのアクセスを許可するためにも必要です。 パッケージが事前に作成されている場合のチュートリアルを迅速に実行できます。

1. [aem-guides-wknd-graphql.all-1.0.0-SNAPSHOT.zip](./assets/setup/aem-guides-wknd-graphql.all-1.0.0-SNAPSHOT.zip)パッケージをダウンロードします。
1. **AEM開始**&#x200B;メニューから&#x200B;**ツール**/**展開**/**パッケージ**&#x200B;に移動します。
1. 「パッケージ&#x200B;**アップロード**」をクリックし、前の手順でダウンロードしたパッケージを選択します。 **「**&#x200B;をインストール」をクリックして、パッケージをインストールします。

上記のパッケージには、後の章で使用する[GraphicQLツール](https://github.com/graphql/graphiql)も含まれています。 CORS設定に関する詳細は、[](#cors-config)の下にあります。

## サンプルアプリ{#sample-app}をインストール

このチュートリアルの目標の1つは、GraphQL APIを使用して、外部アプリケーションからAEMコンテンツを使用する方法を示すことです。 このチュートリアルでは、部分的に完成したReact Appの例を使用して、このチュートリアルをすばやく実行します。 iOS、Android、その他のプラットフォームで作成したアプリにも、同じレッスンと概念が適用されます。 Reactアプリは、不必要な複雑さを避けるため、意図的に単純です。これは、リファレンス実装ではありません。

1. 新しいターミナルウィンドウを開き、Gitを使用してチュートリアルのスターターブランチをコピーします。

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 任意のIDEで、`aem-guides-wknd-graphql/react-app/.env.development`にある`.env.development`ファイルを開きます。 `REACT_APP_AUTHORIZATION`行のコメントを解除し、ファイルが次のようになるようにします。

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/endpoint.gql
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   `React_APP_HOST_URI`がローカルのAEMインスタンスと一致することを確認します。 この章では、React AppをAEM **作成者**&#x200B;環境に直接接続します。したがって、認証が必要になります。 これは、AEM環境ーにすばやく変更を加え、アプリに即座に反映されるので、開発時の一般的な方法です。

   >[!NOTE]
   >
   > 実稼働環境では、アプリはAEM **発行**&#x200B;環境に接続します。 これについては、チュートリアルの後半で詳しく説明します。

1. `aem-guides-wknd-graphql/react-app`フォルダーに移動します。 アプリのインストールと開始:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 新しいブラウザーウィンドウで、[http://localhost:3000](http://localhost:3000)にあるアプリが自動的に起動します。

   ![React starterアプリ](assets/setup/react-starter-app.png)

   AEMの現在のアドベンチャーコンテンツのリストを表示する必要があります。

1. アドベンチャー画像の1つをクリックして、アドベンチャーの詳細を表示します。 アドベンチャーの詳細をAEMに返すように要求。

   ![アドベンチャー詳細表示](assets/setup/adventure-details-view.png)

1. ブラウザーの開発者ツールを使用して、**ネットワーク**&#x200B;リクエストを調べます。 **XHR**&#x200B;リクエストを表示し、`/content/graphql/endpoint.gql`に対する複数のPOSTリクエストを観察します。これは、AEM用に設定されたGraphQLエンドポイントです。

   ![GraphQL Endpoint XHR要求](assets/setup/endpoint-gql.png)

1. また、ネットワーク要求を検査することで、パラメーターとJSON応答を表示することもできます。 クエリと応答をより深く理解するために、Chrome用の[GraphQL Network](https://chrome.google.com/webstore/detail/graphql-network/igbmhmnkobkjalekgiehijefpkdemocm)のようなブラウザー拡張機能をインストールすると便利です。

   ![GraphQL Network Extension](assets/setup/GraphQL-extension.png)

   *Chrome拡張機能GraphQLネットワークの使用*

## コンテンツフラグメントの変更

Reactアプリが実行されたら、AEMのコンテンツを更新し、変更がアプリに反映されていることを確認します。

1. AEM [http://localhost:4502](http://localhost:4502)に移動します。
1. **アセット**/**ファイル**/**WKNDサイト**/**英語**/**冒険**/**[バリ>サーフキャンプ](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)&lt;a1に移動します。2/>.**

   ![バリサーフキャンプフォルダ](assets/setup/bali-surf-camp-folder.png)

1. **Bali Surf Camp**&#x200B;コンテンツフラグメント内をクリックして、コンテンツフラグメントエディターを開きます。
1. アドベンチャーの&#x200B;**タイトル**&#x200B;と&#x200B;**説明**&#x200B;を変更します

   ![コンテンツフラグメントの変更](assets/setup/modify-content-fragment-bali.png)

1. 「**保存**」をクリックして、変更を保存します。
1. [http://localhost:3000](http://localhost:3000)にあるReactアプリに戻り、更新して変更を確認します。

   ![更新版バリサーフキャンプアドベンチャー](assets/setup/overnight-bali-surf-camp-changes.png)

## バリデーターが{#congratulations}

GraphQLを使用して、AEMコンテンツを消費する外部アプリケーションが提供されました。 Reactアプリ内のコードを自由に調べて、既存のコンテンツフラグメントの変更を試し続けます。

## 次の手順 {#next-steps}

次の章「[コンテンツフラグメントモデルの定義](content-fragment-models.md)」では、コンテンツをモデル化し、**コンテンツフラグメントモデル**&#x200B;を使用してスキーマを構築する方法を説明します。 既存のモデルをレビューし、新しいモデルを作成します。 また、スキーマをモデルの一部として定義するために使用できる様々なデータタイプについても学習します。

## （賞与）CORSの設定{#cors-config}

AEMは、デフォルトでセキュリティ保護されているので、接触チャネル間のリクエストをブロックし、許可されていないアプリケーションがコンテンツに接続して表示するのを防ぎます。

このチュートリアルのReactアプリがAEM GraphQL APIエンドポイントとやり取りできるように、GraphQLエンドポイントパッケージで接触チャネル間のリソース共有の設定が定義されています。

![接触チャネル間のリソース共有の設定](./assets/setup/cross-origin-resource-sharing-configuration.png)

手動で設定するには：

1. **ツール**/**操作**/**Webコンソール**&#x200B;にあるAEM SDKのWebコンソールに移動します。
1. **AdobeGranite Cross-接触チャネルリソース共有ポリシー**&#x200B;のラベルの付いた行をクリックして、新しい構成を作成します
1. 他のフィールドはデフォルト値のままにして、次のフィールドを更新します。
   * 許可されている接触チャネル:`localhost:3000`
   * 許可されている接触チャネル(Regex):`.* `
   * 許可されているパス: `/content/graphql/endpoint.gql`
   * 許可されているメソッド：`GET`、`HEAD`、`POST`
      * GraphQLでは`POST`のみが必要ですが、AEMとヘッドレスに対話する場合は他の方法が役に立ちます。
   * 秘密鍵証明書のサポート：`Yes`
      * これは、React AppがAEM Authorサービス上の保護されたGraphQLエンドポイントと通信する際に必要となります。
1. 「**保存**」をクリックします。

この設定では、`localhost:3000`からAEM Authorサービスに対する`POST` HTTP要求をパス`/content/graphql/endpoint.gql`で許可します。

この設定とGraphQLエンドポイントは、AEMプロジェクトから生成されます。 [詳細はこちら](https://github.com/adobe/aem-guides-wknd-graphql/tree/master/aem-project)。
