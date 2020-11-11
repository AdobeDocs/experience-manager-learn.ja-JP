---
title: クライアントライブラリの作成
description: 「保存して終了」ボタンのクリックイベントを処理するクライアントライブラリを作成します
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 6%

---

# クライアントライブラリの作成

CSSクラスの [savebuttonで識別されるボタンのclickイベントで](https://docs.adobe.com/content/help/ja-JP/experience-manager-65/developing/introduction/clientlibs.html) APIのメソッド `doAjaxSubmitWithFileAttachment` を呼び出すコードを含むクライアントlib `guideBridge` を作成します ****。  アダプティブフォームデータ `fileMap`と、をリッスンしているエンドポイント `mobileNumber` に渡します。 `**/bin/storeafdatawithattachments`

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
> ダイアログボックスの表示に [ブートボックスのJavaScriptライブラリ](http://bootboxjs.com/examples.html) を使用しました。

このサンプルで使用するクライアントライブラリは、こちらから [ダウンロードできます。](assets/client-libraries.zip)
