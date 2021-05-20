---
title: インタラクティブ通信ドキュメントの配信 — WebチャネルAEM Forms
seo-title: インタラクティブ通信ドキュメントの配信 — WebチャネルAEM Forms
description: Eメール内のリンクを介したWebチャネルドキュメントの配信
seo-description: Eメール内のリンクを介したWebチャネルドキュメントの配信
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 1%

---


# Webチャネルドキュメントの電子メール配信

Webチャネルのインタラクティブ通信ドキュメントを定義し、テストしたら、受信者にWebチャネルドキュメントを配信する配信メカニズムが必要です。

この記事では、Webチャネルドキュメントの配信メカニズムとしてのEメールを見てみます。 受信者は、電子メールでWebチャネルドキュメントへのリンクを取得します。リンクをクリックすると、ユーザーは認証を求められ、Webチャネルドキュメントには、ログインしたユーザーに固有のデータが入力されます。

次のコードスニペットを見てみましょう。 このコードは、ユーザーがWebチャネルドキュメントを表示する電子メール内のリンクのをクリックするとトリガーされるGET.jspの一部です。 jackrabbit UserManagerを使用してログインユーザーを取得します。 ログインユーザーを取得したら、ユーザーのプロファイルに関連付けられているaccountNumberプロパティの値を取得します。

次に、 accountNumber値をマップ内のaccountnumberというキーに関連付けます。 キー&#x200B;**accountnumber**&#x200B;は、フォームデータモーダルにリクエスト属性として定義されます。 この属性の値は、入力パラメーターとしてForm Data Modal読み取りサービスメソッドに渡されます。

7行目：インタラクティブ通信ドキュメントのURLで識別されるリソースタイプに基づいて、受信した要求を別のサーブレットに送信します。 この2番目のサーブレットによって返された応答は、1番目のサーブレットの応答に含まれます。

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![includemethod](assets/includemethod.jpg)

7行目のコードの視覚表現

![requestparameter](assets/requestparameter.png)

フォームデータモーダルの読み取りサービス用に定義された要求属性


[サンプルのAEMパッケージ](assets/webchanneldelivery.zip)を参照してください。
