---
title: リッチテキストエディター（RTE）へのバッジの追加
description: AEM コンテンツフラグメントエディターのリッチテキストエディター（RTE）にバッジを追加する方法を説明します。
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 83acbddb-9168-4d8b-84b5-97577d8a1ead
duration: 538
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 100%

---

# リッチテキストエディター（RTE）へのバッジの追加

AEM コンテンツフラグメントエディターのリッチテキストエディター（RTE）にバッジを追加する方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

[リッチテキストエディターバッジ](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/)は、リッチテキストエディター（RTE）のテキストを編集不可にする拡張機能です。つまり、そのように宣言されたバッジは完全に削除することのみが可能で、部分的に編集することはできません。 これらのバッジは、RTE 内の特別な色付けもサポートしており、テキストがバッジであるため、編集できないことをコンテンツ作成者に明確に示します。さらに、バッジのテキストの意味に関する視覚的なヒントも示します。

RTE バッジの最も一般的なユースケースでは、[RTE ウィジェット](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/)と組み合わせて使用します。これにより、RTE ウィジェットによって RTE に挿入されたコンテンツを編集できなくなります。

通常、ウィジェットに関連付けられたバッジは、外部システムに依存関係がある動的コンテンツを追加するために使用しますが、挿入した動的コンテンツを&#x200B;_コンテンツ作成者が変更_&#x200B;して整合性を維持することはできません。コンテンツ作成者は、項目全体としてのみ削除できます。

**バッジ**&#x200B;は、`rte` 拡張ポイントを使用して、コンテンツフラグメントエディターの **RTE** に追加します。`rte` 拡張ポイントの `getBadges()` メソッドを使用して、1 つまたは複数のバッジを追加します。

この例では、_大規模グループ予約カスタマーサービス_&#x200B;というウィジェットを追加して、RTE コンテンツ内で&#x200B;**担当者名**&#x200B;や&#x200B;**電話番号**&#x200B;など、WKND アドベンチャー固有のカスタマーサービスの詳細を検索、選択、追加する方法を示します。バッジ機能を使用すると、**電話番号**&#x200B;は&#x200B;**編集不可**&#x200B;になりますが、WKND コンテンツ作成者は担当者名を編集できます。

また、**電話番号**&#x200B;のスタイルが異なります（青）。これは、バッジ機能の追加のユースケースです。

作業をシンプルにするために、この例では [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) フレームワークを使用して、ウィジェットまたはダイアログ UI とハードコードされた WKND カスタマーサービスの電話番号を開発します。コンテンツの非編集および異なるスタイル要素を制御するには、バッジ定義の `prefix` および `suffix` 属性で `#` 文字を使用します。

## 拡張ポイント

この例では、拡張ポイント `rte` まで拡張して、コンテンツフラグメントエディターの RTE にバッジを追加します。

| AEM UI 拡張 | 拡張ポイント |
| ------------------------ | --------------------- | 
| [コンテンツフラグメントエディター](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [リッチテキストエディターバッジ](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/)と[リッチテキストエディターウィジェット](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## 拡張機能の例

次の例では、_大規模グループ予約カスタマーサービス_&#x200B;ウィジェットを作成します。RTE 内で `{` キーを押すと、RTE ウィジェットのコンテキストメニューが開きます。コンテキストメニューから「_大規模グループ予約カスタマーサービス_」オプションを選択すると、カスタムモーダルが開きます。

目的のカスタマーサービス番号をモーダルから追加すると、バッジによって&#x200B;_電話番号が編集不可_&#x200B;になり、スタイルは青色になります。

### 拡張機能の登録

`index.html` ルートにマッピングされた `ExtensionRegistration.js` は、AEM 拡張機能のエントリポイントであり、以下を定義します。

+ バッジの定義は、設定属性の `id`、`prefix`、`suffix`、`backgroundColor`、`textColor` を使用して `getBadges()` で定義します。
+ この例では、`#` 文字を使用してこのバッジの境界を定義しています。つまり、`#` で囲まれた RTE 内の文字列はすべて、このバッジのインスタンスとして扱われます。

また、RTE ウィジェットの次の主な詳細も確認します。

+ `id`、`label`、`url` 属性を含む `getWidgets()` 関数のウィジェット定義。
+ `url` 属性値、モーダルを読み込む相対 URL パス（`/index.html#/largeBookingsCustomerService`）。


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
          // RTE Badges
          getBadges: () => [
            {
              id: "phoneNumber",
              prefix: "#",
              suffix: "#",
              backgroundColor: "",
              textColor: "#071DF8",
            },
          ],

          // RTE Widgets
          getWidgets: () => [
            {
              id: "largegroup-contact-list-widget",
              label: "Large Group Bookings Customer Service",
              url: "/index.html#/largeBookingsCustomerService",
            },
          ],
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### `App.js` への `largeBookingsCustomerService` ルートの追加{#add-widgets-route}

React のメインコンポーネント `App.js` に、`largeBookingsCustomerService` ルートを追加して、上記の相対 URL パスの UI をレンダリングします。

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

### `LargeBookingsCustomerService` React コンポーネントの作成{#create-widget-react-component}

ウィジェットまたはダイアログ UI は、[Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) フレームワークを使用して作成します。

React コンポーネントコードは、カスタマーサービスの詳細を追加する際に、電話番号変数を `#` 登録バッジ文字で囲んで、`#${phoneNumber}#` などのバッジに変換し、編集不可にします。

`LargeBookingsCustomerService` コードの主なハイライトを次に示します。

+ UI は、[ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html)、[ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html)、[Button](https://react-spectrum.adobe.com/react-spectrum/Button.html) などの React Spectrum コンポーネントを使用してレンダリングします。
+ `largeGroupCustomerServiceList` 配列には、担当者名と電話番号のハードコードされたマッピングが含まれています。実際のシナリオでは、このデータは Adobe AppBuilder アクション、外部システム、自社製またはクラウドプロバイダーベースの API ゲートウェイから取得できます。
+ `guestConnection` は、`useEffect` [React hook](https://react.dev/reference/react/useEffect) を使用して初期化し、コンポーネントの状態として管理します。これは、AEM ホストとの通信に使用します。
+ `handleCustomerServiceChange` 関数は、担当者名と電話番号を取得し、コンポーネントの状態変数を更新します。
+ `guestConnection` オブジェクトを使用する `addCustomerServiceDetails` 関数は、実行する RTE 命令を提供します。この場合は、`insertContent` 命令と HTML コードスニペットを使用します。
+ バッジを使用して&#x200B;**電話番号を編集不可**&#x200B;にするには、`...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>` のように、`phoneNumber` 変数の前後に `#` 特殊文字を追加します。

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
