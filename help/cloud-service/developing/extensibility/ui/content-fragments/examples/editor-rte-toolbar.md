---
title: リッチテキストエディター (RTE) ツールバーへのカスタムボタンの追加
description: AEMコンテンツフラグメントエディターでリッチテキストエディター (RTE) ツールバーにカスタムボタンを追加する方法を説明します。
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 2b72c282-bce8-4f2a-bce6-f2f31e96ec88
source-git-commit: 6f537a0c7605b96f6c6b43ff8c5bf634369171cc
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 3%

---

# リッチテキストエディター (RTE) ツールバーへのカスタムボタンの追加

>[!VIDEO](https://video.tv.adobe.com/v/3420768?quality=12&learn=on)

カスタムボタンを **RTE ツールバー** を使用してコンテンツフラグメントエディターで `rte` 拡張ポイント。 この例では、 _ヒントを追加_ を RTE ツールバーに追加し、RTE 内のコンテンツを変更します。

使用 `rte` 拡張ポイント `getCustomButtons()` メソッド 1 つまたは複数のカスタムボタンを **RTE ツールバー**. また、次のような標準の RTE ボタンを追加または削除することもできます。 _コピー、貼り付け、太字、斜体_ using `getCoreButtons()` および `removeButtons)` 各メソッドで使用できます。

この例では、ハイライト表示された注記やヒントを、カスタム _ヒントを追加_ ツールバーボタン 強調表示されたメモまたはヒントのコンテンツには、HTML要素を介して適用された特別な書式と、関連する CSS クラスがあります。 プレースホルダーコンテンツとHTMLコードは、 `onClick()` のコールバックメソッド `getCustomButtons()`.

## 拡張ポイント

この例は、延長点まで延長されます `rte` をクリックして、コンテンツフラグメントエディターの RTE ツールバーにカスタムボタンを追加します。

| AEM UI 拡張 | 拡張ポイント |
| ------------------------ | --------------------- | 
| [コンテンツフラグメントエディター](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [リッチテキストエディターのツールバー](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## 拡張機能の例

次の例では、 _ヒントを追加_ RTE ツールバーのカスタムボタン。 クリック操作により、RTE の現在のキャレット位置にプレースホルダーテキストが挿入されます。

このコードでは、カスタムボタンをアイコンと共に追加し、クリックハンドラー関数を登録する方法を示します。

### 拡張機能の登録

`ExtensionRegistration.js`は、index.html ルートにマッピングされ、AEM拡張機能のエントリポイントで、次の項目を定義します。

+ RTE ツールバーボタンの定義 ( `getCustomButtons()` ～と機能する `id, tooltip and icon` 属性。
+ `onClick()` 関数内のボタンのクリックハンドラー。
+ クリックハンドラー関数は、 `state` オブジェクトを引数として使用して、RTE のコンテンツをHTMLまたはテキスト形式で取得します。 ただし、この例では使用されていません。
+ クリックハンドラー関数は、命令配列を返します。 この配列には、 `type` および `value` 属性。 コンテンツを挿入するには、 `value` 属性HTMLコードスニペット、 `type` 属性は `insertContent`. コンテンツを置き換える使用例がある場合は、 `replaceContent` 命令タイプ。

この `insertContent` 値はHTML文字列 `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`. CSS クラス `cmp-contentfragment__element-tip` 値の表示に使用されるのは、ウィジェットで定義されているのではなく、この「コンテンツフラグメント」フィールドが表示される web エクスペリエンスに実装されます。


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

          // RTE Toolbar custom button
          getCustomButtons: () => ([
            {
              id: "wknd-cf-tip",       // Provide a unique ID for the custom button
              tooltip: "Add Tip",      // Provide a label for the custom button
              icon: 'Note',            // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {    // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [{
                  type: "insertContent",
                  value: "<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>"
                }];
              },
            },
          ]),
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```
