---
title: AEM Content Fragment Console 拡張機能モーダル
description: AEMコンテンツフラグメントコンソール拡張モーダルを作成する方法について説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---


# 拡張モーダル

![AEM Content Fragment 拡張機能モーダル](./assets/modal/modal.png){align="center"}

AEMコンテンツフラグメント拡張モーダルは、カスタム UI をAEMコンテンツフラグメント拡張機能にアタッチする方法を提供します。 [アクションバー](./action-bar.md) または [ヘッダーメニュー](./header-menu.md) ボタン

モデルは、 [React スペクトル](https://react-spectrum.adobe.com/react-spectrum/)を使用すれば、拡張機能に必要な任意のカスタム UI を作成できます。これには以下が含まれますが、これらに限定されません。

+ 確認ダイアログ
+ [入力フォーム](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [進行状況指標](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [結果の概要](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ エラーメッセージ
+ ...またはフルブローのマルチビュー React アプリケーションも。

## モーダルルート

モーダルエクスペリエンスは、 `web-src` フォルダー。 他の React アプリと同様に、フルエクスペリエンスは [React ルート](https://reactrouter.com/en/main/components/routes) レンダリング [React コンポーネント](https://reactjs.org/docs/components-and-props.html).

最初のモーダルビューを生成するには、少なくとも 1 つのルートが必要です。 この初期ルートは、 [拡張登録](#extension-registration)&#39;s `onClick(..)` 関数に含めることができます。


+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

```javascript
import MyModal from './MyModal';
import MyModalResults from './MyModalResults';
...
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          ...         
          {/* 
            Define the entry route to the modal.

            For modals opened from Action Bar extensions, typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="content-fragment/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="content-fragment/my-modal"
            element={<MyOtherModalView />}
          />                    

        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## 拡張機能の登録

モーダルを開くには、を呼び出します。 `guestConnection.host.modal.showUrl(..)` は、拡張機能の `onClick(..)` 関数に置き換えます。 `showUrl(..)` は、キーと値を持つ JavaScript オブジェクトを渡します。

+ `title` ユーザーに表示されるモーダルのタイトルの名前を指定します
+ `url` は、 [React ルート](#modal-routes) は、モーダルの初期表示に対して使用されます。

これは必ず `url` に渡される `guestConnection.host.modal.showUrl(..)` は拡張機能でルーティングするように解決され、それ以外の場合は、モーダルには何も表示されません。

以下を確認します。 [ヘッダーメニュー](./header-menu.md#modal) および [アクションバー](./action-bar.md#modal) モーダル URL の作成方法に関するドキュメントです。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-modal";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

## モーダルコンポーネント

拡張機能の各ルート [それは `index` ルート](./extension-registration.md#app-routes)は、拡張機能のモーダルでレンダリングできる React コンポーネントにマッピングされます。

モーダルは、単純な 1 ルートモーダルから複雑なマルチルートモーダルまで、任意の数の React ルートで構成することができます。

次に、単純な 1 ルートモーダルを示します。ただし、このモーダルビューには、他のルートや動作を呼び出す React リンクを含めることができます。

+ `./src/aem-cf-console-admin-1/web-src/src/components/MyModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Provider,
  Content,
  defaultTheme,
  Text,
  ButtonGroup,
  Button
} from '@adobe/react-spectrum'
import Spinner from "./Spinner"
import { useParams } from "react-router-dom"
import { extensionId } from "./Constants"

export default function MyModal() {
  // Initial modal views for Action Bar extensions typically pass in the list of selected Content Fragment Paths from ExtensionRegistration.js
  // Get the paths from useParams() and split on delimiter used
  let { selection } = useParams();
  let contentFragmentPaths = selection?.split('|') || [];
  
  // Asynchronously attach the extension to AEM. 
  // Wait or the guestConnection to be set before doing anything in the modal.
  const [guestConnection, setGuestConnection] = useState()

  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])

  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else {
    // Else the modal is ready to render!
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
        {/* 
            Use the React Spectrum components to render the modal UI.
            Using React Spectrum ensures a consistent, accessible, future-proof look-and-feel and speeds up development.
        */}
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        This is the contents in the modal! 
                        Anything can be created in this return statement!

                        The selected Content Fragments are: { contentFragmentPaths.join(', ') }
                    </Text>                    
                    {/*
                        Modals must provide their own Close button, by calling: guestConnection.host.modal.close()
                    */}
                    <ButtonGroup align="end">
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  }
}
```

## モーダルを閉じる

![AEMコンテンツフラグメント拡張モーダル閉じるボタン](./assets/modal/close.png){align="center"}

モデルは、独自のクローズコントロールを提供する必要があります。 これは、を呼び出すことでおこなわれます。 `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
