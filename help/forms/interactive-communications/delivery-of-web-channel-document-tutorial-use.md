---
title: インタラクティブ通信ドキュメントの配信 - Web チャネル AEM Forms
description: メール内のリンク経由での web チャネルドキュメントの配信
feature: Interactive Communication
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 50858100-3d0c-42bd-87b8-f626212669e2
last-substantial-update: 2019-07-07T00:00:00Z
duration: 60
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 100%

---

# Web チャネルドキュメントのメール配信

Web チャネルのインタラクティブ通信ドキュメントを定義しテストしたら、web チャネルドキュメントを受信者に配信するための配信メカニズムが必要になります。

この記事では、web チャネルドキュメントの配信メカニズムとしてのメールを見ていきます。 受信者は、メールで web チャネルドキュメントへのリンクを取得します。リンクをクリックすると、ユーザーは認証を求められ、web チャネルドキュメントには、ログインしたユーザーに固有のデータが入力されます。

次のコードスニペットを見てみましょう。 このコードは、ユーザーがメール内のリンクをクリックして web チャネルドキュメントを表示する際にトリガーされる GET.jsp の一部です。jackrabbit UserManager を使用してログインユーザーが取得されます。 ログインユーザーを取得したら、ユーザーのプロファイルに関連付けられている accountNumber プロパティの値を取得します。

次に、accountNumber 値をマップ内の accountnumber というキーに関連付けます。 キー **accountnumber** は、フォームデータモーダル内でリクエスト属性として定義されます。 この属性の値は、入力パラメーターとしてフォームデータモーダル読み取りサービスメソッドに渡されます。

7 行目：インタラクティブ通信ドキュメントの URL で識別されるリソースタイプに基づいて、受信したリクエストを別のサーブレットに送信します。 この 2 番目のサーブレットによって返された応答は、最初のサーブレットの応答に含まれます。

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

7 行目のコードの視覚的表示域

![リクエストパラメーターの設定](assets/requestparameter.png)

フォームデータモーダルの読み取りサービス用に定義されたリクエスト属性

[サンプルの AEM パッケージ](assets/webchanneldelivery.zip)
