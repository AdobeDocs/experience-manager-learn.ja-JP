---
title: data属性を使用してHTML5Formsを事前入力します。
seo-title: data属性を使用してHTML5Formsを事前入力します。
description: バックエンドソースからデータを取得して、HTML5フォームに入力します。
seo-description: バックエンドソースからデータを取得して、HTML5フォームに入力します。
feature: integrations
topics: mobile-forms
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5.
uuid: 889d2cd5-fcf2-4854-928b-0c2c0db9dbc2
discoiquuid: 3aa645c9-941e-4b27-a538-cca13574b21c
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 2%

---


# データ属性{#prepopulate-html-forms-using-data-attribute}を使用してHTML5Formsを事前入力

[AEM Formsのサンプル](https://forms.enablementadobe.com/content/samples/samples.html?query=0)ページをご覧になって、この機能のライブデモへのリンクをご覧ください。

AEM Formsを使用してHTML形式でレンダリングされたXDPテンプレートは、HTML5またはモバイルFormsと呼ばれます。 一般的な使用例は、これらのフォームがレンダリングされる際に事前設定する場合です。

HTMLとしてレンダリングされるデータをxdpテンプレートとマージする方法は2つあります。

**dataRef**:URLでdataRefパラメーターを使用できます。このパラメーターは、テンプレートと結合されるデータファイルの絶対パスを指定します。 このパラメーターには、XML形式でデータを返すRESTサービスへのURLを指定できます。

**data**:このパラメーターは、テンプレートと結合されるUTF-8エンコードされたデータバイトを指定します。このパラメーターが指定されている場合、HTML5 form は dataRef パラメーターを無視します。ベストプラクティスとして、データアプローチを使用することをお勧めします。

推奨されるアプローチは、フォームに事前入力するデータを使用して、リクエストのdata属性を設定することです。

slingRequest.setAttribute(&quot;data&quot;, content);

この例では、コンテンツを使用してdata属性を設定します。 コンテンツは、フォームに事前入力するデータを表します。 通常は、内部サービスに対してREST呼び出しを行うことで「コンテンツ」を取得します。

この使用例を実現するには、カスタムプロファイルを作成する必要があります。 カスタムプロファイルの作成に関する詳細は、[AEM Formsのドキュメント](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html)に明確に記載されています。

カスタムプロファイルを作成したら、バックエンドシステムに対して呼び出しを行ってデータを取得するJSPファイルを作成します。 データが取得されると、slingRequest.setAttribute(&quot;data&quot;, content)；を使用します。フォームを事前入力するには

XDPのレンダリング中に、いくつかのパラメーターをxdpに渡し、パラメーターの値に基づいて、バックエンドシステムからデータを取得することもできます。

[例えば、このURLにnameパラメーターがあるとします。](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

作成したJSPは、request.getParameter(&quot;name&quot;)を通じてnameパラメーターにアクセスできます。 その後、このパラメーターの値をバックエンドプロセスに渡して、必要なデータを取得できます。
この機能をシステムで動作させるには、次の手順に従ってください。

* [Package ](assets/prepopulatemobileform.zip)
Managerを使用して、アセットをダウンロードし、AEMに読み込みます。パッケージは、次をインストールします

   * CustomProfile
   * サンプルXDP
   * フォームに入力するためにデータを返すサンプルPOSTエンドポイント

>[!NOTE]
>
>Workbenchプロセスを呼び出してフォームを設定する場合、/apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jspフォルダーにsetdata.jspではなくcallWorkbenchProcess.jspを含めることができます

* [お気に入りのブラウザーにこのURLを指定します](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems)。フォームには、nameパラメーターの値が事前に入力される必要があります
