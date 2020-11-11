---
title: MyAccountFormの作成
description: 申し込みIDと電話番号の検証が成功した場合に、部分的に記入済みのフォームを取得するためのmyaccountフォームを作成します。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---



# MyAccountFormの作成

フォームMyAccountForm **** は、ユーザーがアプリケーションIDと、そのアプリケーションIDに関連付けられたモバイル番号を確認した後に、部分的に完了したアダプティブフォームを取得するために使用されます。

![マイアカウントフォーム](assets/6599.JPG)

ユーザーがアプリケーションIDを入力し、「FetchApplication **** 」ボタンをクリックすると、フォームデータモデルの「Get」操作を使用して、アプリケーションIDに関連付けられたモバイル番号がデータベースから取得されます。

このフォームでは、Form Data ModelのPOST呼び出しを使用して、OTPを使用してモバイル番号を検証します。 次のコードを使用して、モバイル番号の検証が成功した場合に、フォームの送信アクションがトリガーされます。 submitFormという名前の送信ボタンのクリックイベントをトリガし **ます**。

>[!NOTE]
> MyAccountFormの適切なフィールドに、ご使用の [Nexmo](https://dashboard.nexmo.com/) アカウントに固有のAPIキーとAPIシークレットの値を指定する必要があります

![trigger-submit](assets/trigger-submit.JPG)



このフォームは、 **/bin/renderafにマウントされたサーブレットにフォーム送信を転送するカスタム送信アクションに関連付けられています**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

/bin/renderafにマウントされているサーブレット内のコードは、保存されたデータが事前に入力されたアダプティブフォームstoreafwithattachmentsをレンダリングするためのリクエストを転送します。 ****


* MyAccountFormは、こちらから [ダウンロードできます](assets/my-account-form.zip)

* サンプルフォームは、 [カスタムのアダプティブフォームテンプレートに基づいており](assets/custom-template-with-page-component.zip) 、サンプルフォームが正しくレンダリングされるためにはAEMに読み込む必要があります。

* [MyAccountForm送信に関連付けられたカスタム送信ハンドラー](assets/custom-submit-my-account-form.zip) 、AEMにインポートする必要があります。
