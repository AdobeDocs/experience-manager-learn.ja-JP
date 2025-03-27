---
title: AEM Forms as a Cloud Service での垂直タブの使用
description: 垂直タブを使用したアダプティブフォームの作成
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16023
exl-id: c5bbd35e-fd15-4293-901e-c81faf6025f9
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '111'
ht-degree: 100%

---

# タブ間の移動

個々のタブをクリックするか、フォームの「前へ」ボタンと「次へ」ボタンを使用して、タブ間を移動できます。
ボタンを使用して移動するには、フォームに 2 つのボタンを追加し、「前へ」と「次へ」という名前を付けます。タブ間を移動するには、ボタンのクリックイベントに次のカスタム関数を関連付けます。

次に、タブ間を移動するカスタム関数を示します。



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

次に、「次へ」および「前へ」ボタンのルールエディターを示します。

**「次へ」ボタン**

![next-button](assets/next-button.png)

**「前へ」ボタン**

![prev-button](assets/prev-button.png)
