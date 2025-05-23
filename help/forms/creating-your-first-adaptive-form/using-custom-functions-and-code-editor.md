---
title: 関数とコードエディターの使用
description: 関数とコードエディターを使用したビジネスルールの作成
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-4270
thumbnail: 22282.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 7b2a4075-bfdf-49f3-b507-34d86193bf64
duration: 460
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '140'
ht-degree: 100%

---

# カスタム関数とコードエディターの使用 {#using-functions-and-code-editor}

ここでは、カスタム関数とコードエディターを使用して、ビジネスルールを作成します。

このチュートリアルで既に、[カスタム関数](assets/client-libs-and-logo.zip)を使用して ClientLib をインストールしています。

通常、クライアントライブラリは CSS と JavaScript ファイルで構成されます。 このクライアントライブラリには、コンボボックス値を入力する関数を公開する javascript ファイルが含まれています。


## ドロップダウンリストに入力する関数 {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/326877?quality=12&learn=on&captions=jpn)

### パネルの概要タイトルを設定する {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/35439?quality=12&learn=on&captions=jpn)

#### パネルを検証する {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/35438?quality=12&learn=on&captions=jpn)

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

1 行目のコメントアウトを解除すると、ブラウザーウィンドウでコードをデバッグできます。

4 行目 - 現在のパネルを取得します

5 行目 - 現在のパネルを検証します。

9 行目 - エラーがない場合は、次のパネルに移動します

フォームをプレビューし、新しく有効にした機能をテストします。
