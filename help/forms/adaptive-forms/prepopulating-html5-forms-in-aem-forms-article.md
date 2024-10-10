---
title: data 属性を使用して HTML5 フォームに事前入力します。
description: バックエンドソースからデータを取得して、HTML5 フォームに入力します。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
last-substantial-update: 2020-01-09T00:00:00Z
duration: 94
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '449'
ht-degree: 100%

---

# data 属性を使用した HTML5 フォームへの事前入力 {#prepopulate-html-forms-using-data-attribute}


AEM Forms を使用して HTML 形式でレンダリングされた XDP テンプレートは、HTML5 フォームまたはモバイルフォームと呼ばれます。一般的なユースケースは、レンダリングの際にこれらのフォームに事前入力することです。

HTML としてレンダリングするときにデータを XDP テンプレートと結合する方法は 2 とおりあります。

**dataRef**：URL で dataRef パラメータを使用できます。このパラメーターは、テンプレートと結合されるデータファイルの絶対パスを指定します。このパラメーターには、XML 形式データを返す REST サービスへの URL を使用できます。

**data**：このパラメーターはテンプレートと結合される、UTF-8 でエンコードされたデータバイトを指定します。このパラメーターが指定されている場合、HTML5 フォームでは dataRef パラメーターが無視されます。ベストプラクティスとして、データアプローチを使用することをお勧めします。

お勧めのアプローチは、フォームに事前入力するデータを使用してリクエストの data 属性を設定することです。

slingRequest.setAttribute(&quot;data&quot;, content);

この例では、コンテンツを data 属性に設定しています。コンテンツは、フォームに事前入力するデータを表します。通常は、内部サービスに対して REST 呼び出しを実行して、「コンテンツ」を取得します。

このユースケースを達成するには、カスタムプロファイルを作成する必要があります。カスタムプロファイルの作成について詳しくは、[こちらの AEM Forms ドキュメント](https://helpx.adobe.com/jp/aem-forms/6/html5-forms/custom-profile.html)を参照してください。

カスタムプロファイルを作成したら、バックエンドシステムに対して呼び出しを実行して、データを取得する JSP ファイルを作成します。データを取得したら、slingRequest.setAttribute(&quot;data&quot;, content); を使用してフォームに事前入力します。

XDP がレンダリングされる際に、一部のパラメーターを XDP に渡すこともでき、パラメーターの値に基づいて、バックエンドシステムからデータを取得できます。

[例えば、この URL には name パラメーターが含まれています。](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

書き込んだ JSP で、request.getParameter(&quot;name&quot;) を使用して name パラメーターにアクセスできます。その後、このパラメーターの値をバックエンドプロセスに渡して、必要なデータを取得できます。
この機能をシステムで動作させるには、次の手順に従ってください。

* [パッケージマネージャーを使用して、アセットをダウンロードし AEM に読み込みます](assets/prepopulatemobileform.zip)。
パッケージでは以下をインストールします。

   * カスタムプロファイル
   * サンプル XDP
   * フォームに入力するデータを返すサンプル POST エンドポイント

>[!NOTE]
>
>ワークベンチプロセスを呼び出してフォームに入力する場合は、setdata.jsp ではなく callWorkbenchProcess.jsp を /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp にインクルードすることができます。

* [お気に入りのブラウザーでこの URL を参照します](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems)。フォームに name パラメーターの値が事前入力されているはずです。
