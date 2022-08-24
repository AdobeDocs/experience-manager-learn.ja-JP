---
title: AEM Formsでのカスタム送信の作成
description: アダプティブフォーム用に独自のカスタム送信アクションを簡単かつ迅速に作成する方法
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 64b586a6-e9ef-4a3d-8528-55646ab03cc4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---

# AEM Formsでのカスタム送信の作成 {#writing-a-custom-submit-in-aem-forms}

アダプティブフォーム用に独自のカスタム送信アクションを簡単かつ迅速に作成する方法

この記事では、アダプティブFormsの送信を処理するためのカスタム送信アクションを作成するために必要な手順について説明します。

* crx にログイン
* apps の下に、タイプ「sling :folder」のノードを作成します。 このノードを CustomSubmitHelpx と呼びます。
* 新しく作成したノードを保存します。
* 新しく作成したノードに次の 2 つのプロパティを追加します。
* プロパティ名 |プロパティ値
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel | xfa,xsd,basic
* jcr :description | CustomSubmitHelpx
* 変更を保存します。
* CustomSubmitHelpxPOSTの下に post.node.jsp という新しいファイルを作成します。アダプティブフォームが送信されると、この JSP が呼び出されます。 必要に応じて、このファイルに JSP コードを書き込むことができます。 次のコードは、要求をサーブレットに転送します。

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

* CustomSubmitHelpx ノードの下に addfields.jsp という名前のファイルを作成します。 このファイルを使用すると、署名済みドキュメントにアクセスできます。
* このファイルに次のコードを追加します。

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* 変更を保存します

次の画像のように、アダプティブフォームの送信アクションに「CustomSubmitHelpx」が表示され始めます。

![カスタム送信を含むアダプティブフォーム](assets/capture-2.gif)
