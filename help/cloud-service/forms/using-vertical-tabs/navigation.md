---
title: AEM Forms Cloud Serviceでの垂直タブの使用
description: 垂直タブを使用したアダプティブフォームの作成
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16023
source-git-commit: f3f5c4c4349c8d02c88e1cf91dbf18f58db1e67e
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 5%

---

# タブ間の移動

個々のタブをクリックするか、フォームの「前へ」ボタンと「次へ」ボタンを使用して、タブ間を移動できます。
ボタンを使用して移動するには、フォームに 2 つのボタンを追加し、それらに Previous と Next という名前を付けます。 次のカスタム関数をボタンのクリックイベントに関連付けて、タブ間を移動します。

次に、タブ間を移動するためのカスタム関数を示します。



```javascript
/**
 * Navigate in panel with focusOption
 * @name navigateInPanelWithFocusOption
 * @param {object} panelField
 * @param {string} focusOption - values can be 'nextItem'/'previousItem'
 * @param {scope} globals
 */
function navigateInPanelWithFocusOption(panelField, focusOption, globals)
{
    globals.functions.setFocus(panelField, focusOption);
}
```

次に、「次へ」および「前へ」ボタンのルールエディターを示します

**次へボタン**

![next-button](assets/next-button.png)

**前へボタン**

![prev-button](assets/prev-button.png)

