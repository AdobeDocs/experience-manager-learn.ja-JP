---
title: クイックセットアップ — AEMヘッドレスの概要 — GraphQL
description: Adobe Experience Manager(AEM) と GraphQL の基本を学びます。 AEM SDK をインストールし、サンプルコンテンツを追加し、AEMの GraphQL API を使用して、コンテンツを使用するアプリケーションをデプロイします。 AEMがオムニチャネルエクスペリエンスを強化する方法を参照してください。
version: Cloud Service
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
source-git-commit: 846400cd3ac4eb1b04ece055dfcbbd677f11e88e
workflow-type: tm+mt
source-wordcount: '1819'
ht-degree: 3%

---

# クイックセットアップ {#setup}

この章では、ローカル環境の簡単なセットアップを提供し、外部アプリケーションがAEM GraphQL API を使用してAEMからコンテンツを使用するのを確認します。 このチュートリアルの後の章は、この設定から構築されます。

## 前提条件 {#prerequisites}

以下のツールをローカルにインストールする必要があります。

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+JDK%7E+11%E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10 以降](https://nodejs.org/ja/)
* [npm 6 以降](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## 目的 {#objectives}

1. AEM SDK をダウンロードしてインストールします。
1. WKND リファレンスサイトからサンプルコンテンツをダウンロードしてインストールします。
1. GraphQL API を使用してコンテンツを使用するサンプルアプリをダウンロードしてインストールします。

## AEM SDK のインストール {#aem-sdk}

このチュートリアルでは、 [AEMas a Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#aem-as-a-cloud-service-sdk) AEM GraphQL API を調べる この節では、AEM SDK をインストールしてオーサーモードで実行する方法のクイックガイドを示します。 ローカル開発環境の設定に関する詳細なガイド [ここにあります](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja#local-development-environment-set-up).

>[!NOTE]
>
> また、このチュートリアルに従ってAEMas a Cloud Service環境を使用することもできます。 クラウド環境を使用する場合の追加の注意事項は、このチュートリアル全体に含まれています。

1. 次に移動： **[ソフトウェア配布ポータル](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEMas a Cloud Service** および最新バージョンの **AEM SDK**.

   ![ソフトウェア配布ポータル](assets/setup/software-distribution-portal-download.png)

   >[!CAUTION]
   >
   > GraphQL 機能は、デフォルトでは、2021-02-04以降のAEM SDK でのみ有効です。

1. ダウンロードを解凍し、クイックスタート jar(`aem-sdk-quickstart-XXX.jar`) を専用のフォルダーにコピーする場合は、 `~/aem-sdk/author`.
1. jar ファイルの名前をに変更します。 `aem-author-p4502.jar`.

   この `author` name は、クイックスタート jar がオーサーモードで起動することを指定します。 この `p4502` は、クイックスタートサーバーがポート 4502 で実行されることを指定します。

1. 新しいターミナルウィンドウを開き、jar ファイルを含むフォルダーに移動します。 次のコマンドを実行して、AEMインスタンスをインストールして起動します。

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. 管理者パスワードをとして入力 `admin`. 管理者パスワードは使用できますが、 `admin` ローカル開発を使用して、再設定の必要性を減らす。
1. 数分後にAEMインスタンスがインストールを完了し、新しいブラウザーウィンドウが次の場所で開きます： [http://localhost:4502](http://localhost:4502).
1. ユーザー名でログイン `admin` と、AEMの初回起動時に選択したパスワード ( 通常は `admin`) をクリックします。

## サンプルコンテンツと GraphQL エンドポイントのインストール {#wknd-site-content-endpoints}

のサンプルコンテンツ **WKND リファレンスサイト** をインストールして、チュートリアルを高速化します。 WKND は架空のライフスタイルブランドで、AEMトレーニングと組み合わせて使用することが多いです。

WKND リファレンスサイトには、 [GraphQL エンドポイント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=en#graphql-aem-endpoint). 実際の実装では、ドキュメントに記載された手順に従って、 [GraphQL エンドポイントを含める](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=en#graphql-aem-endpoint) を選択します。 A [CORS](#cors-config) は、WKND サイトの一部としてもパッケージ化されています。 外部アプリケーションへのアクセスを許可するには、CORS 設定が必要です。詳細は、 [CORS](#cors-config) は以下にあります。

1. WKND サイト用に最新のコンパイル済みAEMパッケージをダウンロードします。 [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > AEM as a Cloud Serviceおよび **not** の `classic` バージョン。

1. 次の **AEM Start** メニュー移動先 **ツール** > **導入** > **パッケージ**.

   ![パッケージに移動します。](assets/setup/navigate-to-packages.png)

1. クリック **パッケージをアップロード** 前の手順でダウンロードした WKND パッケージを選択します。 クリック **インストール** をクリックしてパッケージをインストールします。

1. 次の **AEM Start** メニュー移動先 **Assets** > **ファイル**.
1. フォルダーをクリックして移動先 **WKND サイト** > **英語** > **冒険**.

   ![Adventures のフォルダービュー](assets/setup/folder-view-adventures.png)

   これは、WKND ブランドによって促進される様々なアドベンチャーを構成するすべてのアセットのフォルダーです。 これには、画像やビデオなどの従来のメディアタイプと、AEMに特有のメディアが含まれます **コンテンツフラグメント**.

1. をクリックして、 **ダウンヒルスキーワイオミング** フォルダーに移動し、 **Downhill Skiing Wyoming コンテンツフラグメント** カード：

   ![Downwhill Skiing コンテンツフラグメントカード](assets/setup/downhill-skiing-cntent-fragment.png)

1. コンテンツフラグメントエディターの UI が開き、Downhill Skiing Wyoming のアドベンチャー向けになります。

   ![Downhill Skiing コンテンツフラグメント](assets/setup/down-hillskiing-fragment.png)

   次のような様々なフィールドを確認します。 **タイトル**, **説明**、および **アクティビティ** フラグメントを定義します。

   **コンテンツフラグメント** は、AEMでコンテンツを管理する方法の 1 つです。 コンテンツフラグメントは、テキスト、リッチテキスト、日付、他のコンテンツフラグメントへの参照などの構造化されたデータ要素で構成され、再利用可能で、プレゼンテーションに依存しないコンテンツです。 コンテンツフラグメントについては、このチュートリアルの後半で詳しく説明します。

1. クリック **キャンセル** をクリックしてフラグメントを閉じます。 他のフォルダーの一部に移動して、他のアドベンチャーコンテンツを参照できます。

>[!NOTE]
>
> Cloud Service環境を使用する場合は、 [WKND リファレンスサイトなどのコードベースをCloud Service環境にデプロイする](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=en#coding-against-the-right-aem-version).

## サンプルアプリケーションのインストール{#sample-app}

このチュートリアルの目的の 1 つは、GraphQL API を使用して外部アプリケーションからAEMコンテンツを使用する方法を示すことです。 このチュートリアルでは、チュートリアルを高速化するために部分的に完了した React App の例を使用します。 同じレッスンと概念が、iOS、Android またはその他のプラットフォームで作成されたアプリに適用されます。 React アプリは意図的にシンプルで、不要な複雑さを回避できます。これは、参照用実装ではありません。

1. 新しいターミナルウィンドウを開き、Git を使用してチュートリアルのスターターブランチを複製します。

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 任意の IDE で、ファイルを開きます。 `.env.development` 時刻 `aem-guides-wknd-graphql/react-app/.env.development`. を確認します。 `REACT_APP_AUTHORIZATION` 行のコメントが解除され、ファイルが次のようになります。

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   以下を確認します。 `React_APP_HOST_URI` はローカルのAEMインスタンスと一致します。 この章では、React アプリをAEMに直接接続します **作成者** 環境。 **作成者** デフォルトでは認証が必要なので、アプリはを `admin` ユーザー。 これは、AEM環境をすばやく変更し、アプリケーションに即座に反映されるので、開発時には一般的な方法です。

   >[!NOTE]
   >
   > 実稼動シナリオでは、アプリはAEMに接続します **公開** 環境。 詳しくは、 [実稼動のデプロイメント](production-deployment.md) チャプター。

1. 次に移動： `aem-guides-wknd-graphql/react-app` フォルダー。 デスクトップアプリケーションをインストールして起動します。

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 新しいブラウザーウィンドウで、アプリが [http://localhost:3000](http://localhost:3000).

   ![React スターターアプリ](assets/setup/react-starter-app.png)

   AEMの現在のアドベンチャーコンテンツのリストが表示されます。

1. アドベンチャー画像の 1 つをクリックして、アドベンチャーの詳細を表示します。 アドベンチャーの詳細を返すようにAEMにリクエストが送信されます。

   ![アドベンチャーの詳細ビュー](assets/setup/adventure-details-view.png)

1. ブラウザーの開発者ツールを使用して、 **ネットワーク** リクエスト。 次を表示： **XHR** をリクエストし、 `/content/graphql/global/endpoint.json`:AEM用に設定された GraphQL エンドポイント。

   ![GraphQL エンドポイント XHR リクエスト](assets/setup/endpoint-gql.png)

1. また、ネットワークリクエストを調べて、パラメーターと JSON 応答を表示することもできます。 ブラウザー拡張機能 ( [GraphQL ネットワークインスペクター](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln) Chrome でクエリと応答をより深く理解できるようにするため。

## コンテンツフラグメントの変更

これで React アプリが実行されたので、AEMのコンテンツを更新し、変更がアプリに反映されていることを確認します。

1. AEMに移動 [http://localhost:4502](http://localhost:4502).
1. に移動します。 **Assets** > **ファイル** > **WKND サイト** > **英語** > **冒険** > **[バリサーフキャンプ](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**.

   ![バリサーフキャンプフォルダ](assets/setup/bali-surf-camp-folder.png)

1. をクリックして、 **バリサーフキャンプ** コンテンツフラグメントを使用して、コンテンツフラグメントエディターを開きます。
1. を変更します。 **タイトル** そして **説明** 冒険の

   ![コンテンツフラグメントを変更](assets/setup/modify-content-fragment-bali.png)

1. 「**保存**」をクリックして、変更を保存します。
1. React アプリ ( ) に戻ります。 [http://localhost:3000](http://localhost:3000) を更新して、変更を確認します。

   ![アップデートバリサーフキャンプアドベンチャー](assets/setup/overnight-bali-surf-camp-changes.png)

## GraphiQL ツールのインストール {#install-graphiql}

[GraphiQL](https://github.com/graphql/graphiql) は開発ツールであり、開発インスタンスやローカルインスタンスなどの下位レベルの環境でのみ必要です。 GraphiQL IDE を使用すると、返されるクエリとデータをすばやくテストし、調整できます。 また、GraphiQL はドキュメントに簡単にアクセスでき、使用可能なメソッドを簡単に学習し、理解できます。

1. 次に移動： **[ソフトウェア配布ポータル](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEMas a Cloud Service**.
1. 「GraphiQL」を検索します ( 必ず **i** in **GraphiQL**.
1. 最新の **GraphiQL コンテンツパッケージ v.x.x.x**

   ![GraphiQL パッケージをダウンロード](assets/explore-graphql-api/software-distribution.png)

   zip ファイルは、直接インストールできるAEMパッケージです。

1. 次の **AEM Start** メニュー移動先 **ツール** > **導入** > **パッケージ**.
1. クリック **パッケージをアップロード** 前の手順でダウンロードしたパッケージを選択します。 クリック **インストール** をクリックしてパッケージをインストールします。

   ![GraphiQL パッケージのインストール](assets/explore-graphql-api/install-graphiql-package.png)
1. GraphiQL IDE( ) に移動します。 [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html) GraphQL API の詳細をご覧ください。

   >[!NOTE]
   >
   > GraphiQL ツールと GraphQL API は、 [チュートリアルの後半で詳しく説明します。](./explore-graphql-api.md).

## おめでとうございます。 {#congratulations}

これで、GraphQL でAEMコンテンツを使用する外部アプリケーションが作成されました。 React アプリのコードを調べ、既存のコンテンツフラグメントの変更を引き続き試してみてください。

## 次の手順 {#next-steps}

次の章では、 [コンテンツフラグメントモデルの定義](content-fragment-models.md)を使用して、コンテンツをモデル化し、スキーマを構築する方法を学びます。 **コンテンツフラグメントモデル**. 既存のモデルを確認し、新しいモデルを作成します。 また、モデルの一部としてスキーマを定義するために使用できる様々なデータタイプについても学習します。

## （ボーナス）CORS 設定 {#cors-config}

AEMは、デフォルトでセキュリティで保護されているので、クロスオリジンリクエストをブロックし、権限のないアプリケーションがそのコンテンツに接続して表示するのを防ぎます。

このチュートリアルの React アプリがAEM GraphQL API エンドポイントとやり取りできるように、WKND サイト参照プロジェクトでクロスオリジンリソース共有設定が定義されています。

![クロスオリジンリソース共有設定](assets/setup/cross-origin-resource-sharing-configuration.png)

デプロイされた設定を表示するには：

1. AEM SDK の Web コンソール ( ) に移動します。 [http://localhost:4502/system/console](http://localhost:4502/system/console).

   >[!NOTE]
   >
   > Web コンソールは、SDK でのみ使用できます。 AEMas a Cloud Service環境では、この情報は [開発者コンソール](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html).

1. 上部のメニューで、 **OSGI** > **設定** 全てを上げる [OSGi 設定](http://localhost:4502/system/console/configMgr).
1. ページを下にスクロールします。 **AdobeGranite クロスオリジンリソース共有**.
1. の設定をクリックします。 `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.
1. 次のフィールドが更新されました。
   * 許可されたオリジン（正規表現）: `http://localhost:.*`
      * すべてのローカルホスト接続を許可します。
   * 許可されているパス: `/content/graphql/global/endpoint.json`
      * これは、現在設定されている唯一の GraphQL エンドポイントです。 ベストプラクティスとして、COR の設定はできるだけ制限する必要があります。
   * 許可されるメソッド： `GET`, `HEAD`, `POST`
      * のみ `POST` は GraphQL に必要ですが、ヘッドレスな方法でAEMを操作する場合は、他のメソッドが役立ちます。
   * サポートされるヘッダー： **認証** オーサー環境での基本認証を渡すためのが追加されました。
   * 認証情報をサポート： `Yes`
      * React アプリが AEM オーサーサービス上の保護された GraphQL エンドポイントと通信するので、これが必要です。

この設定と GraphQL エンドポイントは、AEM WKND プロジェクトの一部です。 すべての [OSGi 設定はここで](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).
