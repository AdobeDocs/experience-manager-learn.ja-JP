---
title: MyAccountFormの作成
description: 申し込みIDと電話番号の検証に成功した場合に、部分的に記入されたフォームを取得するためのmyaccountフォームを作成します。
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: 開発
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 1%

---



# MyAccountFormの作成

フォーム&#x200B;**MyAccountForm**&#x200B;は、ユーザーがアプリケーションIDと、アプリケーションIDに関連付けられているモバイル番号を確認した後に、部分的に完了したアダプティブフォームを取得するために使用されます。

![マイアカウントフォーム](assets/6599.JPG)

ユーザーがアプリケーションIDを入力して「**FetchApplication**」ボタンをクリックすると、フォームデータモデルのGet操作を使用して、アプリケーションIDに関連付けられたモバイル番号がデータベースから取得されます。

このフォームは、フォームデータモデルのPOST呼び出しを利用して、OTPを使用してモバイル番号を検証します。 フォームの送信アクションは、次のコードを使用して、携帯電話番号の検証が正常に完了するとトリガーされます。 送信ボタン&#x200B;**submitForm**&#x200B;のclickイベントを発生させます。

>[!NOTE]
> MyAccountFormの適切なフィールドに、[Nexmo](https://dashboard.nexmo.com/)アカウントに固有のAPIキーとAPI秘密鍵の値を指定する必要があります

![トリガー送信](assets/trigger-submit.JPG)



このフォームは、**/bin/renderaf**&#x200B;にマウントされたサーブレットにフォーム送信を転送するカスタム送信アクションに関連付けられています

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

**/bin/renderaf**&#x200B;にマウントされたサーブレット内のコードは、保存されたデータが事前に入力されたstoreafwithattachmentsアダプティブフォームをレンダリングする要求を転送します。


* MyAccountFormは、[ここからダウンロードできます。](assets/my-account-form.zip)

* サンプルフォームは、サンプルフォームが正しくレンダリングされるためにAEMに読み込む必要がある、[カスタムアダプティブフォームテンプレート](assets/custom-template-with-page-component.zip)に基づいています。

* [MyAccountForm送信に関](assets/custom-submit-my-account-form.zip) 連付けられたカスタム送信ハンドラーは、AEMに読み込む必要があります。
