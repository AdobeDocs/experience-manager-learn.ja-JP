---
title: Content MCP Server を使用したAEM コンテンツ処理の高速化
description: AI を利用した Cursor などの好みの IDE からAEM Content MCP Server を使用して、AEM コンテンツ操作を合理化および高速化し、手作業を減らし、生産性を高める方法を説明します。
version: Experience Manager as a Cloud Service
role: Leader, User, Developer
level: Beginner
doc-type: tutorial
duration: null
last-substantial-update: 2026-03-04T00:00:00Z
jira: KT-20474
source-git-commit: c5f1c7f57181b1e9de6dd91aa2428f2fe1a04893
workflow-type: tm+mt
source-wordcount: '850'
ht-degree: 0%

---


# Content MCP Server を使用したAEM コンテンツ処理の高速化

**Cursor IDE** などの AI を活用した IDE から [Content MCP Server](https://www.cursor.com/) を使用して、低レベルの API コードや UI ナビゲーションなしで、自然言語でのAEM コンテンツを操作できます。

このチュートリアルでは、MCP フローを離れることなく、すべて IDE から _低いAEM環境_ （RDE または開発）に対して _アドベンチャーコンテンツフラグメントの詳細_ レビュー _、_ 更新 [&#x200B; フラグメント（アドベンチャーの価格など）および &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)WKND Adventures React アプリ _の変更を検証_ 検証）します。

## 概要

AEM as a Cloud Serviceには _MCP サーバー_ が用意されているので、IDE やチャットアプリをAEMと安全に連携させることができます。 **Content MCP サーバー** は、ページ、フラグメント、アセットをサポートします。 詳しくは、[AEMの MCP サーバー &#x200B;](./overview.md) を参照してください。

## 開発者による使用方法

[Cursor IDE](https://www.cursor.com/) を Content MCP サーバーに接続し、以下のシナリオを実行します。

### 設定 – カーソルのある Content MCP サーバー

次の手順で、Cursor で Content MCP サーバを設定します。

1. お使いのマシン上でカーソルを開きます。

1. カーソルメニューから **設定**/**カーソル設定** に移動し、設定ウィンドウを開きます。
   ![&#x200B; カーソル設定 &#x200B;](../assets/content-mcp-server/cursor-settings.png)

1. 左側のサイドバーで **ツールと MCP** をクリックして、そのパネルを開きます。
   ![&#x200B; ツールと MCP](../assets/content-mcp-server/tools-mcp.png)

1. **Add Custom MCP** または **New MCP Server** をクリックして開き、次の設定に貼り付け `mcp.json` す。

   ```json
   {
       "mcpServers": {
           // Use this for create, read, update, and delete operations
           "AEM-RDE-Content": {
               "url": "https://mcp.adobeaemcloud.com/adobe/mcp/content"
           },
           //Use this for read-only operations
           "AEM-RDE-Content-Read-Only": {
               "url": "https://mcp.adobeaemcloud.com/adobe/mcp/content-readonly"
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > チュートリアルのために、上記の設定ではこのチュートリアルに **コンテンツ** と **コンテンツ（読み取り専用）** の両方を追加しています。 実際には、**コンテンツ** には、すべての **コンテンツ（読み取り専用）** オファーに加え、作成/更新/削除ツールが既に含まれています。
   >
   >
   > コンテンツの作成、変更または削除の可能性を回避する場合は、**コンテンツ（読み取り専用）** （`/content-readonly`）のみを設定し、**コンテンツ** （`/content`）は省略します。 こうすることで、誤って変更することを回避できます。

   ![AEM MCP サーバーの追加 &#x200B;](../assets/content-mcp-server/mcp-json-file.png)

1. Cursor Settings ウィンドウで、**Connect** をクリックして認証プロセスを開始します。 OAuth 2.0 PKCE フローを使用して、AEM MCP サーバーにアクセスするための **ユーザー固有のアクセストークン** を取得します。
   ![AEM MCP サーバーへの接続 &#x200B;](../assets/content-mcp-server/connect-to-aem-mcp-server.png)

1. Adobe IDでログインし、Cursor Settings ウィンドウに戻ります。
   ![Adobe IDでログイン &#x200B;](../assets/content-mcp-server/login-with-adobe-id.png)

1. **AEM-RDE-Content-Read-Only&rbrace; および** 2&rbrace;AEM-RDE-Content&rbrace; が接続済みとして表示され **ことを確認します。**&#x200B;各サーバーを展開して、そのツールを表示できます。

   ![AEM MCP サーバー &#x200B;](../assets/content-mcp-server/connected-aem-mcp-servers.png)

### 設定 – WKND アドベンチャー React アプリ

次に、カーソルで [WKND Adventures React アプリ &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app) を設定します。

1. お使いのマシン上で次の 2 つのリポジトリのクローンを作成します。

   ```bash
   ## WKND GraphQL repo, the `react-app` folder is the WKND Adventures app
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   
   ## WKND Site repo, you deploy this to RDE so the app can use its content fragments data via GraphQL
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. [WKND Site](https://github.com/adobe/aem-guides-wknd) プロジェクトを RDE にデプロイします。 詳しい手順については、[&#x200B; 迅速な開発環境の使用方法 &#x200B;](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use#deploy-aem-artifacts-using-the-aem-rde-plugin) を参照してください。

1. IDE で `react-app` フォルダーを開きます。

1. `.env.development` を編集して設定します。
   - `REACT_APP_HOST_URI`:RDE オーサー URL
   - `REACT_APP_AUTH_METHOD`: `basic`
   - `REACT_APP_BASIC_AUTH_USER` および `REACT_APP_AEM_AUTH_PASSWORD`:`aem-headless` にする（このユーザーを RDE で作成して `administrators` グループに追加する）

1. IDE ターミナルから、次を実行します。

   ```bash
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. ブラウザーで、[http://localhost:3000](http://localhost:3000) に移動して、WKND Adventures アプリを表示します。

   ![React アプリ - WKND アドベンチャー &#x200B;](../assets/content-mcp-server/react-app-wknd-adventures.png)

### 生産性向上シナリオ - AEMのコンテンツのレビューと更新

シンプルなルールが満たされた際に、アドベンチャーカードに _HOT DEAL_ バナーを表示する必要があるとします。 通常のアプローチは次のとおりです。

- Adventure cards コンポーネントコードを確認します
- バナーを表示するタイミングのロジックを追加します
- AEMの Adventure コンテンツフラグメントモデルを確認します
- 1 つ以上の Adventure フラグメントプロパティを変更して、ルールをテストします

シンプルにするために、アドベンチャーの価格が 100 ドル未満の場合に _HOT DEAL_ バナーを表示してみましょう。

React アプリは RDE 環境からデータを取得するので、Adventure コンテンツフラグメントモデルを理解してから、適切なフラグメントプロパティを更新する必要があります。 AEM Content MCP Server は、まさにこれに役立ちます。 方法は次のとおりです。

1. カーソルで新しいチャットを開き、次のように入力します。

   ```text
   I want to review my Content Fragment Models from AEM RDE, can you list the Adventure Content Fragment details.
   ```

   ![&#x200B; コンテンツフラグメントモデルを確認 &#x200B;](../assets/content-mcp-server/review-content-fragment-models-prompt-response.png)


   Content MCP サーバーを呼び出す前に、続行を確認するメッセージが表示されます。 したがって、コンテンツ操作を制御し続けることができます。

   AI は、Content MCP サーバーを使用してデータを取得し、明確かつ構造化された方法で表示します。 これには、コンテンツフラグメントモデルの詳細、フラグメント数および概要情報が含まれます。

1. _HOT DEAL_ バナーをトリガーにするには、1 つのアドベンチャーの価格を更新します。 同じチャットで、次の操作を試します。

   ```text
   Can you update adventure Beervana in Portland's price to 99.99
   ```

   ![&#x200B; アドベンチャー価格を更新 &#x200B;](../assets/content-mcp-server/update-adventure-price-prompt-response.png)

   同様に、AI は、コンテンツを更新する前に続行することを確認するように求めます。 また、更新前と更新後のコンテンツ操作の概要も説明します。

1. React アプリで、Beervana カードに _HOT DEAL_ バナーが表示されていることを確認します。

   ![&#x200B; ホット・ディール・バナーの確認 &#x200B;](../assets/content-mcp-server/verify-hot-deal-banner.png)

### 追加のプロンプト

（Content MCP サーバーが接続された状態で） IDE でコンテンツに焦点を当てたプロンプトを試して、より多くのワークフローと機能を調べます。

- コンテンツを見つける：

  ```text
  List all content fragments in the WKND Adventures folder
  
  List all WKND Site pages from US English site
  
  Can you give me page metadata for Tahoe Skiing English page? 
  
  List assets of Bali Surf camp
  
  What Content Fragment models are available in this environment?
  ```

- コンテンツの検索：

  ```text
  Search for content fragments that mention 'cycling'
  
  Do we have a magazine page in US English site with "Camping" in it
  ```

- コンテンツを更新：

  ```text
  In WKND US English create a copy of Downhill Skiing Wyoming as "Test Downhill Skiing Wyoming"
  
  In newly created "Test Downhill Skiing Wyoming" please change title to "Duplicated Page"
  ```

- 公開または非公開：

  ```text
  Can you publish the page at /us/en/adventures/test-downhill-skiing-wyoming and give me publish page URL
  
  Can you unpublish the test-downhill-skiing-wyoming page
  ```

## 概要

AEM Content MCP Server を Cursor で設定し、RDE （または開発）環境に接続しました。 次に、WKND Adventures React アプリを使用し、自然言語でチャットして、アドベンチャーコンテンツフラグメントの詳細を確認しました。 また、各コンテンツ操作の前に、フラグメントの価格を更新し、AI が確認を求めるようにしました。 実行中のアプリで変更を確認しました。 IDE から同じ人間中心のフローを使用して、AEM UI に切り替えたり、低レベルの API コードを記述したりすることなく、AEM コンテンツをレビュー、更新および作成できます。
