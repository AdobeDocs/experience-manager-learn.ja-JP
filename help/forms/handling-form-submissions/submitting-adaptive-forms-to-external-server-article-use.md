---
title: アダプティブフォームの外部サーバーへの送信
seo-title: アダプティブフォームの外部サーバーへの送信
description: 外部サーバーで実行されているRESTエンドポイントへのアダプティブフォームの送信
seo-description: 外部サーバーで実行されているRESTエンドポイントへのアダプティブフォームの送信
uuid: 1a46e206-6188-4096-816a-d59e9fb43263
feature: adaptive-forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 5%

---


# アダプティブフォームの外部サーバーへの送信{#submitting-adaptive-form-to-external-server}

RESTエンドポイントへの送信アクションを使用して、送信されたデータをREST URLにPOSTします。 URLには、内部（フォームがレンダリングされるサーバー）または外部サーバーを使用できます。

通常、顧客はフォームデータを外部サーバーに送信して、さらに処理する必要があります。

内部サーバーにデータをPOST送信するには、リソースのパスを指定します。 データは、リソースのパスに POST されます。例えば、&lt;/content/restEndPoint>のように指定します。 このようなPOSTリクエストの場合は、送信リクエストの認証情報が使用されます。

内部サーバーにデータを POST 送信するには、URL を指定します。URLの形式は<http://host:port/path_to_rest_end_point>です。 POST要求を匿名で処理するようにパスを設定していることを確認します。

この記事の目的で、tomcatインスタンスにデプロイできる単純なwarファイルを作成しました。 tomcatがポート8080で実行されている場合、POSTURLは

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

このエンドポイントに送信するようにアダプティブフォームを設定する場合、次のコードによってフォームデータと添付ファイルがサーブレット内で抽出できる場合は添付ファイルが抽出されます

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
formsubmissionサーバーでテストするには、次の手順を実行してください

1. Tomcatをまだインストールしていない場合は、インストールします。 [Tomcatのインストール手順は、こちらを参照してください](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. この記事に関連付けられている[zipファイル](assets/aemformsenablement.zip)をダウンロードします。 ファイルを解凍して、warファイルを取得します。
1. tomcatサーバーにwarファイルをデプロイします。
1. 添付ファイルコンポーネントを含む単純なアダプティブフォームを作成し、上のスクリーンショットに示すようにその送信アクションを設定します。 POSTURLは<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>です。 AEMとtomcatがlocalhostで実行されていない場合は、それに応じてURLを変更してください。
1. tomcatへのマルチパートフォームデータ送信を有効にするには、次の属性を&lt;tomcatInstallDir>\conf\context.xmlのコンテキスト要素に追加し、Tomcatサーバーを再起動してください。
1. **&lt;context allowCasualMultipartParsing=&quot;true&quot;>**
1. アダプティブフォームをプレビューし、添付ファイルを追加して送信します。 tomcatコンソールウィンドウでメッセージを確認します。

