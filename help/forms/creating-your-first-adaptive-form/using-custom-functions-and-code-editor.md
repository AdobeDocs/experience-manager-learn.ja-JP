---
title: 関数とコードエディターの使用
description: 関数とコードエディターを使用したビジネスルールの作成
feature: アダプティブフォーム
version: 6.4,6.5
kt: 4270
thumbnail: 22282.jpg
topic: 開発
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 2%

---


# カスタム関数とコードエディターの使用 {#using-functions-and-code-editor}

ここでは、カスタム関数とコードエディターを使用してビジネスルールを作成します。

このチュートリアルでは、既にカスタム関数](assets/client-libs-and-logo.zip)を使用して[ClientLibをインストールしています。

通常、クライアントライブラリはCSSとJavaScriptファイルで構成されます。 このクライアントライブラリには、コンボボックス値を入力する関数を公開するJavaScriptファイルが含まれています。


## コンボボックスに入力する機能 {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### パネルの概要タイトルを設定 {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### 検証パネル {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=9&learn=on)

次に、パネルのフィールドを検証するためのコードを示します

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

4行目 — 現在のパネルを取得します

5行目 — 現在のパネルを検証します。

9行目 — エラーがない場合は、次のパネルに移動します。

フォームをプレビューし、新しく有効になった機能をテストします。
