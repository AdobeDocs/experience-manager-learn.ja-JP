---
title: インタラクティブ通信ドキュメントの配信 — Web チャネルAEM Forms
description: E メール内のリンク経由での Web チャネルドキュメントの配信
feature: Interactive Communication
audience: developer
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 50858100-3d0c-42bd-87b8-f626212669e2
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 0%

---

# Web チャネルドキュメントのメール配信

Web チャネルのインタラクティブ通信ドキュメントを定義してテストしたら、Web チャネルドキュメントを受信者に配信するための配信メカニズムが必要です。

この記事では、Web チャネルドキュメントの配信メカニズムとしての E メールを見てみます。 受信者は、電子メールで Web チャネルドキュメントへのリンクを取得します。リンクをクリックすると、ユーザーは認証を求められ、Web チャネルドキュメントには、ログインしたユーザーに固有のデータが入力されます。

次のコードスニペットを見てみましょう。 このコードは、ユーザーがメール内のリンク上のをクリックして Web チャネルドキュメントを表示する際にトリガーされる data.jsp の一部です。 Jackrabbit UserManager を使用してログインユーザーが取得されます。 ログインユーザーを取得したら、ユーザーのプロファイルに関連付けられている accountNumber プロパティの値を取得します。

次に、accountNumber 値をマップ内の accountnumber というキーに関連付けます。 キー **accountnumber** は、フォームデータモーダル内で要求属性として定義されます。 この属性の値は、入力パラメーターとしてフォームデータモーダル読み取りサービスメソッドに渡されます。

行 7:インタラクティブ通信ドキュメントの URL で識別されるリソースタイプに基づいて、受信したリクエストを別のサーブレットに送信します。 この 2 番目のサーブレットによって返された応答は、最初のサーブレットの応答に含まれます。

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![メソッドアプローチを含める](assets/includemethod.jpg)

7 行目のコードの視覚的表現

![リクエストパラメーターの設定](assets/requestparameter.png)

フォームデータモーダルの読み取りサービス用に定義された要求属性

[サンプルのAEMパッケージ](assets/webchanneldelivery.zip).
