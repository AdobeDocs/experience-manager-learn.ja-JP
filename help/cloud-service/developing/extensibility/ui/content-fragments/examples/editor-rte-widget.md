---
title: リッチテキストエディター (RTE) へのウィジェットの追加
description: AEMコンテンツフラグメントエディターでリッチテキストエディター (RTE) にウィジェットを追加する方法を説明します。
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13465
thumbnail: KT-13465.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: be4c0a6a-5c1f-4408-9ac6-56b8f0653d42
source-git-commit: 9c8c03df7c510ab697d5222f9dffd5111519b712
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# リッチテキストエディター (RTE) へのウィジェットの追加

>[!VIDEO](https://video.tv.adobe.com/v/3420822?quality=12&learn=on)

リッチテキストエディター (RTE) に動的コンテンツを追加するには、 **widgets** 機能を使用できます。 ウィジェットは、RTE にシンプルな UI または複雑な UI を統合するのに役立ち、UI は、選択した JS フレームワークを使用して作成できます。 これらは、押すことで開かれるダイアログと考えることができます `{` RTE の特殊キー。

通常、ウィジェットは、外部のシステム依存関係を持つ動的コンテンツを挿入する場合や、現在のコンテキストに基づいて変更される場合がある動的コンテンツを挿入する場合に使用します。

この **widgets** が **RTE** を使用してコンテンツフラグメントエディターで `rte` 拡張ポイント。 使用 `rte` 拡張ポイント `getWidgets()` メソッド 1 つ以上のウィジェットが追加されます。 これらは、 `{` コンテキストメニューオプションを開くための特別なキー。次に、目的のウィジェットを選択してカスタムダイアログ UI を読み込みます。

この例では、 _割引コードリスト_ をクリックし、WKND アドベンチャー固有の割引コードを RTE コンテンツ内に追加します。 これらの割引コードは、Order Management System(OMS)、Product Information Management(PIM)、Home gloned Application、Adobeの AppBuilder アクションなど、外部システムで管理できます。

話を簡単にするために、この例では [AdobeReact スペクトル](https://react-spectrum.adobe.com/react-spectrum/index.html) ウィジェットまたはダイアログ UI、およびハードコードされた WKND アドベンチャー名、割引コードデータを開発するためのフレームワーク。

## 拡張ポイント

この例は、延長点まで延長されます `rte` をクリックして、ウィジェットをコンテンツフラグメントエディターで RTE に追加します。

| AEM UI 拡張 | 拡張ポイント |
| ------------------------ | --------------------- | 
| [コンテンツフラグメントエディター](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [リッチテキストエディターウィジェット](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## 拡張機能の例

次の例では、 _割引コードリスト_ ウィジェット。 を押して、 `{` RTE 内の特別なキーを使用する場合は、コンテキストメニューが開いた後で、 _割引コードリスト_ オプションを選択すると、ダイアログ UI が開きます。

WKND コンテンツ作成者は、現在のアドベンチャー固有の割引コードがある場合、そのコードを検索、選択および追加できます。

### 拡張機能の登録

`ExtensionRegistration.js`は、index.html ルートにマッピングされ、AEM拡張機能のエントリポイントで、次の項目を定義します。

+ のウィジェット定義 `getWidgets()` ～と機能する `id, label and url` 属性。
+ この `url` 属性値、相対 URL パス (`/index.html#/discountCodes`) をクリックして、ダイアログ UI を読み込みます。

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
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
              id: "discountcode-list-widget",       // Provide a unique ID for the widget
              label: "Discount Code List",          // Provide a label for the widget
              url: "/index.html#/discountCodes",     // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/discountCodes`
            },
          ],
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```

### 追加 `discountCodes` ～に道を開く `App.js`{#add-widgets-route}

メインの React コンポーネント内 `App.js`、 `discountCodes` 上記の相対 URL パスの UI をレンダリングするルート。

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

### 作成 `DiscountCodes` React コンポーネント{#create-widget-react-component}

ウィジェットまたはダイアログ UI は、 [AdobeReact スペクトル](https://react-spectrum.adobe.com/react-spectrum/index.html) フレームワーク。 この `DiscountCodes` コンポーネントコードの主な特徴を次に示します。

+ UI は、次のような React Spectrum コンポーネントを使用してレンダリングされます。 [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [ボタン](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ この `adventureDiscountCodes` 配列には、アドベンチャー名と割引コードのハードコードマッピングが含まれています。 実際のシナリオでは、このデータは、Adobeの AppBuilder アクションや、PIM、OMS、自宅開発、クラウドプロバイダーベースの API ゲートウェイなどの外部システムから取得できます。
+ この `guestConnection` は `useEffect` [React フック](https://react.dev/reference/react/useEffect) コンポーネントの状態として管理されます。 AEMホストとの通信に使用されます。
+ この `handleDiscountCodeChange` 関数は、選択されたアドベンチャー名の割引コードを取得し、state 変数を更新します。
+ この `addDiscountCode` 関数 `guestConnection` オブジェクトは、実行する RTE 命令を提供します。 この場合、 `insertContent` RTE に挿入する実際の割引コードの命令およびHTMLコードスニペット。

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
