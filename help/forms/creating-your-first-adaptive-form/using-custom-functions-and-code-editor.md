---
title: 関数とコードエディターの使用
seo-title: 関数とコードエディターの使用
description: 関数とコードエディターを使用したビジネスルールの作成
seo-description: 関数とコードエディターを使用したビジネスルールの作成
uuid: 578e91f8-0d93-4192-b7af-1579df2feaf8
feature: adaptive-forms
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.4,6.5
discoiquuid: f480ef3e-7e38-4a6b-a223-c102787aea7f
kt: 4270
thumbnail: 22282.jpg
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---


# カスタム関数とコードエディターの使用 {#using-functions-and-code-editor}

この部分では、カスタム関数とコードエディターを使用してビジネスルールを作成します。

このチュートリアルの前の [手順で、ClientLibをカスタム関数と共に既にインストール済みです](assets/client-libs-and-logo.zip) 。

通常、クライアントライブラリはCSSとJavaScriptファイルで構成されます。 このクライアントライブラリには、コンボボックスのリスト値を入力する関数を公開するjavascriptファイルが含まれています。


## ドロップダウンリストを入力する関数 {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### パネルの概要タイトルを設定 {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### 検証パネル {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=9&learn=on)

パネルのフィールドの検証に使用するコードを次に示します

```javascript
//debugger;
var errors =[];
var fields ="";
var currentPanel = guideBridge.getFocus({"focusOption": "navigablePanel"});
window.guideBridge.validate(errors,currentPanel);
console.log("The errors are "+ errors.length);
if(errors.length===0)
{
        window.guideBridge.setFocus(this.panel.somExpression, 'nextItem', true);
}
else
  {
    for(var i=0;i<errors.length;i++)
      {
        var fields = fields+guideBridge.resolveNode(errors[i].som).title+" , ";
      }
        window.confirm("Please fill out  "+fields.slice(0,-1)+ " fields");
  }
```

1行目のコメントを解除して、ブラウザーウィンドウでコードをデバッグできます。

4行目 — 現在のパネルを取得します。

5行目 — 現在のパネルを検証します。

9行目 — エラーがない場合は、次のパネルに移動します。

フォームをプレビューし、新しく有効になった機能をテストします。
