---
title: 外部サーバーへのアダプティブフォームの送信
seo-title: Submitting Adaptive Form to External Server
description: 外部サーバーで実行中の REST エンドポイントにアダプティブフォームを送信する
seo-description: Submitting Adaptive Form to REST endpoint running on external server
uuid: 1a46e206-6188-4096-816a-d59e9fb43263
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Development
role: Developer
level: Beginner
exl-id: 5363c3f7-9006-4430-b647-f3283a366a64
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 12%

---

# 外部サーバーへのアダプティブフォームの送信 {#submitting-adaptive-form-to-external-server}

REST エンドポイントへの送信アクションを使用して、送信されたデータを REST URL に投稿します。 URL は、内部（フォームがレンダリングされるサーバー）または外部サーバーのどちらのものでも使用できます。

通常、顧客はフォームデータを外部サーバーに送信して、さらに処理を行う必要があります。

内部サーバーにデータを POST 送信するには、リソースのパスを指定します。 データは、リソースのパスに POST されます。例： &lt;/content restendpoint=&quot;&quot;> . このような POST リクエストでは、送信リクエストの認証情報が使用されます。

外部サーバーにデータを POST 送信するには、URL を指定します。URL の形式は、<http://host:port/path_to_rest_end_point> です。匿名で要求を処理するパスが設定されていることを確認し、POST要求を処理します。

この記事の目的のために、Tomcat インスタンスにデプロイできる単純な war ファイルを作成しました。 Tomcat がポート 8080 で実行されている場合、POSTURL は次のようになります。

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

このエンドポイントにアダプティブフォームを送信するよう設定する場合、次のコードによって、フォームデータと添付ファイル（存在する場合）がサーブレットで抽出される可能性がある場合は、

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

1. Tomcat をインストールします（まだインストールしていない場合）。 [Tomcat のインストール手順は、こちらを参照してください。](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. をダウンロードします。 [zip ファイル](assets/aemformsenablement.zip) この記事に関連付けられています。 ファイルを解凍し、war ファイルを取得します。
1. war ファイルを tomcat サーバーにデプロイします。
1. 添付ファイルコンポーネントを含むシンプルなアダプティブフォームを作成し、上のスクリーンショットに示すように、送信アクションを設定します。 POSTURL は <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. AEMと tomcat が localhost で実行されていない場合は、URL を適宜変更してください。
1. Tomcat へのマルチパートフォームデータ送信を有効にするには、次の属性を &lt;tomcatinstalldir>\conf\context.xml を起動し、Tomcat サーバーを再起動します。
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. アダプティブフォームをプレビューし、添付ファイルを追加して送信します。 Tomcat コンソールウィンドウでメッセージを確認します。
