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
jira: KT-8852
exl-id: 983e0394-7142-481f-bd5e-6c9acefbfdd0
duration: 52
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 100%

---

# 送信されたデータを処理するためのサーブレットの作成

IntelliJ で aem-banking プロジェクトを起動します。
簡単なサーブレットを作成して、送信されたデータをログファイルに出力します。次のスクリーンショットに示すように、コードがコアプロジェクト内にあることを確認します。
![サーブレットの作成](assets/create-servlet.png)

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

[以前のバージョンの AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=ja) で作成したのと同じ方法で、`apps/bankingapplication` フォルダーにカスタム送信アクションを作成します。このチュートリアルでは、CRX リポジトリの `apps/bankingapplication` ノードの下に SubmitToAEMServlet というフォルダーを作成します。

post.POST.jsp 内の次のコードは、/bin/formstutorial にマウントされたサーブレットにリクエストを転送するだけです。これは、前の手順で作成したサーブレットと同じです

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

IntelliJ の AEM プロジェクトで、`apps/bankingapplication` フォルダーを右クリックし、新規／パッケージを選択し、新しいパッケージダイアログボックスの apps.bankingapplication の後に SubmitToAEMServlet と入力します。SubmitToAEMServlet ノードを右クリックし、リポジトリ／コマンドを取得を選択して、AEM プロジェクトを AEM サーバーリポジトリと同期します。


## アダプティブフォームの設定

これで、**AEM サーブレットに送信**&#x200B;と呼ばれるこのカスタム送信ハンドラーに送信するようにアダプティブフォームを設定できます。

## 次の手順

[リソースタイプを使用したサーブレットの登録](./registering-servlet-using-resourcetype.md)
