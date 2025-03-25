---
title: クライアントライブラリの作成
description: 「保存して終了」ボタンのクリックイベントを処理するクライアントライブラリを作成します
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
duration: 42
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 100%

---

# クライアントライブラリの作成

CSS クラス **savebutton** によって識別されるボタンのクリックイベントで、`guideBridge` API のメソッド `doAjaxSubmitWithFileAttachment` を呼び出すコードを含む[クライアントライブラリ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ja)を作成します。`**/bin/storeafdatawithattachments` でリッスンしているエンドポイントに、アダプティブフォームデータ、`fileMap` および `mobileNumber` を渡します。

フォームデータを保存すると、一意のアプリケーション ID が生成され、ダイアログボックスでユーザーに表示されます。ダイアログボックスを閉じると、ユーザーはフォームに移動し、一意のアプリケーション ID を使用して、保存済みのアダプティブフォームを取得できます。

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
                  x.data.applicationID +
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
> ダイアログボックスを表示するために、[bootbox JavaScript ライブラリ](https://bootboxjs.com/examples.html)を使用しました

このサンプルで使用しているクライアントライブラリは、[こちらからダウンロード](assets/store-af-with-attachments-client-lib.zip)できます.

## 次の手順

[OTP サービスでのユーザーの検証](./verify-users-with-otp.md)
