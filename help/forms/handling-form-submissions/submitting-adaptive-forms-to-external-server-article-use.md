---
title: 外部サーバーへのアダプティブフォームの送信
description: 外部サーバーで実行中の REST エンドポイントへのアダプティブフォームの送信
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Development
role: Developer
level: Beginner
exl-id: 5363c3f7-9006-4430-b647-f3283a366a64
last-substantial-update: 2020-07-07T00:00:00Z
duration: 78
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 100%

---

# 外部サーバーへのアダプティブフォームの送信 {#submitting-adaptive-form-to-external-server}

REST エンドポイントへの送信アクションを利用して、送信されたデータを REST URL に POST することができます。URL は、内部（フォームがレンダリングされるサーバー）または外部サーバーのどちらのものでも使用できます。

通常、さらに処理を行うために、顧客はフォームデータを外部サーバーに送信します。

内部サーバーにデータを POST 送信するには、リソースのパスを指定します。データは、リソースのパスに POST されます。例：&lt;/content/restEndPoint>。このような POST リクエストには、送信リクエストの認証情報が使用されます。

外部サーバーにデータを POST 送信するには、URL を指定します。URL の形式は、<http://host:port/path_to_rest_end_point> です。POST リクエストを匿名で処理するようにパスを設定していることを確認してください。

この記事の目的のために、Tomcat インスタンスにデプロイできる単純な war ファイルを作成しました。Tomcat がポート 8080 で実行されている場合、POST URL は次のようになります。

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

このエンドポイントにアダプティブフォームを送信するように設定する場合、次のコードによって、フォームデータと添付ファイル（存在する場合）をサーブレットで抽出することができます

```java
System.out.println("form was submitted");
Part attachment = request.getPart("attachments");
if(attachment!=null)
{
    System.out.println("The content type of the attachment added is "+attachment.getContentType());
}
Enumeration<String> params = request.getParameterNames();
while(params.hasMoreElements())
{
String paramName = params.nextElement();
System.out.println("The param Name is "+paramName);
String data = request.getParameter(paramName);System.out.println("The data  is "+data);
}
```

![formsubmission](assets/formsubmission.gif)
これをサーバーでテストするには、次の手順を実行してください

1. Tomcat をインストールします（まだインストールしていない場合）。 [Tomcat のインストール手順は、こちらを参照してください](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. この記事に関連付けられている [zip ファイル](assets/aemformsenablement.zip)をダウンロードします。ファイルを解凍し、war ファイルを取得します。
1. war ファイルを Tomcat サーバーにデプロイします。
1. 添付ファイルコンポーネントを含むシンプルなアダプティブフォームを作成し、上のスクリーンショットに示されているように、送信アクションを設定します。POST URL は <http://localhost:8080/AemFormsEnablement/HandleFormSubmission> です。AEMと Tomcat が localhost で実行されていない場合は、URL を適宜変更してください。
1. Tomcat へのマルチパートフォームデータ送信を有効にするには、次の属性を &lt;tomcatInstallDir>\conf\context.xml のコンテキスト要素に追加し、Tomcat サーバーを再起動します。
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. アダプティブフォームをプレビューし、添付ファイルを追加して送信します。Tomcat コンソールウィンドウでメッセージを確認します。
