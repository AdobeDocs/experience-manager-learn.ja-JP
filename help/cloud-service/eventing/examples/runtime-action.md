---
title: Adobe I/O Runtime アクションと AEM イベント
description: Adobe I/O Runtime アクションを使用して AEM イベントを受け取り、イベントの詳細（ペイロード、ヘッダー、メタデータなど）を確認する方法を説明します。
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 457
last-substantial-update: 2024-01-29T00:00:00Z
jira: KT-14878
thumbnail: KT-14878.jpeg
exl-id: b1c127a8-24e7-4521-b535-60589a1391bf
source-git-commit: efa0a16649c41fab8309786a766483cfeab98867
workflow-type: ht
source-wordcount: '699'
ht-degree: 100%

---

# Adobe I/O Runtime アクションと AEM イベント

[Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) アクションを使用して AEM イベントを受け取り、イベントの詳細（ペイロード、ヘッダー、メタデータなど）を確認する方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3427053?quality=12&learn=on)

Adobe I/O Runtime は、Adobe I/O イベントに応答してコードを実行できるサーバーレスプラットフォームです。これにより、インフラストラクチャを気にせずに、イベントベースのアプリケーションを構築できるようになります。

この例では、AEM イベントを受け取り、イベントの詳細を記録する Adobe I/O Runtime [アクション](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)を作成します。
https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/

大まかな手順は次のとおりです。

- Adobe Developer Console でのプロジェクトの作成
- プロジェクトをローカル開発用に初期化
- Adobe Developer Console でのプロジェクトの設定
- AEM イベントのトリガーとアクションの実行の検証

## 前提条件

このチュートリアルを完了するには、次が必要になります。

- [AEM イベント処理が有効](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment)になっている AEM as a Cloud Service環境。

- [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/) へのアクセス。

- ローカルマシンにインストールされた [Adobe Developer CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/)。

## Adobe Developer Console でのプロジェクトの作成

Adobe Developer Console でプロジェクトを作成するには、次の手順に従います。

- [Adobe Developer Console](https://developer.adobe.com/) に移動し、「**コンソール**」ボタンをクリックします。

- 「**クイックスタート**」セクションで、「**テンプレートからプロジェクトを作成**」をクリックします。次に、**テンプレートを参照**&#x200B;ダイアログで、**App Builder** テンプレートを選択します。

- 必要に応じて、プロジェクトタイトル、アプリ名、ワークスペースを更新します。次に、「**保存**」をクリックします。

  ![Adobe Developer Console でのプロジェクトの作成](../assets/examples/runtime-action/create-project.png)


## プロジェクトをローカル開発用に初期化

プロジェクトに Adobe I/O Runtime アクションを追加するには、ローカル開発用にプロジェクトを初期化する必要があります。ローカルマシンのターミナルを開き、プロジェクトを初期化する場所に移動し、次の手順に従います。

- 実行によるプロジェクトの初期化

  ```bash
  aio app init
  ```

- `Organization`、前の手順で作成した `Project` およびワークスペースを選択します。`What templates do you want to search for?` 手順で、「`All Templates`」オプションを選択します。

  ![Org-Project-Selection - プロジェクトの初期化](../assets/examples/runtime-action/all-templates.png)

- テンプレートのリストで、「`@adobe/generator-app-excshell`」オプションを選択します。

  ![拡張テンプレート - プロジェクトの初期化](../assets/examples/runtime-action/extensibility-template.png)

- お気に入りの IDE で、VSCode などのプロジェクトを開きます。

- 選択した&#x200B;_拡張テンプレート_（`@adobe/generator-app-excshell`）は、汎用のランタイムアクションを提供します。コードは `src/dx-excshell-1/actions/generic/index.js` ファイルにあります。イベントを更新してシンプルにし、イベントの詳細をログに記録して、成功応答を返しましょう。ただし、次の例では、受け取った AEM イベントを処理するように拡張されます。

  ```javascript
  const fetch = require("node-fetch");
  const { Core } = require("@adobe/aio-sdk");
  const {
  errorResponse,
  getBearerToken,
  stringParameters,
  checkMissingRequestInputs,
  } = require("../utils");
  
  // main function that will be executed by Adobe I/O Runtime
  async function main(params) {
  // create a Logger
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });
  
  try {
      // 'info' is the default level if not set
      logger.info("Calling the main action");
  
      // log parameters, only if params.LOG_LEVEL === 'debug'
      logger.debug(stringParameters(params));
  
      const response = {
      statusCode: 200,
      body: {
          message: "Received AEM Event, it will be processed in next example",
      },
      };
  
      // log the response status code
      logger.info(`${response.statusCode}: successful request`);
      return response;
  } catch (error) {
      // log any server errors
      logger.error(error);
      // return with 500
      return errorResponse(500, "server error", logger);
  }
  }
  
  exports.main = main;
  ```

- 最後に、更新されたアクションを実行して Adobe I/O Runtime にデプロイします。

  ```bash
  aio app deploy
  ```

## Adobe Developer Console でのプロジェクトの設定

AEM イベントを受け取り、前の手順で作成した Adobe I/O Runtime アクションを実行するには、Adobe Developer Console でプロジェクトを設定します。

- Adobe Developer Console で、前の手順で作成した[プロジェクト](https://developer.adobe.com/console/projects)に移動し、クリックして開きます。`Stage` ワークスペースを選択します。ここにアクションがデプロイされます。

- 「**サービスを追加**」ボタンをクリックし、「**API**」オプションを選択します。**API を追加**&#x200B;モーダルで、**アドビサービス**／**I/O Management API** を選択し、「**次へ**」をクリックします。追加の設定手順に従って「**設定済み API を保存**」をクリックします。

  ![サービスを追加 - プロジェクトの設定](../assets/examples/runtime-action/add-io-management-api.png)

- 同様に、「**サービスを追加**」ボタンをクリックし、「**イベント**」オプションを選択します。**イベントを追加**&#x200B;ダイアログで、**Experience Cloud**／**AEM Sites** を選択し、「**次へ**」をクリックします。追加の設定手順に従い、AEMCS インスタンス、イベントタイプなどの詳細を選択します。

- 最後に、**イベントの受信方法**&#x200B;の手順で、「**ランタイムアクション**」オプションを展開し、前の手順で作成した&#x200B;_汎用_&#x200B;アクションを選択します。「**設定済みイベントを保存**」をクリックします。

  ![ランタイムアクション - プロジェクトの設定](../assets/examples/runtime-action/select-runtime-action.png)

- イベント登録の詳細を確認し、「**デバッグトレース**」タブに移動して、**チャレンジプローブ**&#x200B;リクエストと応答を確認します。

  ![イベント登録の詳細](../assets/examples/runtime-action/debug-tracing-challenge-probe.png)


## AEM イベントをトリガー

上記の Adobe Developer Console プロジェクトで登録した AEM as a Cloud Service 環境から AEM イベントをトリガーするには、次の手順に従います。

- [Cloud Manager](https://my.cloudmanager.adobe.com/) から AEM as a Cloud Service オーサー環境にアクセスし、ログインします。

- **購読しているイベント**&#x200B;に応じて、コンテンツフラグメントの作成、更新、削除、公開、非公開を行います。

## イベントの詳細を確認

上記の手順を完了すると、AEM イベントが汎用アクションに配信されているのを確認できます。

イベントの詳細は、イベント登録詳細の「**デバッグトレース**」タブで確認できます。

![AEM イベントの詳細](../assets/examples/runtime-action/aem-event-details.png)


## 次の手順

次の例では、このアクションを拡張して AEM イベントを処理し、AEM オーサーサービスをコールバックしてコンテンツの詳細を取得し、詳細を Adobe I/O Runtime ストレージに保存して、シングルページアプリケーション（SPA）経由で表示します。
