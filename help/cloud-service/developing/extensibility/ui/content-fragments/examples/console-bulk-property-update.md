---
title: プロパティの一括更新の例 AEM コンテンツフラグメントコンソールの拡張機能
description: コンテンツフラグメントのプロパティを一括更新する AEM コンテンツフラグメントコンソール拡張機能の例。
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11604
thumbnail: KT-11604.png
doc-type: article
last-substantial-update: 2024-01-26T00:00:00Z
exl-id: fbfb5c10-95f8-4875-88dd-9a941d7a16fd
duration: 1362
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '769'
ht-degree: 100%

---

# プロパティの一括アップデートの例の拡張

>[!VIDEO](https://video.tv.adobe.com/v/3454450?captions=jpn&quality=12&learn=on)

この例の AEM コンテンツフラグメントコンソール拡張機能は、コンテンツフラグメントプロパティを共通の値に一括アップデートする [アクションバー](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) の拡張機能です。

この例の拡張機能の機能フローは次のとおりです。

![Adobe I/O Runtimeアクションフロー](./assets/bulk-property-update/flow.png){align="center"}

1. コンテンツフラグメントを選択し、[アクションバー](#extension-registration)で「拡張機能」ボタンをクリックすると、[モーダル](#modal)が開きます。
2. この[モーダル](#modal)は、[React Spectrum](https://react-spectrum.adobe.com/react-spectrum/) で構築されたカスタム入力フォームを表示します。
3. フォームを送信すると、選択したコンテンツフラグメントのリストと AEM ホストが[カスタム Adobe I/O Runtime アクション](#adobe-io-runtime-action)に送信されます。
4. この [Adobe I/O Runtime アクション](#adobe-io-runtime-action)は入力を検証し、AEM に HTTP PUT リクエストを行い、選択したコンテンツフラグメントを更新します。
5. 指定したプロパティをアップデートするための、コンテンツフラグメントごとの一連の HTTP PUT。
6. AEM as a Cloud Service は、プロパティのアップデートをコンテンツフラグメントに保持し、成功または失敗の応答を Adobe I/O Runtime アクションに返します。
7. モーダルは Adobe I/O Runtime アクションから応答を受け取り、成功した一括アップデートのリストを表示します。

## 拡張ポイント

この例では、拡張機能ポイント `actionBar` まで拡張して、コンテンツフラグメントコンソールにカスタムボタンを追加します。

| AEM UI 拡張 | 拡張機能ポイント |
| ------------------------ | --------------------- |
| [コンテンツフラグメントコンソール](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/) | [アクションバー](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) |


## 拡張機能の例

この例では、既存の Adobe Developer Console プロジェクトを使用し、`aio app init` を介して App Builder アプリを初期化する際に、次のオプションを使用します。

+ 検索するテンプレートは？：`All Extension Points`
+ インストールするテンプレートを選択：` @adobe/aem-cf-admin-ui-ext-tpl`
+ 拡張機能の名前は？：`Bulk property update`
+ 拡張機能の短い説明を入力：`An example action bar extension that bulk updates a single property one or more content fragments.`
+ 開始するバージョン：`0.0.1`
+ 次は何を行いますか？
   + `Add a custom button to Action Bar`
      + ボタンのラベル名を指定：`Bulk property update`
      + ボタンのモーダルを表示する必要がありますか？`y`
   + `Add server-side handler`
      + Adobe I/O Runtime では、サーバーレスのコードをオンデマンドで呼び出すことができます。 このアクションの名前を指定：`generic`

生成された App Builder 拡張機能アプリが、以下に示すようにアップデートされます。

### アプリルート{#app-routes}

`src/aem-cf-console-admin-1/web-src/src/components/App.js` には [React ルーター](https://reactrouter.com/en/main)が含まれています。

次の 2 つの論理セットのルートがあります。

1. 最初のルートはリクエストを `index.html` にマップし、[拡張機能登録](#extension-registration)を担当する React コンポーネントを呼び出します。

   ```javascript
   <Route index element={<ExtensionRegistration />} />
   ```

1. 2 つ目のルートセットは、拡張機能のモーダルのコンテンツをレンダリングする React コンポーネントに URL をマップします。 この `:selection` param は、区切られたリストコンテンツフラグメントのパスを表します。

   個別のアクションを呼び出すための複数のボタンが拡張機能にある場合、それぞれの[拡張機能の登録](#extension-registration)は、ここで定義されたルートにマップされます。

   ```javascript
   <Route
       exact path="content-fragment/:selection/bulk-property-update"
       element={<BulkPropertyUpdateModal />}
       />
   ```

### 拡張機能の登録

`index.html` ルートにマッピングされた `ExtensionRegistration.js` は、AEM 拡張機能のエントリポイントであり、以下を定義します。

1. AEM オーサリングエクスペリエンスに表示される「拡張機能」ボタンの場所（`actionBar` または `headerMenu`）
1. `getButtons()` 関数内の拡張機能ボタンの定義 
1. `onClick()` 関数内のボタンのクリックハンドラー

+ `src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
import { generatePath } from "react-router";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // Some unique ID for the extension used to facilitate communication between the extension and Content Fragment Console
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButtons() {
            return [
              {
                id: "examples.action-bar.bulk-property-update", // Unique ID for the button
                label: "Bulk property update", // Button label
                icon: "OpenIn", // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
                // Click handler for the extension button
                onClick(selections) {
                  // Collect the selected content fragment paths
                  const selectionIds = selections.map(
                    (selection) => selection.id
                  );

                  // Create a URL that maps to the
                  const modalURL =
                    "/index.html#" +
                    generatePath(
                      "/content-fragment/:selection/bulk-property-update",
                      {
                        // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                        selection: encodeURIComponent(selectionIds.join("|")),
                      }
                    );

                  // Open the route in the extension modal using the constructed URL
                  guestConnection.host.modal.showUrl({
                    // The modal title
                    title: "Bulk property update",
                    url: modalURL,
                  });
                },
              },
            ];
          },
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### モーダル

[`App.js`](#app-routes) で定義されている拡張機能の各ルートは、拡張機能のモーダルでレンダリングされる React コンポーネントにマップされます。

このサンプルアプリには、3 つの状態を持つモーダル React コンポーネント（`BulkPropertyUpdateModal.js`）があります。

1. 読み込み中。ユーザーが待機する必要があることを示しています。
1. ユーザーが更新するプロパティ名と値を指定できる一括プロパティ更新フォーム
1. 一括プロパティアップデート操作の応答。アップデートされたコンテンツフラグメントとアップデートできなかったコンテンツフラグメントのリスト

重要な点は、拡張機能からの AEM とのインタラクションは、[AppBuilder Adobe I/O Runtime アクション](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)に委任する必要があります。これは、[Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/) で実行される別のサーバーレスプロセスです。
Adobe I/O Runtime アクションを使用して AEM と通信するのは、クロスオリジンリソース共有（CORS）接続の問題を回避するためです。

プロパティ一括更新フォームが送信されると、カスタムの `onSubmitHandler()` が Adobe I/O Runtime アクションを呼び出し、現在の AEM ホスト（ドメイン）とユーザーの AEM アクセストークンを渡します。これが [AEM コンテンツフラグメント API](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html?lang=ja) を呼び出して、コンテンツフラグメントを更新します。

Adobe I/O Runtime アクションからの応答を受け取ると、モーダルが更新され、一括プロパティ更新操作の結果が表示されます。

+ `src/aem-cf-console-admin-1/web-src/src/components/BulkPropertyUpdateModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Form,
  Provider,
  Content,
  defaultTheme,
  ContextualHelp,
  Text,
  TextField,
  ButtonGroup,
  Button,
  Heading,
  ListView,
  Item,
} from '@adobe/react-spectrum'

import Spinner from "./Spinner"

import { useParams } from "react-router-dom"

import allActions from '../config.json'
import actionWebInvoke from '../utils'

import { extensionId } from "./Constants"

export default function BulkPropertyUpdateModal() {
  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState()
  
  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  const [propertyName, setPropertyName] = useState(null);
  const [propertyValue, setPropertyValue] = useState(null);
  const [validationState, setValidationState] = useState({});

  // Get the selected content fragment paths from the route parameter `:selection`
  let { selection } = useParams();
  let fragmentIds = selection?.split('|') || [];
  
  console.log('Content Fragment Ids', fragmentIds);

  if (!fragmentIds || fragmentIds.length === 0) {
    console.error("Unable to locate a list of Content Fragments to update.")
    return;
  }

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
       // extensionId is the unique id of this extension (you can make this up as long as its unique) .. in this case its `bulk-property-update` pulled out into Constants.js as it is also referenced in ExtensionRegistration.js
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])


  // Determine view to display in the modal
  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else if (actionInvokeInProgress) {
    // If the bulk property action has been invoked but not completed, display a loading spinner
    return <Spinner />
  } else if (actionResponse) {
    // If the bulk property action has completed, display the response
    return renderResponse();
  } else {
    // Else display the bulk property update form
    return renderForm();
  }

  /**
   * Renders the Bulk Property Update form. 
   * This form has two fields:
   * - Property Name: The name of the Content Fragment property name to update
   * - Property Value: the value the Content Fragment property, specified by the Property Name, will be updated to
   * 
   * @returns the Bulk Property Update form
   */
  function renderForm() {
    return (
      // Use React Spectrum components to render the form
      <Provider theme={defaultTheme} colorScheme='light'>
        <Content width="100%">
          <Flex width="100%">
            <Form 
              width="100%">
              <TextField label="Property name"
                          isRequired={true}
                          validationState={validationState?.propertyName}
                onChange={setPropertyName}
                contextualHelp={
                  <ContextualHelp>
                    <Heading>Need help?</Heading>
                    <Content>
                      <Text>The <strong>Property name</strong> must be a valid for the Content Fragment model(s) the selected Content Fragments implement.</Text>
                    </Content>
                  </ContextualHelp>
                } />

              <TextField
                label="Property value"
                validationState={validationState?.propertyValue}
                onChange={setPropertyValue} />

              <ButtonGroup align="start" marginTop="size-200">
                <Button variant="cta" onPress={onSubmitHandler}>Update {fragmentIds.length} Content Fragments</Button>
              </ButtonGroup>
            </Form>
          </Flex>

          {/* Render the close button so the user can close the modal */}
          {renderCloseButton()}
        </Content>
      </Provider>
    )
  }
  /**
   * Display the response from the Adobe I/O Runtime action in the modal.
   * This includes:
   * - A list of content fragments that were updated successfully
   * - A list a content fragments that failed to update
   * 
   * @returns the response view
   */
  function renderResponse() {
    // Separate the successful and failed content fragments updates
    const successes = actionResponse.filter(item => item.status === 200);
    const failures = actionResponse.filter(item => item.status !== 200);

    return (
      <Provider theme={defaultTheme} colorScheme='light'>
        <Content width="100%">

          <Text>Bulk updated property <strong>{propertyName}</strong> with value <strong>{propertyValue}</strong></Text>

          {/* Render the list of content fragments that were updated successfully */}
          {successes.length > 0 &&
            <><Heading level="4">{successes.length} Content Fragments successfully updated</Heading>
              <ListView
                items={successes}
                selectionMode="none"
                aria-label="Successful updates"
              >
                {(item) => (
                  <Item key={item.fragmentId} textValue={item.fragmentId.split('/').pop()}>
                    {item.fragmentId.split('/').pop()}
                  </Item>
                )}
              </ListView></>}

          {/* Render the list of content fragments that failed to update */}
          {failures.length > 0 &&
            <><Heading level="4">{failures.length} Content Fragments failed to update</Heading><ListView
              items={failures}
              selectionMode="none"
              aria-label="Failed updates"
            >
              {(item) => (
                <Item key={item.fragmentId} textValue={item.fragmentId.split('/').pop()}>
                  {item.fragmentId.split('/').pop()}
                </Item>
              )}
            </ListView></>}

          {/* Render the close button so the user can close the modal */}
          {renderCloseButton()}
        </Content>
      </Provider>);
  }

  /**
   * Provide a close button for the modal, else it cannot be closed (without refreshing the browser window)
   * 
   * @returns a button that closes the modal.
   */
   function renderCloseButton() {
    return (
      <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
        <ButtonGroup align="end">
          <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
        </ButtonGroup>
      </Flex>
    );
  }

  /**
   * Handle the Bulk Property Update form submission.
   * This function calls the supporting Adobe I/O Runtime action to update the selected Content Fragments, and then returns the response for display in the modal
   * When invoking the Adobe I/O Runtime action, the following parameters are passed as they're used by the action to connect to AEM:
   * - AEM Host to connect to
   * - AEM access token to connect to AEM with
   * - The list of Content Fragment paths to update
   * - The Content Fragment property name to update
   * - The value to update the Content Fragment property to
   * 
   * @returns a list of content fragment update successes and failures
   */
  async function onSubmitHandler() {
    // Validate the form input fields
    if (propertyName?.length > 1) {
      setValidationState({propertyName: 'valid', propertyValue: 'valid'});
    } else {
      setValidationState({propertyName: 'invalid', propertyValue: 'valid'});
      return;
    }

    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
    };

    console.log('headers', headers);

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {
      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,

      fragmentIds: fragmentIds,
      propertyName: propertyName,
      propertyValue: propertyValue
    };

    // Invoke the Adobe I/O Runtime action named `generic`. This name defined in the `ext.config.yaml` file.
    const action = 'generic';

    try {
      // Invoke Adobe I/O Runtime action with the configured headers and parameters
      const actionResponse = await actionWebInvoke(allActions[action], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(actionResponse);

      console.log(`Response from ${action}:`, actionResponse)
    } catch (e) {
      // Log and store any errors
      console.error(e)
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```


### Adobe I/O Runtime アクション

AEM 拡張機能の App Builder アプリは、0 個または複数の Adobe I/O Runtime アクションを定義または使用できます。
Adobe Runtime アクションは、AEM またはその他のアドビの web サービスとの対話を必要とする作業を担当する必要があります。

このサンプルアプリでは、Adobe I/O Runtime アクションは、デフォルトの名前 `generic` を使用し、次を担当します。

1. AEM コンテンツフラグメント API に対して一連の HTTP リクエストを実行して、コンテンツフラグメントを更新する。
1. これらの HTTP 要求の応答を収集し、成功と失敗に並べ替える
1. モーダル（`BulkPropertyUpdateModal.js`）で表示する成功と失敗のリストを返す

+ `src/aem-cf-console-admin-1/actions/generic/index.js`


```javascript
const fetch = require('node-fetch')
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, getBearerToken, stringParameters, checkMissingRequestInputs } = require('../utils')

// main function that will be executed by Adobe I/O Runtime
async function main (params) {
  // create a Logger
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

  try {
    // 'info' is the default level if not set
    logger.info('Calling the main action')

    // log parameters, only if params.LOG_LEVEL === 'debug'
    logger.debug(stringParameters(params))

    // check for missing request input parameters and headers
    const requiredParams = [ 'aemHost', 'fragmentIds', 'propertyName', 'propertyValue' ]
    const requiredHeaders = ['Authorization']
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger)
    }
      
    const body = {
      "properties": {
        "elements": {
          [params.propertyName]: {
            "value": params.propertyValue
          }
        }
      }
    };

    // Extract the user Bearer token from the Authorization header used to authenticate the request to AEM
    const accessToken = getBearerToken(params);

    let results = await Promise.all(params.fragmentIds.map(async (fragmentId) => {

      logger.info(`Updating fragment ${fragmentId} with property ${params.propertyName} and value ${params.propertyValue}`);

      const res = await fetch(`${params.aemHost}${fragmentId.replace('/content/dam/', '/api/assets/')}.json`, { 
        method: 'put',
        body: JSON.stringify(body),
        headers: {
          'Authorization': `Bearer ${accessToken}`,
          'Content-Type': 'application/json'
        }
      });

      if (res.ok) {
        logger.info(`Successfully updated ${fragmentId}`);
        return { fragmentId, status: res.status, statusText: res.statusText, body: await res.json() };
      } else {
        logger.info(`Failed to update ${fragmentId}`);
        return { fragmentId, status: res.status, statusText: res.statusText, body: await res.text() };
      }
    }));

    const response = {
      statusCode: 200,
      body: results
    };

    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the A
     return response;

  } catch (error) {
    // log any server errors
    logger.error(error)
    // return with 500
    return errorResponse(500, 'server error', logger)
  }
}

exports.main = main
```
