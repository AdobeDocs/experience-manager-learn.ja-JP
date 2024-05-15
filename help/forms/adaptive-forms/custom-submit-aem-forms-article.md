---
title: AEM Forms でのカスタム送信の作成
description: アダプティブフォーム用に独自のカスタム送信アクションを簡単かつ迅速に作成する方法
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 64b586a6-e9ef-4a3d-8528-55646ab03cc4
last-substantial-update: 2021-04-09T00:00:00Z
duration: 51
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 100%

---

# AEM Forms でのカスタム送信の作成 {#writing-a-custom-submit-in-aem-forms}

アダプティブフォーム用に独自のカスタム送信アクションを簡単かつ迅速に作成する方法

>[!NOTE]
>この記事のコードは、コアコンポーネントベースのアダプティブフォームでは機能しません。
>[コアコンポーネントベースのアダプティブフォームに関する同様の記事は、こちらから参照できます](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/custom-submit-headless-forms/custom-submit-service.html?lang=ja)


この記事では、アダプティブフォームの送信を処理するカスタム送信アクションを作成するために必要な手順について説明します。

* crx にログインする
* アプリの下に、「sling :folder」タイプのノードを作成します。このノードを CustomSubmitHelpx と呼びましょう。
* 新しく作成したノードを保存します。
* 新しく作成したノードに次の 3 つのプロパティを追加します

| プロパティ名 | プロパティの値 |
|----------------    | ---------------------------------|
| guideComponentType | fd/af/components/guidesubmittype |
| guideDataModel | xfa,xsd,basic |
| jcr :description | CustomSubmitHelpx |


* 変更内容を保存します
* CustomSubmitHelpx ノードの下に post.POST.jsp という新しいファイルを作成します。アダプティブフォームが送信されると、この JSP が呼び出されます。このファイルに要件に応じた JSP コードを書き込むことができます。次のコードは、リクエストをサーブレットに転送します。

```java
<%
%><%@include file="/libs/foundation/global.jsp"%>
<%@taglib prefix="cq" uri="http://www.day.com/taglibs/cq/1.0"%>
<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode,com.adobe.forms.common.submitutils.CustomParameterRequest,com.adobe.aemds.guide.submitutils.*" %>

<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode" %>
<%@page session="false" %>
<%

   com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/storeafsubmission",null,null);

%>
```

* CustomSubmitHelpx ノードの下に addfields.jsp という名前のファイルを作成します。このファイルを使用すると、署名済みドキュメントにアクセスできます。
* 次のコードをこのファイルに追加します

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* 変更を保存します。

この画像のように、アダプティブフォームの送信アクションに「CustomSubmitHelpx」が表示され始めます。

![カスタム送信を含むアダプティブフォーム](assets/capture-2.gif)
