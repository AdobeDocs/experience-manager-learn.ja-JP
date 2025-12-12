---
title: リッチテキストエディター（RTE）へのウィジェットの追加
description: AEM コンテンツフラグメントエディターのリッチテキストエディター（RTE）にウィジェットを追加する方法を説明します。
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13465
thumbnail: KT-13465.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 167a4b11-1202-4c7a-b022-f3f996348a4e
duration: 476
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 100%

---

# リッチテキストエディター（RTE）へのウィジェットの追加

AEM コンテンツフラグメントエディターのリッチテキストエディター（RTE）にウィジェットを追加する方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3447433?captions=jpn&quality=12&learn=on)

リッチテキストエディター（RTE）に動的コンテンツを追加するには、**ウィジェット**&#x200B;機能を使用できます。ウィジェットは、シンプルな UI または複雑な UI を RTE に統合するのに役立ち、UI は選択した JS フレームワークを使用して作成できます。これらは、RTE で `{` 特殊キーを押すと開くダイアログと考えることができます。

通常、ウィジェットは、外部システムに依存関係がある動的コンテンツや、現在のコンテキストに基づいて変更される可能性のある動的コンテンツを挿入するために使用します。

**ウィジェット**&#x200B;は、`rte` 拡張ポイントを使用してコンテンツフラグメントエディターの **RTE** に追加します。`rte` 拡張ポイントの `getWidgets()` メソッドを使用して、1 つ以上のウィジェットを追加します。これらをトリガーするには、`{` 特殊キーを押してコンテキストメニューオプションを開き、目的のウィジェットを選択してカスタムダイアログ UI を読み込みます。

この例では、RTE コンテンツ内で WKND アドベンチャー固有のディスカウントコードを検索、選択、追加するために、_ディスカウントコードリスト_&#x200B;というウィジェットを追加する方法を示します。これらのディスカウントコードは、注文管理システム（OMS）、製品情報管理（PIM）、自社製アプリケーション、Adobe AppBuilder アクションなどの外部システムで管理できます。

作業をシンプルにするために、この例では [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) フレームワークを使用して、ウィジェットまたはダイアログ UI とハードコードされた WKND アドベンチャー名、ディスカウントコードデータを開発します。

## 拡張機能ポイント

この例では、拡張ポイント `rte` まで拡張して、コンテンツフラグメントエディターの RTE にウィジェットを追加します。

| AEM UI 拡張 | 拡張ポイント |
| ------------------------ | --------------------- |
| [コンテンツフラグメントエディター](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [リッチテキストエディターウィジェット](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## 拡張機能の例

次の例では、_ディスカウントコードリスト_&#x200B;ウィジェットを作成します。RTE 内で `{` 特殊キーを押すとコンテキストメニューが開き、コンテキストメニューから「_ディスカウントコードリスト_」オプションを選択するとダイアログ UI が開きます。

WKND コンテンツ作成者は、現在のアドベンチャー固有のディスカウントコード（使用可能な場合）を検索、選択、追加できます。

### 拡張機能の登録

index.html ルートにマッピングされた `ExtensionRegistration.js` は、AEM 拡張機能のエントリポイントであり、次を定義します。

+ `id, label and url` 属性を含む `getWidgets()` 関数のウィジェット定義。
+ `url` 属性値、ダイアログ UI を読み込むための相対 URL パス（`/index.html#/discountCodes`）。

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

// This function is called when the extension is registered with the host and runs in an iframe in the Content Fragment Editor browser window.
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        rte: {
          // RTE Widgets
          getWidgets: () => [
            {
              id: "discountcode-list-widget", // Provide a unique ID for the widget
              label: "Discount Code List", // Provide a label for the widget
              url: "/index.html#/discountCodes", // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/discountCodes`
            },
          ],
        }, // Add a comma here
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### `App.js` で `discountCodes` ルートを追加{#add-widgets-route}

React のメインコンポーネント `App.js` に、`discountCodes` ルートを追加して、上記の相対 URL パスの UI をレンダリングします。

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
...

<Routes>
  <Route index element={<ExtensionRegistration />} />
  <Route
    exact path="index.html"
    element={<ExtensionRegistration />}
  />

  {/* Content Fragment RTE routes that support the Discount Codes Widget functionality*/}
  <Route path="/discountCodes" element={<DiscountCodes />} />
</Routes>
...
```

### `DiscountCodes` React コンポーネントの作成{#create-widget-react-component}

ウィジェットまたはダイアログ UI は、[Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) フレームワークを使用して作成します。`DiscountCodes` コンポーネントのコードと、主なハイライトを次に示します。

+ UI は、[ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html)、[ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html)、[Button](https://react-spectrum.adobe.com/react-spectrum/Button.html) などの React Spectrum コンポーネントを使用してレンダリングします。
+ `adventureDiscountCodes` 配列には、アドベンチャー名とディスカウントコードのハードコードされたマッピングが含まれています。実際のシナリオでは、このデータは Adobe AppBuilder アクションや、PIM、OMS、自社製またはクラウドプロバイダーベースの API ゲートウェイなどの外部システムから取得できます。
+ `guestConnection` は、`useEffect` [React hook](https://react.dev/reference/react/useEffect) を使用して初期化し、コンポーネントの状態として管理します。これは、AEM ホストとの通信に使用します。
+ `handleDiscountCodeChange` 関数は、選択したアドベンチャー名のディスカウントコードを取得し、状態変数を更新します。
+ `guestConnection` オブジェクトを使用する `addDiscountCode` 関数は、実行する RTE 命令を提供します。この場合、`insertContent` 命令と、実際のディスカウントコードの HTML コードスニペットを RTE に挿入します。

`src/aem-cf-editor-1/web-src/src/components/DiscountCodes.js`

```javascript
import {
  Button,
  ButtonGroup,
  ComboBox,
  Content,
  Divider,
  Flex, Form,
  Item,
  Provider,
  Text,
  defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';

const DiscountCodes = () => {

  // The Adventure Discount Code list
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const adventureDiscountCodes = [
    { id: 1, adventureName: 'BALI SURF CAMP', discountCode: 'BALI2023' },
    { id: 2, adventureName: 'BEERVANA IN PORTLAND', discountCode: 'PORTFEST' },
    { id: 3, adventureName: 'NAPA WINE TASTING', discountCode: 'WINEINSPRING' },
    { id: 4, adventureName: 'RIVERSIDE CAMPING', discountCode: 'SUMMERSCAPE' },
    { id: 5, adventureName: 'TAHOE SKIING', discountCode: 'EPICPASS' },
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [discountCode, setDiscountCode] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `discountCodeList` Dropdown change
  const handleDiscountCodeChange = (key) => {

    if (key) {
      //console.log(`DiscountCode Key: ${key}`);
      //console.log(`DiscountCode Value: ${adventureDiscountCodes[key-1].discountCode}`);

      //Get discount code value using selected key/index
      let discountCodeValue = adventureDiscountCodes[key - 1].discountCode;

      //update the `discountCode` state
      setDiscountCode(discountCodeValue);
    }
  };

  // Add the selected Adventure's Discount Code into the RTE
  const addDiscountCode = () => {

    if (discountCode) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<strong>Discount Code: ${discountCode}</strong>` }]);
    }

  };

  // Adobe React Spectrum (HTML code) that renders the Discount Code dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">

            <Text>Selected Discount Code: <strong>{discountCode}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="discountCodeList"
              label="Type or Select an Adventure name"
              defaultItems={adventureDiscountCodes}
              onSelectionChange={handleDiscountCodeChange}>
              {item => <Item>{item.adventureName}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addDiscountCode} autoFocus>Add</Button>
              <Button variant="secondary" onPress={() => setDiscountCode(null)}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );
}

export default DiscountCodes;
```
