---
title: data 属性を使用したHTML5 Formsの事前入力
description: バックエンドソースからHTMLを取得して、データを取得することによってデータ 5 フォームを生成します。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
source-git-commit: d7b1ab815d9c8a0d0342f7b57d1efb08fb39a26a
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 2%

---

# データ属性を使用したHTML5 Formsの事前入力 {#prepopulate-html-forms-using-data-attribute}


AEM Formsを使用してHTML形式でレンダリングされた XDP テンプレートは、HTML5 またはモバイルFormsと呼ばれます。 一般的な使用例としては、フォームのレンダリング時にこれらのフォームに事前に値を設定する場合があります。

データをテンプレートとしてレンダリングする際に、データを xdp テンプレートとマージする方法は 2 つあります。HTML

**dataRef**:URL で dataRef パラメーターを使用できます。 このパラメーターは、テンプレートと結合されるデータファイルの絶対パスを指定します。 このパラメーターには、XML 形式のデータを返す REST サービスへの URL を指定できます。

**データ**:このパラメーターは、テンプレートと結合される UTF-8 でエンコードされたデータバイトを指定します。 このパラメーターが指定されている場合、HTML5 form は dataRef パラメーターを無視します。ベストプラクティスとして、データアプローチを使用することをお勧めします。

推奨される方法は、フォームに事前入力するデータをリクエストのデータ属性に設定することです。

slingRequest.setAttribute(&quot;data&quot;, content);

この例では、コンテンツを data 属性に設定します。 コンテンツは、フォームに事前入力するデータを表します。 通常は、内部サービスに対して REST 呼び出しをおこなうことで、「コンテンツ」を取得します。

この使用例を実現するには、カスタムプロファイルを作成する必要があります。 カスタムプロファイルの作成に関する詳細については、 [AEM Formsドキュメントはこちら](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

カスタムプロファイルを作成したら、バックエンドシステムに対して呼び出しを実行してデータを取得する JSP ファイルを作成します。 データを取得すると、 slingRequest.setAttribute(&quot;data&quot;, content);フォームに事前入力する

XDP がレンダリングされる際に、一部のパラメーターを xdp に渡すこともできます。また、パラメーターの値に基づいて、バックエンドシステムからデータを取得できます。

[例えば、この URL には名前パラメーターが含まれています](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

書き込む JSP は、 request.getParameter(&quot;name&quot;) を使用して name パラメーターにアクセスできます。 その後、このパラメーターの値をバックエンドプロセスに渡して、必要なデータを取得できます。
この機能をシステムで動作させるには、次の手順に従ってください。

* [パッケージマネージャーを使用したAEMへのアセットのダウンロードと読み込み](assets/prepopulatemobileform.zip)
パッケージは次をインストールします

   * CustomProfile
   * サンプル XDP
   * フォームに入力するデータを返すサンプルPOSTエンドポイント

>[!NOTE]
>
>workbench プロセスを呼び出してフォームに入力する場合は、 /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jspに setdata.jsp ではなく callWorkbenchProcess.jsp を含める必要があります

* [お気に入りのブラウザーでこの URL を参照](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). フォームには、name パラメーターの値が事前入力されている必要があります
