---
title: AEM Formsでのカスタム送信の作成
seo-title: AEM Formsでのカスタム送信の作成
description: アダプティブフォーム向けに独自のカスタム送信アクションを簡単かつ迅速に作成する方法
seo-description: アダプティブフォーム向けに独自のカスタム送信アクションを簡単かつ迅速に作成する方法
feature: アダプティブフォーム
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 1%

---


# AEM Formsでのカスタム送信の書き込み{#writing-a-custom-submit-in-aem-forms}

アダプティブフォーム向けに独自のカスタム送信アクションを簡単かつ迅速に作成する方法

この記事では、アダプティブFormsの送信を処理するためのカスタム送信アクションを作成するために必要な手順について説明します。

* crxにログインします。
* appsの下に、タイプ「sling :folder」のノードを作成します。 このノードをCustomSubmitHelpxと呼びます。
* 新しく作成したノードを保存します。
* 新しく作成したノードに次の2つのプロパティを追加します
* プロパティ名       |プロパティ値
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,basic
* jcr :description   | CustomSubmitHelpx
* 変更を保存します。
* CustomSubmitHelpxPOSTの下に、post.node.jspという名前の新しいファイルを作成します。アダプティブフォームが送信されると、このJSPが呼び出されます。 必要に応じて、このファイルにJSPコードを書き込むことができます。 次のコードは、要求をサーブレットに転送します。

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

* CustomSubmitHelpxノードの下にaddfields .jspというファイルを作成します。 このファイルを使用すると、署名済みドキュメントにアクセスできます。
* このファイルに次のコードを追加します

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* 変更を保存します

次の図に示すように、アダプティブフォームの送信アクションに「CustomSubmitHelpx」が表示され始めます。

![カスタム送信を含むアダプティブフォーム](assets/capture-2.gif)

