---
title: カスタム送信アクションハンドラーの作成
description: カスタム送信ハンドラーへのアダプティブフォームの送信
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8852
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# サーブレットを作成して送信されたデータを処理

IntelliJ で aem-banking プロジェクトを起動します。
簡単なサーブレットを作成して、送信されたデータをログファイルに出力します。次のスクリーンショットに示すように、コードがコアプロジェクト内にあることを確認します。
![create-servlet](assets/create-servlet.png)

```java
package com.aem.bankingapplication.core.servlets;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import javax.servlet.Servlet;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.osgi.service.component.annotations.Component;
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/formstutorial"})
public class HandleFormSubmissison extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(HandleFormSubmissison.class);
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) {
        log.debug("Inside my formstutorial servlet");
        log.debug("The form data I got was "+request.getParameter("jcr:data"));
    }
}
```

## カスタム送信の作成

app/bankingapplication フォルダーに、 [AEM Formsの以前のバージョン](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en)
post.formstution.jsp の次のコードは、/bin/formstutorial にマウントされたPOSTに要求を転送するだけです。 これは、前の手順で作成したサーブレットと同じです

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

## アダプティブフォームの設定

これで、アダプティブフォームを設定して、次の名前のカスタム送信ハンドラーに送信できます。 **AEM Servlet に送信**



