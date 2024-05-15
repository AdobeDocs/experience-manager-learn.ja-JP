---
title: AEM Forms でのサービスユーザーを使用した開発
description: この記事では、AEM Formsでサービスユーザーを作成するプロセスについて説明します
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 5fa3d52a-6a71-45c4-9b1a-0e6686dd29bc
last-substantial-update: 2020-09-09T00:00:00Z
duration: 129
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 100%

---

# AEM Forms でのサービスユーザーを使用した開発

この記事では、AEM Formsでサービスユーザーを作成するプロセスについて説明します

以前のバージョンの Adobe Experience Manager（AEM）では、管理リソースリゾルバーが、リポジトリへのアクセスを必要とするバックエンド処理に使用されていました。AEM 6.3 では、管理リソースリゾルバーの使用は非推奨（廃止予定）となっています。代わりに、リポジトリ内の特定の権限を持つシステムユーザーが使用されます。

[AEM でのサービスユーザーの作成と使用](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/service-users.html?lang=ja)について詳しく説明します。

この記事では、システムユーザーの作成とユーザーマッパープロパティの設定について説明します。

1. [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp) に移動します。
1. 「admin」としてログインします。
1. 「ユーザー管理」をクリックします。
1. 「システムユーザーを作成」をクリックします。
1. ユーザー ID タイプを「data」に設定し、緑のアイコンをクリックして、システムユーザーの作成プロセスを完了します。
1. [configMgr を開く](http://localhost:4502/system/console/configMgr)
1. _Apache Sling Service User Mapper Service_ を探し、クリックしてプロパティを開きます。
1. *+* アイコン（プラス）をクリックして、次のサービスマッピングを追加します。

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. 「保存」をクリック

上記の設定では、DevelopingWithServiceUser.core がバンドルのシンボル名になります。getresourceresolver はサブサービス名です。data は、前の手順で作成したシステムユーザーです。

また、fd-service ユーザーの代わりにリソースリゾルバーを取得することもできます。このサービスユーザーは、ドキュメントサービスに使用されます。例えば、使用権限などを認証／適用する場合、fd-service ユーザーのリソースリゾルバーを使用して操作を実行します

1. [この記事に関連付けられている zip ファイルをダウンロードして展開します。](assets/developingwithserviceuser.zip)
1. [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) に移動します。
1. OSGi バンドルをアップロードして開始します。
1. バンドルがアクティブ状態であることを確認します。
1. これで、*システムユーザー*&#x200B;を正常に作成し、*サービスユーザーバンドル*&#x200B;もデプロイしました。

   /content へのアクセスを提供するには、content ノードに対する読み取り権限をシステムユーザー（「data」）に付与します。

   1.  [http://localhost:4502/useradmin](http://localhost:4502/useradmin) に移動します。
   1. ユーザー「data」を検索します。これは、前の手順で作成しシステムユーザーと同じです
   1. ユーザーをダブルクリックし、「権限」タブをクリックします。
   1. 「読み取り」アクセス権を「コンテンツ」フォルダーに付与します。
   1. サービスユーザーを使用して /content フォルダーにアクセスするには、次のコードを使用します。



```java
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
resourceResolver = aemDemoListings.getServiceResolver();
   
// get the resource. This will succeed because we have given ' read ' access to the content node
   
Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
```

バンドル内の /content/dam/data.json ファイルにアクセスする場合は、次のコードを使用します。このコードは、/content/dam/ノードの「data」ユーザーに対して読み取り権限が付与されていることを前提としています。

```java
@Reference
GetResolver getResolver;
.
.
.
try {
   ResourceResolver serviceResolver = getResolver.getServiceResolver();

   // To get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();

   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
} catch(LoginException ex) {
   // Unable to get the service user, handle this exception as needed
} finally {
  // Always close all ResourceResolvers you open!
  
  if (serviceResolver != null( { serviceResolver.close(); }
  if (fdserviceResolver != null) { fdserviceResolver.close(); }
}
```

実装の完全なコードを以下に示します

```java
package com.mergeandfuse.getserviceuserresolver.impl;
import java.util.HashMap;
import org.apache.sling.api.resource.LoginException;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = GetResolver.class)
public class GetResolverImpl implements GetResolver {
        @Reference
        ResourceResolverFactory resolverFactory;

        @Override
        public ResourceResolver getServiceResolver() {
                System.out.println("#### Trying to get service resource resolver ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {

                        System.out.println("Login Exception " + e.getMessage());
                }
                return resolver;

        }

        @Override
        public ResourceResolver getFormsServiceResolver() {

                System.out.println("#### Trying to get Resource Resolver for forms ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getformsresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {
                        System.out.println("Login Exception ");
                        System.out.println("The error message is " + e.getMessage());
                }
                return resolver;
        }

}
```
