---
title: リッチテキストエディター (RTE) へのバッジの追加
description: AEMコンテンツフラグメントエディターでリッチテキストエディター (RTE) にバッジを追加する方法を説明します。
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
source-git-commit: c965d5ff3f49f4859779e657674dab8602fb831b
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 2%

---


# リッチテキストエディター (RTE) へのバッジの追加

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

[リッチテキストエディターバッジ](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/)  は、リッチテキストエディター (RTE) 内のテキストを編集不可にする拡張機能です。 つまり、バッジとして宣言されたバッジは完全にのみ削除でき、部分的には編集できません。 また、RTE 内で特別な色付けをサポートしているので、コンテンツ作成者に対して、テキストがバッジであり、編集できないことを明確に示します。 また、バッジテキストの意味に関する視覚的な手掛かりを提供します。

RTE バッジの最も一般的な使用例は、バッジを [RTE ウィジェット](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/). これにより、RTE ウィジェットによって RTE に挿入されたコンテンツを編集できなくなります。

通常、ウィジェットと関連付けられたバッジは、外部のシステム依存関係を持つ動的コンテンツを追加するために使用されますが、 _コンテンツ作成者が_ 整合性を維持するために挿入された動的コンテンツ。 削除できるのは、項目全体のみです。

この **バッジ** が **RTE** を使用してコンテンツフラグメントエディターで `rte` 拡張ポイント。 使用 `rte` 拡張ポイント `getBadges()` メソッド：1 つ以上のバッジを追加します。

この例では、 _大きなグループの予約顧客サービス_ WKND アドベンチャー専用のカスタマーサービスの詳細を検索、選択および追加するには、次のようにします。 **担当者名** および **電話番号** を設定します。 バッジ機能の使用 **電話番号** 作成済み **編集不可** ただし、WKND コンテンツ作成者は、代表名を編集できます。

また、 **電話番号** のスタイルが異なる（青）ので、バッジ機能の追加の使用例です。

話を簡単にするために、この例では [AdobeReact スペクトル](https://react-spectrum.adobe.com/react-spectrum/index.html) ウィジェットまたはダイアログ UI とハードコードされた WKND カスタマーサービスの電話番号を開発するためのフレームワーク。 コンテンツの非編集および異なるスタイル要素を制御するには、 `#` 文字が `prefix` および `suffix` バッジ定義の属性。

## 拡張ポイント

この例は、延長点まで延長されます `rte` をクリックして、コンテンツフラグメントエディターで RTE にバッジを追加します。

| AEM UI 拡張 | 拡張ポイント |
| ------------------------ | --------------------- | 
| [コンテンツフラグメントエディター](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [リッチテキストエディターバッジ](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) および [リッチテキストエディターウィジェット](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## 拡張機能の例

次の例では、 _大きなグループの予約顧客サービス_ ウィジェット。 を押して、 `{` キーを押すと、RTE ウィジェットのコンテキストメニューが開きます。 次を選択すると、 _大きなグループの予約顧客サービス_ オプションを選択すると、カスタムモーダルが開きます。

目的のカスタマーサービス番号をモーダルから追加すると、バッジによって _電話番号を編集できません_ 青い色でスタイルを設定します。

### 拡張機能の登録

`index.html` ルートにマッピングされた `ExtensionRegistration.js` は、AEM 拡張機能のエントリポイントであり、以下を定義します。

+ バッジの定義は、 `getBadges()` 設定属性の使用 `id`, `prefix`, `suffix`, `backgroundColor` および `textColor`.
+ この例では、 `#` 文字は、このバッジの境界を定義するために使用されます。つまり、RTE 内の、 `#` は、このバッジのインスタンスとして扱われます。

また、RTE ウィジェットの主な詳細も参照してください。

+ のウィジェット定義 `getWidgets()` ～と機能する `id`, `label` および `url` 属性。
+ この `url` 属性値、相対 URL パス (`/index.html#/largeBookingsCustomerService`) をクリックして、モーダルを読み込みます。


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

          // RTE Badges
          getBadges: () => [
            {
              id: "phoneNumber",                    // Provide a unique ID for the badge
              prefix: "#",                          // Provide a Badge starting character
              suffix: "#",                          // Provide a Badge ending character
              backgroundColor: "",                  // Provide HEX or text CSS color code for the background
              textColor: "#071DF8"                  // Provide HEX or text CSS color code for the text
            }
          ],

          // RTE Widgets
          getWidgets: () => [
            {
              id: "largegroup-contact-list-widget",       // Provide a unique ID for the widget
              label: "Large Group Bookings Customer Service",          // Provide a label for the widget
              url: "/index.html#/largeBookingsCustomerService",     // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/largeBookingsCustomerService`
            },
          ],
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```

### 追加 `largeBookingsCustomerService` ～に道を開く `App.js`{#add-widgets-route}

メインの React コンポーネント内 `App.js`、 `largeBookingsCustomerService` 上記の相対 URL パスの UI をレンダリングするルート。

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
  <Route path="/largeBookingsCustomerService" element={<LargeBookingsCustomerService />} />
</Routes>
...
```

### 作成 `LargeBookingsCustomerService` React コンポーネント{#create-widget-react-component}

ウィジェットまたはダイアログ UI は、 [AdobeReact スペクトル](https://react-spectrum.adobe.com/react-spectrum/index.html) フレームワーク。

カスタマーサービスの詳細を追加する際に React コンポーネントコードは、電話番号変数を `#` バッジをバッジに変換する登録済みのバッジ文字（例： ） `#${phoneNumber}#`したがって、編集不可能にします。

主な特徴を次に示します。 `LargeBookingsCustomerService` コード：

+ UI は、次のような React Spectrum コンポーネントを使用してレンダリングされます。 [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [ボタン](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ この `largeGroupCustomerServiceList` 配列には、代表名と電話番号のハードコードされたマッピングが含まれています。 実際のシナリオでは、このデータは、Adobeの AppBuilder アクション、外部システム、または自宅で開発した、またはクラウドプロバイダーベースの API ゲートウェイから取得できます。
+ この `guestConnection` は `useEffect` [React フック](https://react.dev/reference/react/useEffect) コンポーネントの状態として管理されます。 AEMホストとの通信に使用されます。
+ この `handleCustomerServiceChange` 関数は、代表名と電話番号を取得し、コンポーネント状態変数を更新します。
+ この `addCustomerServiceDetails` 関数 `guestConnection` オブジェクトは、実行する RTE 命令を提供します。 この場合、 `insertContent` 命令およびHTMLコードスニペット。
+ を **電話番号は編集できません** バッジを使用する場合、 `#` 特殊文字を前後に追加する `phoneNumber` 変数、例： `...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`.

`src/aem-cf-editor-1/web-src/src/components/LargeBookingsCustomerService.js`

```javascript
import {
  Button,
  ButtonGroup,
  Text,
  Divider,
  ComboBox,
  Content, Flex, Form,
  Item,
  Provider, defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';


const LargeBookingsCustomerService = () => {

  // The Large Group Bookings Customer Service
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const largeGroupCustomerServiceList = [
    { id: 1, repName: 'Max', phoneNumber: '1-800-235-1000' },
    { id: 2, repName: 'John', phoneNumber: '1-700-235-2000' },
    { id: 3, repName: 'Leah', phoneNumber: '1-600-235-3000' },
    { id: 4, repName: 'Leno', phoneNumber: '1-500-235-4000' }
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [repName, setRepName] = useState(null);
  const [phoneNumber, setPhoneNumber] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `customerService` Dropdown change
  const handleCustomerServiceChange = (id) => {

    if (id) {
      //Get Customer Service RepName and Phone Number values using selected id

      const rep = largeGroupCustomerServiceList.filter((r) => r.id === id)[0];

      //update the `repName` state
      setRepName(rep?.repName);

      //update the `phoneNumber` state
      setPhoneNumber(rep?.phoneNumber);
    }
  };

  // Add the selected Customer Service details into the RTE
  const addCustomerServiceDetails = () => {

    if (repName && phoneNumber) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<div><p>Representative Name: <strong>${repName}</strong></p></div><div><p>Phone Number: #${phoneNumber}#</strong></p></div>` }]);
    }
  };


  // Adobe React Spectrum (HTML code) that renders the Customer Service dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">
            <Text>Representative Name: <strong>{repName}</strong></Text>
            <Text>Phone Number: <strong>{phoneNumber}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="customerService"
              label="Type or Select Phone Number"
              defaultItems={largeGroupCustomerServiceList}
              onSelectionChange={handleCustomerServiceChange}>
              {item => <Item>{item.phoneNumber}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addCustomerServiceDetails}>Add</Button>
              <Button variant="secondary" onPress={() => {setPhoneNumber(null); setRepName(null);}}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );

};

export default LargeBookingsCustomerService;
```
