---
title: データ属性を使用してHTML5 Formsを事前入力します。
description: バックエンドソースからデータを取得してHTML5フォームを設定する。
feature: アダプティブフォーム
version: 6.3,6.4,6.5.
topic: 開発
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 2%

---


# データ属性を使用したHTML5 Formsの事前入力 {#prepopulate-html-forms-using-data-attribute}

この機能のライブデモへのリンクについては、[AEM Formsのサンプル](https://forms.enablementadobe.com/content/samples/samples.html?query=0)ページを参照してください。

AEM Formsを使用してHTML形式でレンダリングされるXDPテンプレートは、HTML5またはMobile Formsと呼ばれます。 一般的な使用例としては、これらのフォームのレンダリング時に事前入力する場合があります。

データをHTMLとしてレンダリングする際に、xdpテンプレートとマージする方法は2つあります。

**dataRef**:URLでdataRefパラメーターを使用できます。このパラメーターは、テンプレートと結合されるデータファイルの絶対パスを指定します。 このパラメーターには、XML形式のデータを返すRESTサービスへのURLを指定できます。

**データ**:このパラメーターは、テンプレートと結合されるUTF-8エンコードされたデータバイトを指定します。このパラメーターが指定されている場合、HTML5 form は dataRef パラメーターを無視します。ベストプラクティスとして、データアプローチの使用をお勧めします。

推奨されるアプローチは、フォームに事前入力するデータをリクエストのデータ属性に設定することです。

slingRequest.setAttribute(&quot;data&quot;, content);

この例では、コンテンツを使用してdata属性を設定します。 コンテンツは、フォームに事前入力するデータを表します。 通常は、内部サービスに対してREST呼び出しをおこなうことで、「コンテンツ」を取得します。

この使用例を実現するには、カスタムプロファイルを作成する必要があります。 カスタムプロファイルの作成に関する詳細は、こちらの[AEM Formsのドキュメント](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html)に明確に記載されています。

カスタムプロファイルを作成したら、バックエンドシステムに対して呼び出しをおこなってデータを取得するJSPファイルを作成します。 データを取得すると、 slingRequest.setAttribute(&quot;data&quot;, content)；を使用します。フォームに事前入力する

XDPがレンダリングされる際に、一部のパラメーターをxdpに渡すこともできます。また、パラメーターの値に基づいて、バックエンドシステムからデータを取得できます。

[例えば、このURLにはnameパラメーターが含まれます](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

書き込むJSPは、 request.getParameter(&quot;name&quot;)を使用してnameパラメーターにアクセスできます。 その後、このパラメーターの値をバックエンドプロセスに渡して、必要なデータを取得できます。
この機能をシステムで動作させるには、次の手順に従ってください。

* [パッケージマネージャーを使用してアセットをダウンロードし、AEMに読み込み](assets/prepopulatemobileform.zip)
ます。パッケージには次がインストールされます

   * CustomProfile
   * サンプルXDP
   * フォームに入力するデータを返すサンプルのPOSTエンドポイント

>[!NOTE]
>
>Workbenchプロセスを呼び出してフォームに入力する場合は、 setdata.jspではなく、 /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jspにcallWorkbenchProcess.jspを含めることができます

* [お気に入りのブラウザーでこのURLを参照します](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems)。フォームには、nameパラメーターの値が事前に設定されている必要があります
