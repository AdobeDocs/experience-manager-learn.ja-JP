---
title: クライアントライブラリの作成
description: 「保存して終了」ボタンのクリックイベントを処理するクライアントライブラリを作成します
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: 開発
role: デベロッパー
level: 中間
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 8%

---

# クライアントライブラリの作成

CSSクラス&#x200B;**savebutton**&#x200B;で識別されるボタンのクリックイベントで、`guideBridge` APIのメソッド`doAjaxSubmitWithFileAttachment`を呼び出すコードを含む[クライアントlib](https://docs.adobe.com/content/help/ja-JP/experience-manager-65/developing/introduction/clientlibs.html)を作成します。  アダプティブフォームデータ`fileMap`と`mobileNumber`を`**/bin/storeafdatawithattachments`でリッスンしているエンドポイントに渡します

フォームデータを保存すると、一意のアプリケーションIDが生成され、ダイアログボックスにユーザーに表示されます。 ダイアログボックスを閉じると、ユーザーはフォームに移動し、保存済みのアダプティブフォームを一意のアプリケーションIDを使用して取得できるようになります。

```java
$(document).ready(function () {
  
  $(".savebutton").click(function () {
    var tel = guideBridge.resolveNode(
      "guide[0].guide1[0].guideRootPanel[0].contactInformation[0].basicContact[0].telephoneNumber[0]"
    );
    var telephoneNumber = tel.value;
    guideBridge.getFormDataString({
      success: function (data) {
        var map = guideBridge._getFileAttachmentMapForSubmit();
        guideBridge.doAjaxSubmitWithFileAttachment(
          "/bin/storeafdatawithattachments",
          {
            data: data.data,
            fileMap: map,
            mobileNumber: telephoneNumber,
          },
          {
            success: function (x) {
              bootbox.alert(
                "This is your reference number.<br>" +
                  x.data.path +
                  " <br>You will need this to retrieve your application",
                function () {
                  console.log(
                    "This was logged in the callback! After the ok button was pressed"
                  );
                  window.location.href =
                    "http://localhost:4502/content/dam/formsanddocuments/myaccountform/jcr:content?wcmmode=disabled";
                }
              );
              console.log(x.data.path);
            },
          },
          guideBridge._getFileAttachmentsList()
        );
      },
    });
  });
});
```

>[!NOTE]
> [ブートボックスjavascriptライブラリ](http://bootboxjs.com/examples.html)を使用してダイアログボックスを表示しました

このサンプルで使用するクライアントライブラリは、[ここから](assets/client-libraries.zip)ダウンロードできます。
