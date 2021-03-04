---
title: MyAccountFormの作成
description: 申し込みIDと電話番号の検証が成功した場合に、部分的に記入済みのフォームを取得するためのmyaccountフォームを作成します。
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---



# MyAccountFormの作成

ユーザーが申込みIDと、申込みIDに関連付けられたモバイル番号を確認した後、フォーム&#x200B;**MyAccountForm**&#x200B;を使用して、部分的に完了したアダプティブフォームを取得します。

![マイアカウントフォーム](assets/6599.JPG)

ユーザーがアプリケーションIDを入力し、**FetchApplication**&#x200B;ボタンをクリックすると、フォームデータモデルのGet操作を使用して、アプリケーションIDに関連付けられたモバイル番号がデータベースから取得されます。

このフォームでは、Form Data ModelのPOST呼び出しを使用して、OTPを使用してモバイル番号を検証します。 次のコードを使用して、モバイル番号の検証が成功した場合に、フォームの送信アクションがトリガーされます。 **submitForm**&#x200B;という送信ボタンのクリックイベントをトリガーします。

>[!NOTE]
> [Nexmo](https://dashboard.nexmo.com/)アカウントに固有のAPIキーとAPIシークレットの値を、MyAccountFormの適切なフィールドに入力する必要があります

![トリガー送信](assets/trigger-submit.JPG)



このフォームは、**/bin/renderaf**&#x200B;にマウントされたサーブレットにフォーム送信を転送するカスタム送信アクションに関連付けられています

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

**/bin/renderaf**&#x200B;にマウントされているサーブレット内のコードは、保存されたデータが事前に入力されたアダプティブフォームstoreafwithattachmentsをレンダリングする要求を転送します。


* MyAccountFormは、[ここから](assets/my-account-form.zip)ダウンロードできます

* サンプルフォームは、サンプルフォームが正しくレンダリングされるためにAEMに読み込む必要がある、[カスタムアダプティブフォームテンプレート](assets/custom-template-with-page-component.zip)に基づいています。

* [MyAccountForm送信に関連付けられたカスタム送信](assets/custom-submit-my-account-form.zip) ハンドラをAEMにインポートする必要があります。
