---
title: AEM Formsでカスタム送信を作成する
seo-title: AEM Formsでカスタム送信を作成する
description: アダプティブフォーム用に独自のカスタム送信アクションをすばやく簡単に作成する方法
seo-description: アダプティブフォーム用に独自のカスタム送信アクションをすばやく簡単に作成する方法
feature: Adaptive Forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 1%

---


# AEM Formsでのカスタム送信の作成{#writing-a-custom-submit-in-aem-forms}

アダプティブフォーム用に独自のカスタム送信アクションをすばやく簡単に作成する方法

この記事では、アダプティブFormsの送信を処理するカスタム送信アクションを作成するために必要な手順を説明します。

* crxにログイン
* アプリの下にタイプ「sling :folder」のノードを作成します。 このノードをCustomSubmitHelpxと呼び出します。
* 新しく作成したノードを保存します。
* 新し追加く作成されたノードに対する次の2つのプロパティ
* プロパティ名       |プロパティ値
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,basic
* jcr :description   | CustomSubmitHelpx
* 変更を保存する
* CustomSubmitHelpxPOSTの下にpost.node.jspという名前の新しいファイルを作成します。アダプティブフォームが送信されると、このJSPが呼び出されます。 このファイルでは、必要に応じてJSPコードを書き込むことができます。 次のコードは、リクエストをサーブレットに転送します。

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

* CustomSubmitHelpノードの下にaddfields.jspというファイルを作成します。 このファイルを使用すると、署名済みドキュメントにアクセスできます。
* こ追加のファイルの次のコード

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* 変更を保存する

次の図に示すように、アダプティブフォームの送信アクションに「CustomSubmitHelpx」と表示される開始が表示されます。

![カスタム送信を伴うアダプティブフォーム](assets/capture-2.gif)

