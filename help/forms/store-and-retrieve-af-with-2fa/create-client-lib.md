---
title: クライアントライブラリの作成
description: 「保存して終了」ボタンのクリックイベントを処理するクライアントライブラリを作成する
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: 開発
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 8%

---

# クライアントライブラリの作成

[クライアントlib](https://docs.adobe.com/content/help/ja-JP/experience-manager-65/developing/introduction/clientlibs.html)を作成します。このコードには、CSSクラス&#x200B;**savebutton**&#x200B;で識別されるボタンのclickイベントで`guideBridge` APIのメソッド`doAjaxSubmitWithFileAttachment`を呼び出すコードが含まれます。  アダプティブフォームのデータ`fileMap`と`mobileNumber`を、`**/bin/storeafdatawithattachments`をリッスンするエンドポイントに渡します。

フォームデータを保存すると、一意のアプリケーションIDが生成され、ダイアログボックスに表示されます。 ダイアログボックスを閉じると、ユーザーは、一意のアプリケーションIDを使用して保存済みのアダプティブフォームを取得できるフォームに移動します。

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
> [bootbox javascript library](http://bootboxjs.com/examples.html)を使用してダイアログボックスを表示しています

このサンプルで使用されるクライアントライブラリは、[こちら](assets/client-libraries.zip)からダウンロードできます。
