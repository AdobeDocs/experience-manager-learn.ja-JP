---
title: MyAccountForm の作成
description: アプリケーション ID と電話番号の検証に成功した際に、部分的に入力されたフォームを取得する myaccount フォームを作成します。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
duration: 53
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 100%

---

# MyAccountForm の作成

**MyAccountForm** は、ユーザーがアプリケーション ID とアプリケーション ID に関連付けられた携帯電話番号を確認した後、部分的に完成したアダプティブフォームを取得するために使用されます。

![マイアカウントフォーム](assets/6599.JPG)

ユーザーがアプリケーション ID を入力し、**FetchApplication** ボタンをクリックすると、フォームデータモデルの「Get」操作を使用して、アプリケーション id に関連付けられたモバイル番号がデータベースから取得されます。

このフォームは、フォームデータモデルの POST 呼び出しを利用して、OTP を使用して携帯電話番号を検証します。フォームの送信アクションは、次のコードを使用して、携帯電話番号の検証が成功したときにトリガーされます。**submitForm** という名前の送信ボタンのクリックイベントをトリガーしています。

>[!NOTE]
> MyAccountForm の該当フィールドに、[Nexmo](https://dashboard.nexmo.com/) アカウントに固有の API Key と API Secret の値を入力する必要があります。

![トリガー送信](assets/trigger-submit.JPG)



このフォームは、**/bin/renderaf** にマウントされたサーブレットにフォーム送信を転送するカスタム送信アクションと関連付けられています。

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

**/bin/renderaf** にマウントされたサーブレット内のコードは、保存されたデータを事前に入力した storeafwithattachments のアダプティブフォームを表示するためにリクエストを転送します。


* MyAccountForm は、[ここからダウンロード](assets/my-account-form.zip)できます

* サンプルフォームは[カスタムアダプティブフォームテンプレート](assets/custom-template-with-page-component.zip)サンプルフォームを正しくレンダリングするには、AEM に読み込む必要があります。

* MyAccountForm の送信に関連する[カスタム送信ハンドラー](assets/custom-submit-my-account-form.zip)を AEM に読み込む必要があります。

## 次の手順

[サンプルアセットをデプロイしてソリューションをテスト](./deploy-this-sample.md)
