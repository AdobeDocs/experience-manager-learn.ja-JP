---
title: Adobe I/O Runtime アクションを使用した AEM イベント処理
description: Adobe I/O Runtime アクションを使用して受け取った AEM イベントを処理する方法を説明します。
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 558
last-substantial-update: 2024-01-30T00:00:00Z
jira: KT-14879
thumbnail: KT-14879.jpeg
exl-id: c362011e-89e4-479c-9a6c-2e5caa3b6e02
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '548'
ht-degree: 100%

---

# Adobe I/O Runtime アクションを使用した AEM イベント処理

[Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) アクション使用して受け取った AEM イベントを処理する方法を説明します。この例は、前述した例 [Adobe I/O Runtime アクションと AEM イベント](runtime-action.md)を強化したものです。この例に進む前に、それが完了していることを確認します。

>[!VIDEO](https://video.tv.adobe.com/v/3427054?quality=12&learn=on)

この例では、イベント処理によって元のイベントデータと受信したイベントがアクティビティメッセージとして、Adobe I/O Runtime ストレージに保存されます。ただし、イベントが&#x200B;_変更済みコンテンツフラグメント_&#x200B;タイプの場合は、AEM オーサーサービスも呼び出して、変更の詳細を検索します。最後に、イベントの詳細が単一ページアプリケーション（SPA）に表示されます。

## 前提条件

このチュートリアルを完了するには、次が必要になります。

- [AEM イベンティングが有効な](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) AEM as a Cloud Service 環境。 また、サンプル [WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) プロジェクトをそこにデプロイする必要があります。

- [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/) へのアクセス権。

- ローカルマシンにインストールされた [Adobe Developer CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/)。

- 前述の例 [Adobe I/O Runtime アクションと AEM イベント](./runtime-action.md#initialize-project-for-local-development)からローカルに初期化されたプロジェクト。

## AEM イベントプロセッサーアクション

この例では、イベントプロセッサー[アクション](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)は、次のタスクを実行します。

- 受信したイベントを解析してアクティビティメッセージにします。
- 受信したイベントが&#x200B;_変更済みコンテンツフラグメント_&#x200B;タイプの場合は、AEM オーサーサービスにコールバックして、変更の詳細を確認します。
- 元のイベントデータ、アクティビティメッセージおよび変更の詳細（存在する場合）を、Adobe I/O Runtime ストレージに保持します。

上記のタスクを実行するには、まず、プロジェクトにアクションを追加し、JavaScript モジュールを開発して上記のタスクを実行し、最後に、開発したモジュールを使用するようにアクションコードを更新します。

完全なコードについて詳しくは、添付の [WKND-AEM-Eventing-Runtime-Action.zip](../assets/examples/event-processing-using-runtime-action/WKND-AEM-Eventing-Runtime-Action.zip) ファイルを参照してください。以下の節では主要なファイルをハイライト表示しています。

### アクションの追加

- アクションを追加するには、次のコマンドを実行します。

  ```bash
  aio app add action
  ```

- アクションテンプレートとして「`@adobe/generator-add-action-generic`」を選択し、アクションに `aem-event-processor` という名前を付けます。

  ![アクションの追加](../assets/examples/event-processing-using-runtime-action/add-action-template.png)

### JavaScript モジュールの開発

上記のタスクを実行するには、次の JavaScript モジュールを開発します。

- `src/dx-excshell-1/actions/aem-event-processor/eventValidator.js` モジュールは、受信したイベントが&#x200B;_変更済みコンテンツフラグメント_&#x200B;タイプであるかどうかを判断します。

  ```javascript
  async function needsAEMCallback(aemEvent) {
  // create a Logger
  const logger = Core.Logger('eventValidator', {
      level: 'info',
  });
  
  let isValid = false;
  
  // verify the event is a Content Fragment Modified event
  if (
      aemEvent
      && aemEvent.ContentType === 'contentFragment'
      && aemEvent.EventName === 'modified'
  ) {
      logger.info('Processing Content Fragment Modified Event');
      isValid = true;
  }
  
  return isValid;
  }
  
  module.exports = needsAEMCallback;
  ```

- `src/dx-excshell-1/actions/aem-event-processor/loadEventDetailsFromAEM.js` モジュールは AEM オーサーサービスを呼び出して、変更の詳細を見つけます。

  ```javascript
  ...
  const auth = require('@adobe/jwt-auth');
  ...
  // Get AEM Service Credentials aka Technical Account details.
  // These are passed to the action as params and added in .env file.
  const clientId = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTID;
  const technicalAccountId = params.AEM_SERVICECREDENTIALS_ID;
  const orgId = params.AEM_SERVICECREDENTIALS_ORG;
  const clientSecret = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTSECRET;
  // Private key is passed as a string with \n and \r characters escaped.
  const privateKey = params.AEM_SERVICECREDENTIALS_PRIVATEKEY.replace(
      /\\n/g,
      '\n',
  ).replace(/\\r/g, '\r');
  const metaScopes = params.AEM_SERVICECREDENTIALS_METASCOPES.split(',');
  const ims = `https://${params.AEM_SERVICECREDENTIALS_IMSENDPOINT}`;
  
  // Get the access token from IMS using Adobe I/O SDK
  const { access_token } = await auth({
      clientId,
      technicalAccountId,
      orgId,
      clientSecret,
      privateKey,
      metaScopes,
      ims,
  });
  ...
  // Call AEM Author service to get the CF details using AEM Assets API
  const res = await fetch(
      `${aemAuthorHost}${cfPath.replace('/content/dam/', '/api/assets/')}.json`,
  {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${access_token}`,
    },
  },
  );
  
  let newValuesOfCFPropertiesAddedOrModified = {};
  // If the response is OK, get the values of the CF properties that were added or modified
  if (res.ok) {
      logger.info('AEM Event Details loaded from AEM Author instance');
      const responseJSON = await res.json();
  
      // Get the values of the CF properties that were added or modified
      if (
      responseJSON
      && responseJSON.properties
      && responseJSON.properties.elements
      ) {
      const allCurrentCFProperties = responseJSON.properties.elements;
  
      newValuesOfCFPropertiesAddedOrModified = cfPropertiesAddedOrModified.map(
          (key) => ({
          key,
          value: allCurrentCFProperties[key],
          }),
      );
      }    
  }
  ...
  ```

  詳しくは、[AEM サービス資格情報チュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=ja)を参照してください。また、シークレットとアクションパラメーターを管理する [App Builder 設定ファイル](https://developer.adobe.com/app-builder/docs/guides/configuration/)も参照してください。

- `src/dx-excshell-1/actions/aem-event-processor/storeEventData.js` モジュールは、元のイベントデータ、アクティビティメッセージおよび変更の詳細（存在する場合）を、Adobe I/O Runtime ストレージに保存します。

  ```javascript
  ...
  const filesLib = require('@adobe/aio-lib-files');
  ...
  
  const files = await filesLib.init();
  
  const eventDataAsJSON = JSON.stringify({
      activity: activityMessage,
      aemEvent,
      aemEventDetails,
  });
  
  // store details in a folder named YYYY-MM-DD and a file named <eventID>.json
  const bytesWritten = await files.write(
      `${formattedDate}/${aemEvent.getEventID()}.json`,
      eventDataAsJSON,
  );
  ...
  ```

### アクションコードの更新

最後に、開発したモジュールを使用するように `src/dx-excshell-1/actions/aem-event-processor/index.js` でアクションコードを更新します。

```javascript
...
// handle the challenge probe request, they are sent by I/O to verify the action is valid
if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
} else {
    logger.info('AEM Event request received');

    // create AEM Event object from request parameters
    const aemEvent = new AEMEvent(params);

    // get AEM Event as activity message using the helper method
    const activityMessage = aemEvent.getEventAsActivityMessage();

    // determine if AEM Event requires callback to AEM Author service
    const callbackAEMForEventDetails = await needsAEMCallback(aemEvent);

    let eventDetails = {};
    if (callbackAEMForEventDetails) {
    // call AEM Author service to get specifics about the event
    eventDetails = await loadEventDetailsFromAEMAuthorService(
        aemEvent,
        params,
    );
    }

    // store AEM Event and Event details in the file system
    const storageDetails = await storeEventData(
    activityMessage,
    aemEvent,
    eventDetails || {},
    );
    logger.info(`Storage details: ${JSON.stringify(storageDetails)}`);

    // create response message
    responseMsg = JSON.stringify({
    message: 'AEM Event processed',
    activityMessage,
    });

    // response object
    const response = {
    statusCode: 200,
    body: responseMsg,
    };
    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
}
...
```

## その他のリソース

- `src/dx-excshell-1/actions/model` フォルダーには、`aemEvent.js` および `errors.js` ファイルが含まれています。これらのファイルは、アクションが受け取ったイベントを解析し、エラーを処理するために使用されます。
- `src/dx-excshell-1/actions/load-processed-aem-events` フォルダーにはアクションコードが含まれています。このアクションは、SPA が Adobe I/O Runtime ストレージから処理済みの AEM イベントを読み込む際に使用します。
- `src/dx-excshell-1/web-src` フォルダーには、処理された AEM イベントを表示する SPA コードが含まれます。
- `src/dx-excshell-1/ext.config.yaml` ファイルには、アクションの設定とパラメーターが含まれています。

## 概念と重要な留意点

イベント処理の要件は、プロジェクトによって異なりますが、この例で重要なポイントは次のとおりです。

- イベントの処理の実行には、Adobe I/O Runtime アクションを使用します。
- ランタイムアクションは、内部アプリケーション、サードパーティソリューション、アドビソリューションなどのシステムと通信できます。
- ランタイムアクションは、コンテンツ変更を中心に設計されたビジネスプロセスのエントリポイントとなります。
