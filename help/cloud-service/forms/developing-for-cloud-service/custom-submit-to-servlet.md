---
title: カスタム送信アクションハンドラーの作成
description: カスタム送信ハンドラーへのアダプティブフォームの送信
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
kt: 8852
exl-id: 832f7e82-3e03-4ac6-9c8b-e96f0efecd32
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 2%

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

## カスタム送信ハンドラーの作成

カスタム送信アクションを `apps/bankingapplication` フォルダー内に [AEM Formsの以前のバージョン](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en). このチュートリアルの目的で、SubmitToAEMServlet というフォルダーを `apps/bankingapplication` CRX リポジトリのノード。

post.formstution.jsp の次のコードは、/bin/formstutorial にマウントされたPOSTに要求を転送するだけです。 これは、前の手順で作成したサーブレットと同じです

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

IntelliJ のAEMプロジェクトで、 `apps/bankingapplication` フォルダーに移動し、「新規」を選択します。 |新しいパッケージダイアログボックスで、apps.bankingapplication の後に SubmitToAEMServlet をパッケージ化して入力します。 「 SubmitToAEMServlet 」ノードを右クリックし、「 repo 」を選択します。 | AEMプロジェクトをAEMサーバーリポジトリと同期するコマンドを取得します。


## アダプティブフォームの設定

これで、任意のアダプティブフォームを設定して、次の名前のカスタム送信ハンドラーに送信できます。 **AEM Servlet に送信**

## 次の手順

[フォームポータルコンポーネントの有効化](./forms-portal-components.md)
