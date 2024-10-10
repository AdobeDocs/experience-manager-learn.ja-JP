---
title: リッチテキストエディター（RTE）ツールバーへのカスタムボタンの追加
description: AEM コンテンツフラグメントエディターのリッチテキストエディター（RTE）ツールバーにカスタムボタンを追加する方法を説明します。
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 6fd93d3b-6d56-43c5-86e6-2e2685deecc9
duration: 345
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '406'
ht-degree: 100%

---

# リッチテキストエディター（RTE）ツールバーへのカスタムボタンの追加

AEM コンテンツフラグメントエディターのリッチテキストエディター（RTE）ツールバーにカスタムボタンを追加する方法を学びます。

>[!VIDEO](https://video.tv.adobe.com/v/3420768?quality=12&learn=on)

`rte` 拡張ポイントを使用すると、コンテンツフラグメントエディターの **RTE ツールバー**&#x200B;にカスタムボタンを追加できます。この例では、「_ヒントを追加_」というカスタムボタンを RTE ツールバーに追加し、RTE 内のコンテンツを変更する方法を示します。

`rte` 拡張ポイントの `getCustomButtons()` メソッドを使用すると、1 つまたは複数のカスタムボタンを **RTE ツールバー**&#x200B;に追加できます。また、`getCoreButtons()` メソッドと `removeButtons)` メソッドをそれぞれ使用して、_コピー、ペースト、太字、斜体_&#x200B;などの標準 RTE ボタンを追加または削除することもできます。

この例では、カスタムの「_ヒントを追加_」ツールバーボタンを使用して、ハイライト表示されたメモまたはヒントを挿入する方法を示します。ハイライト表示されたメモまたはヒントのコンテンツには、HTML 要素および関連する CSS クラスを介して特別な書式設定が適用されます。プレースホルダーのコンテンツと HTML コードは、`getCustomButtons()` の `onClick()` コールバックメソッドを使用して挿入します。

## 拡張ポイント

この例では、拡張ポイント `rte` まで拡張して、コンテンツフラグメントエディターの RTE ツールバーにカスタムボタンを追加します。

| AEM UI 拡張 | 拡張ポイント |
| ------------------------ | --------------------- | 
| [コンテンツフラグメントエディター](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [リッチテキストエディターツールバー](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## 拡張機能の例

次の例では、RTE ツールバーに「_ヒントを追加_」カスタムボタンを作成します。ククリックアクションにより、RTE の現在のキャレット位置にプレースホルダーテキストが挿入されます。

このコードでは、アイコン付きのカスタムボタンを追加し、クリックハンドラー関数を登録する方法を示します。

### 拡張機能の登録

index.html ルートにマッピングされた `ExtensionRegistration.js` は、AEM 拡張機能のエントリポイントであり、次を定義します。

+ `id, tooltip and icon` 属性を含む `getCustomButtons()` 関数の RTE ツールバーボタンの定義。
+ `onClick()` 関数内のボタンのクリックハンドラー。
+ クリックハンドラー関数は、`state` オブジェクトを引数として受け取り、RTE のコンテンツを HTML またはテキスト形式で取得します。ただし、この例では使用しません。
+ クリックハンドラー関数は、命令配列を返します。この配列には、`type` 属性と `value` 属性を持つオブジェクトがあります。コンテンツを挿入するには、`value` 属性で HTML コードスニペットを使用し、`type` 属性で `insertContent` を使用します。コンテンツを置き換えるユースケースがある場合は、`replaceContent` 命令タイプを使用します。

`insertContent` 値は、HTML 文字列 `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>` です。値の表示に使用する CSS クラス `cmp-contentfragment__element-tip` は、ウィジェット内で定義されず、このコンテンツフラグメントフィールドを表示する web エクスペリエンスに実装されます。


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
          // RTE Toolbar custom button
          getCustomButtons: () => [
            {
              id: "wknd-cf-tip", // Provide a unique ID for the custom button
              tooltip: "Add Tip", // Provide a label for the custom button
              icon: "Note", // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {
                // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [
                  {
                    type: "insertContent",
                    value:
                      '<div class="cmp-contentfragment__element-tip"><div>TIP</div><div>Add your tip text here...</div></div>',
                  },
                ];
              },
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
