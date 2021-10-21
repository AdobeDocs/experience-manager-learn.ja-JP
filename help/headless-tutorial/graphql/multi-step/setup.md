---
title: クイックセットアップ — AEMヘッドレスの概要 — GraphQL
description: Adobe Experience Manager(AEM) と GraphQL の概要。 AEM SDK をインストールし、サンプルコンテンツを追加して、GraphQL API を使用してAEMのコンテンツを使用するアプリケーションをデプロイします。 AEMがオムニチャネルエクスペリエンスを強化する方法を参照してください。
version: Cloud Service
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
source-git-commit: 9c1649247c65a1fa777b7574d1ab6ab49d0f722b
workflow-type: tm+mt
source-wordcount: '1814'
ht-degree: 3%

---

# クイックセットアップ {#setup}

この章では、ローカル環境の簡単なセットアップを提供し、外部アプリケーションがAEM GraphQL API を使用してAEMのコンテンツを使用することを確認します。 このチュートリアルの後半の章は、この設定から構築されます。

## 前提条件 {#prerequisites}

次のツールは、ローカルにインストールする必要があります。

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+JDK%7E+1%E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10 以降](https://nodejs.org/ja/)
* [npm 6 以降](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## 目的 {#objectives}

1. AEM SDK をダウンロードしてインストールします。
1. WKND リファレンスサイトからサンプルコンテンツをダウンロードしてインストールします。
1. GraphQL API を使用してコンテンツを使用するサンプルアプリをダウンロードしてインストールします。

## AEM SDK のインストール {#aem-sdk}

このチュートリアルでは、 [AEMas a Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#aem-as-a-cloud-service-sdk) を参照してください。 この節では、AEM SDK をインストールし、オーサーモードで実行するクイックガイドを示します。 環境環境の設定に関する詳細なガイドをローカル開発します。 [こちらにあります。](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja#local-development-environment-set-up).

>[!NOTE]
>
> このチュートリアルに従って、AEM as a Cloud Service環境を使用することもできます。 クラウド環境の使用に関する追加の注意事項は、このチュートリアル全体に記載されています。

1. に移動します。 **[ソフトウェア配布ポータル](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEMas a Cloud Service** および最新バージョンの **AEM SDK**.

   ![ソフトウェア配布ポータル](assets/setup/software-distribution-portal-download.png)

   >[!CAUTION]
   >
   > GraphQL 機能は、デフォルトで2021-02-04以降のAEM SDK でのみ有効です。

1. ダウンロードを解凍し、クイックスタート jar(`aem-sdk-quickstart-XXX.jar`) を専用のフォルダーにコピーする場合、 `~/aem-sdk/author`.
1. jar ファイルの名前をに変更します。 `aem-author-p4502.jar`.

   10. `author` name は、クイックスタート jar がオーサーモードで起動することを指定します。 10. `p4502` は、Quickstart サーバーがポート 4502 で実行されることを指定します。

1. 新しいターミナルウィンドウを開き、 jar ファイルを含むフォルダーに移動します。 次のコマンドを実行して、AEMインスタンスをインストールして起動します。

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. 管理者パスワードを `admin`. admin パスワードはどれでも問題ありませんが、再設定の必要性を減らすために、ローカル開発のデフォルトを使用することをお勧めします。
1. 数分後にAEMインスタンスのインストールが完了し、新しいブラウザーウィンドウが [http://localhost:4502](http://localhost:4502).
1. ユーザー名でログイン `admin` とパスワード `admin`.

## サンプルコンテンツと GraphQL エンドポイントのインストール {#wknd-site-content-endpoints}

のサンプルコンテンツ **WKND リファレンスサイト** チュートリアルを高速化するためにインストールされます。 WKND は架空のライフスタイルブランドで、多くの場合、AEMトレーニングと組み合わせて使用されます。

WKND リファレンスサイトには、 [GraphQL エンドポイント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=en#graphql-aem-endpoint). 実際の実装では、ドキュメントに記載された手順に従って、 [GraphQL エンドポイントを含める](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=en#graphql-aem-endpoint) （顧客プロジェクト内） A [CORS](#cors-config) は、WKND サイトの一部としてパッケージ化されています。 外部アプリケーションへのアクセスを許可するには、CORS 設定が必要です。詳しくは、 [CORS](#cors-config) 以下に示します。

1. WKND サイト用に最新のコンパイル済みAEMパッケージをダウンロードします。 [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > AEM as a Cloud Serviceおよび **not** ザ `classic` バージョン。

1. から **AEM Start** メニュー移動 **ツール** > **導入** > **パッケージ**.

   ![パッケージに移動します。](assets/setup/navigate-to-packages.png)

1. クリック **パッケージのアップロード** 前の手順でダウンロードした WKND パッケージを選択します。 クリック **インストール** をクリックしてパッケージをインストールします。

1. から **AEM Start** メニュー移動 **Assets** > **ファイル**.
1. フォルダーをクリックして移動する **WKND サイト** > **英語** > **冒険**.

   ![Adventures のフォルダービュー](assets/setup/folder-view-adventures.png)

   これは、WKND ブランドが推進する様々なアドベンチャーを構成するすべてのアセットのフォルダーです。 これには、画像やビデオなどの従来のメディアタイプや、AEMに特有のメディアも含まれます **コンテンツフラグメント**.

1. をクリックして、 **ダウンヒルスキーワイオミング** フォルダーを開き、 **Downhill Skiing Wyoming コンテンツフラグメント** カード：

   ![Downwhill Skiing コンテンツフラグメントカード](assets/setup/downhill-skiing-cntent-fragment.png)

1. コンテンツフラグメントエディターの UI が開き、Downhill Skiing Wyoming のアドベンチャーに向けて開きます。

   ![Downhill Skiing コンテンツフラグメント](assets/setup/down-hillskiing-fragment.png)

   ～のような様々な分野を観察する **タイトル**, **説明**&#x200B;および **アクティビティ** フラグメントを定義します。

   **コンテンツフラグメント** は、AEMでコンテンツを管理する方法の 1 つです。 コンテンツフラグメントは、テキスト、リッチテキスト、日付、他のコンテンツフラグメントへの参照など、構造化されたデータ要素で構成され、再利用可能で、プレゼンテーションに依存しないコンテンツです。 コンテンツフラグメントについては、このチュートリアルの後半で詳しく説明します。

1. クリック **キャンセル** をクリックしてフラグメントを閉じます。 他のフォルダーの一部に移動し、他のアドベンチャーコンテンツを参照してください。

>[!NOTE]
>
> Cloud Service環境を使用する場合は、 [WKND リファレンスサイトなどのコードベースをCloud Service環境にデプロイする](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=en#coding-against-the-right-aem-version).

## サンプルアプリケーションのインストール{#sample-app}

このチュートリアルの目標の 1 つは、GraphQL API を使用して外部アプリケーションからAEMコンテンツを使用する方法を示すことです。 このチュートリアルでは、チュートリアルを高速化するために部分的に完了した React App の例を使用します。 同じレッスンと概念が、iOS、Android またはその他のプラットフォームで作成されたアプリに適用されます。 React アプリは、不必要な複雑さを避けるために、意図的にシンプルです。これは、参照実装ではありません。

1. 新しいターミナルウィンドウを開き、Git を使用して Tutorial Starter ブランチを複製します。

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 任意の IDE で、ファイルを開きます。 `.env.development` at `aem-guides-wknd-graphql/react-app/.env.development`. を `REACT_APP_AUTHORIZATION` 行がコメント化解除され、ファイルは次のようになります。

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   以下を確認します。 `React_APP_HOST_URI` は、ローカルのAEMインスタンスに一致します。 この章では、React アプリをAEMに直接接続します **作成者** 環境。 **作成者** 環境では、デフォルトで認証が必要です。そのため、アプリは `admin` ユーザー。 これは、AEM環境にすばやく変更を加え、その変更がアプリにすぐに反映されるのを確認できるので、開発時には一般的な方法です。

   >[!NOTE]
   >
   > 実稼動シナリオでは、アプリはAEMに接続します **公開** 環境。 詳しくは、 [実稼動のデプロイメント](production-deployment.md) チャプター

1. に移動します。 `aem-guides-wknd-graphql/react-app` フォルダー。 デスクトップアプリケーションのインストールと起動：

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 新しいブラウザーウィンドウで、アプリが [http://localhost:3000](http://localhost:3000).

   ![React スターターアプリ](assets/setup/react-starter-app.png)

   AEMの現在のアドベンチャーコンテンツのリストが表示されます。

1. アドベンチャー画像の 1 つをクリックして、アドベンチャーの詳細を表示します。 アドベンチャーの詳細を返すようAEMにリクエストが送信されます。

   ![アドベンチャーの詳細ビュー](assets/setup/adventure-details-view.png)

1. ブラウザーの開発者ツールを使用して、 **ネットワーク** リクエスト。 表示 **XHR** を要求し、 `/content/graphql/global/endpoint.json`:AEM用に設定された GraphQL エンドポイント。

   ![GraphQL エンドポイント XHR リクエスト](assets/setup/endpoint-gql.png)

1. また、ネットワークリクエストを調べて、パラメーターと JSON 応答を表示することもできます。 これは、 [GraphQL ネットワークインスペクター](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln) Chrome のクエリと応答をより深く理解するために使用します。

## コンテンツフラグメントの変更

React アプリが実行されたら、AEMのコンテンツを更新し、変更がアプリに反映されていることを確認します。

1. AEMに移動 [http://localhost:4502](http://localhost:4502).
1. に移動します。 **Assets** > **ファイル** > **WKND サイト** > **英語** > **冒険** > **[バリサーフキャンプ](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**.

   ![バリサーフキャンプフォルダー](assets/setup/bali-surf-camp-folder.png)

1. をクリックして、 **バリサーフキャンプ** コンテンツフラグメントを開いて、コンテンツフラグメントエディターを開きます。
1. の **タイトル** そして **説明** 冒険の

   ![コンテンツフラグメントの変更](assets/setup/modify-content-fragment-bali.png)

1. 「**保存**」をクリックして、変更を保存します。
1. React アプリ ( ) に戻ります。 [http://localhost:3000](http://localhost:3000) をクリックし、変更を確認します。

   ![アップデートバリサーフキャンプアドベンチャー](assets/setup/overnight-bali-surf-camp-changes.png)

## GraphiQL ツールのインストール {#install-graphiql}

[GraphiQL](https://github.com/graphql/graphiql) は開発ツールで、開発インスタンスやローカルインスタンスなど、下位レベルの環境でのみ必要です。 GraphiQL IDE を使用すると、返されるクエリとデータをすばやくテストし、調整できます。 また、GraphiQL はドキュメントに簡単にアクセスでき、利用可能な方法を簡単に学習し、理解できます。

1. に移動します。 **[ソフトウェア配布ポータル](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEMas a Cloud Service**.
1. 「GraphiQL」を検索します ( 必ず **i** in **GraphiQL**.
1. 最新の **GraphiQL コンテンツパッケージ v.x.x.x**

   ![GraphiQL パッケージのダウンロード](assets/explore-graphql-api/software-distribution.png)

   zip ファイルは、直接インストールできるAEMパッケージです。

1. から **AEM Start** メニュー移動 **ツール** > **導入** > **パッケージ**.
1. クリック **パッケージのアップロード** 前の手順でダウンロードしたパッケージを選択します。 クリック **インストール** をクリックしてパッケージをインストールします。

   ![GraphiQL パッケージのインストール](assets/explore-graphql-api/install-graphiql-package.png)
1. GraphiQL IDE( ) に移動します。 [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html) GraphQL API について学びましょう。

   >[!NOTE]
   >
   > GraphQL ツールおよび GraphQL API は、 [チュートリアルの後半で詳しく調べる](./explore-graphql-api.md).

## おめでとうございます。 {#congratulations}

これで、GraphQL を使用したAEMコンテンツを使用する外部アプリケーションが作成されました。 React アプリのコードを調べ、既存のコンテンツフラグメントの変更を試してみてください。

## 次の手順 {#next-steps}

次の章では、 [コンテンツフラグメントモデルの定義](content-fragment-models.md)を使用して、コンテンツをモデル化し、スキーマを構築する方法を学びます。 **コンテンツフラグメントモデル**. 既存のモデルを確認し、新しいモデルを作成します。 また、モデルの一部としてスキーマを定義するために使用できる様々なデータ型についても学習します。

## （ボーナス）CORS の設定 {#cors-config}

AEMはデフォルトでセキュリティで保護されているので、クロスオリジンリクエストをブロックし、権限のないアプリケーションがそのコンテンツに接続して表示するのを防ぎます。

このチュートリアルの React アプリがAEM GraphQL API エンドポイントとやり取りできるように、 WKND サイトのリファレンスプロジェクトでクロスオリジンリソース共有の設定が定義されています。

![クロスオリジンリソース共有設定](assets/setup/cross-origin-resource-sharing-configuration.png)

デプロイ済みの設定を表示するには：

1. AEM SDK の Web コンソール ( ) に移動します。 [http://localhost:4502/system/console](http://localhost:4502/system/console).

   >[!NOTE]
   >
   > Web コンソールは、SDK でのみ使用できます。 AEMas a Cloud Service環境では、この情報は [開発者コンソール](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html).

1. 上部のメニューで、 **OSGI** > **設定** 全てを挙げる [OSGi 設定](http://localhost:4502/system/console/configMgr).
1. ページを下にスクロール **AdobeGranite のクロスオリジンリソース共有**.
1. の設定をクリックします。 `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.
1. 次のフィールドが更新されました。
   * 許可されたオリジン（正規表現）: `http://localhost:.*`
      * すべてのローカルホスト接続を許可します。
   * 許可されているパス: `/content/graphql/global/endpoint.json`
      * これは、現在設定されている唯一の GraphQL エンドポイントです。 ベストプラクティスとして、COR の設定はできるだけ制限する必要があります。
   * 許可されるメソッド： `GET`, `HEAD`, `POST`
      * のみ `POST` は GraphQL に必要ですが、ヘッドレスでAEMを操作する場合は他のメソッドが役立ちます。
   * サポートされるヘッダー： **認証** は、オーサー環境での基本認証を渡すために追加されました。
   * 資格情報をサポート： `Yes`
      * React アプリが AEM オーサーサービス上の保護された GraphQL エンドポイントと通信するので、この処理が必要です。

この設定と GraphQL エンドポイントは、AEM WKND プロジェクトの一部です。 すべての [OSGi 設定はここ](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).
