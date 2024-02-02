---
title: Adobe I/O Runtime Action とAEM Events
description: Adobe I/O Runtimeアクションを使用してAEMイベントを受け取り、ペイロード、ヘッダー、メタデータなどのイベントの詳細を確認する方法について説明します。
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-01-29T00:00:00Z
jira: KT-14878
thumbnail: KT-14878.jpeg
source-git-commit: f0930e517254b6353fe50c3bbf9ae915d9ef6ca3
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 0%

---


# Adobe I/O Runtime Action とAEM Events

を使用してAEMイベントを受け取る方法を説明します。 [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) アクションを作成し、ペイロード、ヘッダー、メタデータなどのイベントの詳細を確認します。

>[!VIDEO](https://video.tv.adobe.com/v/3427053?quality=12&learn=on)

Adobe I/O Runtimeは、サーバーレスのプラットフォームで、イベントに応じてコードを実行できます。Adobe I/Oは したがって、インフラストラクチャを気にせずに、イベント・ベースのアプリケーションを構築するのに役立ちます。

この例では、Adobe I/O Runtime [アクション](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) はAEMイベントを受け取り、イベントの詳細を記録します。
https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/

大まかな手順は次のとおりです。

- Adobe Developer Console でのプロジェクトの作成
- プロジェクトをローカル開発用に初期化
- Adobe Developer Console でのプロジェクトの設定
- トリガーAEMイベントと検証アクションの実行

## 前提条件

このチュートリアルを完了するには、以下が必要です。

- AEMas a Cloud Service環境 [AEM Eventing enabled](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- アクセス先 [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [Adobe Developer CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) をローカルマシンにインストールします。

>[!IMPORTANT]
>
>AEMas a Cloud Serviceイベントは、プレリリースモードで登録ユーザーのみが使用できます。 AEMのas a Cloud Service環境でAEMイベンティングを有効にするには、 [AEM-Eventing チーム](mailto:grp-aem-events@adobe.com).

## Adobe Developer Console でのプロジェクトの作成

Adobe Developer Console でプロジェクトを作成するには、次の手順に従います。

- に移動します。 [Adobe Developer Console](https://developer.adobe.com/) をクリックします。 **コンソール** 」ボタンをクリックします。

- Adobe Analytics の **クイックスタート** セクションで、 **テンプレートからプロジェクトを作成**. 次に、 **テンプレートを参照** ダイアログ、選択 **App Builder** テンプレート。

- 必要に応じて、プロジェクトタイトル、アプリ名、ワークスペースを追加します。 次に、「 **保存**.

  ![Adobe Developer Console でのプロジェクトの作成](../assets/examples/runtime-action/create-project.png)


## プロジェクトをローカル開発用に初期化

プロジェクトにAdobe I/O Runtime Action を追加するには、ローカル開発用にプロジェクトを初期化する必要があります。 ローカルマシンのターミナルを開き、プロジェクトを初期化する場所に移動し、次の手順に従います。

- 実行によるプロジェクトの初期化

  ```bash
  aio app init
  ```

- を選択します。 `Organization`、 `Project` 前の手順で作成したワークスペースと、 In `What templates do you want to search for?` ステップ、選択 `All Templates` オプション。

  ![Org-Project-Selection — プロジェクトを初期化](../assets/examples/runtime-action/all-templates.png)

- テンプレートのリストで、「 」を選択します。 `@adobe/generator-app-excshell` オプション。

  ![拡張機能テンプレート — プロジェクトの初期化](../assets/examples/runtime-action/extensibility-template.png)

- お気に入りの IDE で、VSCode などのプロジェクトを開きます。

- 選択した _拡張テンプレート_ (`@adobe/generator-app-excshell`) は汎用のランタイムアクションを提供します。コードは `src/dx-excshell-1/actions/generic/index.js` ファイル。 イベントを更新してシンプルにし、イベントの詳細をログに記録して、成功応答を返します。 ただし、次の例では、受け取ったAEMイベントを処理するように拡張されます。

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

- 最後に、を実行して、更新したアクションをAdobe I/O Runtimeにデプロイします。

  ```bash
  aio app deploy
  ```

## Adobe Developer Console でのプロジェクトの設定

AEMイベントを受け取り、前の手順で作成したAdobe I/O Runtimeアクションを実行するには、Adobe Developerコンソールでプロジェクトを設定します。

- Adobe Developerコンソールで、 [プロジェクト](https://developer.adobe.com/console/projects) 前の手順で作成し、をクリックして開きます。 を選択します。 `Stage` workspace、アクションがデプロイされた場所です。

- クリック **サービスを追加** ボタンと選択 **API** オプション。 Adobe Analytics の **API を追加** モーダルを選択します。 **Adobe サービス** > **I/O 管理 API** をクリックします。 **次へ**、追加の設定手順に従って、「 」をクリックします。 **設定済み API を保存**.

  ![サービスを追加 — プロジェクトを設定](../assets/examples/runtime-action/add-io-management-api.png)

- 同様に、「 **サービスを追加** ボタンと選択 **イベント** オプション。 Adobe Analytics の **イベントを追加** ダイアログ、選択 **Experience Cloud** > **AEM Sites**&#x200B;をクリックし、 **次へ**. 追加の設定手順に従い、AEMCS インスタンス、イベントタイプ、その他の詳細を選択します。

- 最後に、 **イベントの受信方法** ステップ、展開 **実行時アクション** オプションを選択し、 _汎用_ 前の手順で作成したアクション。 クリック **設定済みイベントを保存**.

  ![実行時アクション — プロジェクトの設定 ](../assets/examples/runtime-action/select-runtime-action.png)

- イベント登録の詳細を確認し、 **デバッグトレース** 「 」タブに移動して、 **チャレンジプローブ** リクエストと応答。

  ![イベント登録の詳細](../assets/examples/runtime-action/debug-tracing-challenge-probe.png)


## トリガーAEMイベント

上記のAdobe Developer Console プロジェクトに登録されたAEMas a Cloud Service環境からAEMイベントをトリガーするには、次の手順に従います。

- を介してAEM as a Cloud Serviceオーサー環境にアクセスし、ログインします。 [Cloud Manager](https://my.cloudmanager.adobe.com/).

- 次の条件に応じて、 **購読済みイベント**&#x200B;コンテンツフラグメントの作成、更新、削除、公開、非公開を行います。

## イベントの詳細を確認

上記の手順を完了すると、AEMイベントが汎用のアクションに配信されていることを確認できます。

イベントの詳細は、 **デバッグトレース** イベント登録の詳細のタブ。

![AEMイベントの詳細](../assets/examples/runtime-action/aem-event-details.png)


## 次の手順

次の例では、このアクションを拡張してAEMイベントを処理し、AEMオーサーサービスを呼び出してコンテンツの詳細を取得し、詳細をAdobe I/O Runtimeストレージに保存して、シングルページアプリケーション (SPA) 経由で表示します。

