---
title: 外部サーバーへのアダプティブフォームの送信
seo-title: 外部サーバーへのアダプティブフォームの送信
description: 外部サーバーで実行されているRESTエンドポイントへのアダプティブフォームの送信
seo-description: 外部サーバーで実行されているRESTエンドポイントへのアダプティブフォームの送信
uuid: 1a46e206-6188-4096-816a-d59e9fb43263
feature: アダプティブフォーム
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 12%

---


# 外部サーバーへのアダプティブフォームの送信{#submitting-adaptive-form-to-external-server}

RESTエンドポイントへの送信アクションを使用して、送信されたデータをREST URLに送信します。 URL は、内部（フォームがレンダリングされるサーバー）または外部サーバーのどちらのものでも使用できます。

通常、顧客は、追加の処理を行うために外部サーバーにフォームデータを送信する必要があります。

内部サーバーにデータをPOST送信するには、リソースのパスを指定します。 データは、リソースのパスに POST されます。例えば、 &lt;/content/restEndPoint> 。 このようなPOSTリクエストでは、送信リクエストの認証情報が使用されます。

外部サーバーにデータを POST 送信するには、URL を指定します。URL の形式は、<http://host:port/path_to_rest_end_point> です。匿名で要求を処理するパスが設定されていることを確認してPOSTリクエストを処理します。

この記事の目的のために、Tomcatインスタンスにデプロイできる単純なwarファイルを作成しました。 Tomcatがポート8080で実行されている場合、POSTURLは

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

アダプティブフォームをこのエンドポイントに送信するように設定する場合、次のコードによってフォームデータと添付ファイル（存在する場合）がサーブレットで抽出されます

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

![](assets/formsubmission.gif)
formsubmissionお使いのサーバーでテストするには、次の手順を実行してください

1. Tomcatをまだインストールしていない場合は、インストールします。 [Tomcatのインストール手順は、こちらから参照できます。](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. この記事に関連付けられた[zipファイル](assets/aemformsenablement.zip)をダウンロードします。 ファイルを解凍し、warファイルを取得します。
1. Tomcatサーバーにwarファイルをデプロイします。
1. 添付ファイルを含むシンプルなアダプティブフォームを作成し、上のスクリーンショットに示すように、その送信アクションを設定します。 POSTURLは<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>です。 AEMとtomcatがlocalhostで実行されていない場合は、URLを適宜変更してください。
1. Tomcatへのマルチパートフォームデータ送信を有効にするには、 &lt;tomcatInstallDir>\conf\context.xmlのコンテキスト要素に次の属性を追加し、Tomcatサーバーを再起動します。
1. **&lt;context allowCasualMultipartParsing=&quot;true&quot;>**
1. アダプティブフォームをプレビューし、添付ファイルを追加して送信します。 Tomcatコンソールウィンドウでメッセージを確認します。

