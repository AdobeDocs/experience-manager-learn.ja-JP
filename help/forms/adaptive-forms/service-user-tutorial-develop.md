---
title: AEM Formsでのサービスユーザーを使用した開発
description: この記事では、AEM Formsでのサービスユーザーの作成プロセスについて説明します
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 5fa3d52a-6a71-45c4-9b1a-0e6686dd29bc
source-git-commit: f1afccdad8d819604c510421204f59e7b3dc68e4
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 2%

---

# AEM Formsでのサービスユーザーを使用した開発

この記事では、AEM Formsでのサービスユーザーの作成プロセスについて説明します

以前のバージョンのAdobe Experience Manager(AEM) では、管理リソースリゾルバーが、リポジトリへのアクセスを必要とするバックエンド処理に使用されていました。 管理リソースリゾルバーの使用は、AEM 6.3 では非推奨（廃止予定）となっています。代わりに、リポジトリ内の特定の権限を持つシステムユーザーが使用されます。

AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/service-users.html) での [ サービスユーザーの作成と使用の詳細について説明します。

この記事では、システムユーザーの作成と、ユーザーマッパーのプロパティの設定に関する手順を説明します。

1. [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp) に移動します。
1. 「管理者」としてログインします。
1. 「ユーザー管理」をクリックします。
1. 「システムユーザーの作成」をクリックします。
1. userid タイプを「 data 」に設定し、緑のアイコンをクリックして、システムユーザーの作成プロセスを完了します。
1. [configMgr を開きます。](http://localhost:4502/system/console/configMgr)
1. 「 Apache Sling Service User Mapper Service 」を検索し、クリックしてプロパティを開きます。
1. *+* アイコン（プラス）をクリックして、次のサービスマッピングを追加します。

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. 「保存」をクリックします。

上記の設定では、 DevelopingWithServiceUser.core がバンドルのシンボリック名になります。 getresourceresolver はサブサービス名です。data は、前の手順で作成したシステムユーザーです。

また、fd-service ユーザーの代わりにリソースリゾルバーを取得することもできます。 このサービスユーザーは、ドキュメントサービスに使用されます。 例えば、使用権限などを認証/適用する場合、fd-service ユーザーのリソースリゾルバーを使用して操作を実行します

1. [この記事に関連付けられている zip ファイルをダウンロードして解凍します。](assets/developingwithserviceuser.zip)
1. [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) に移動します。
1. OSGi バンドルをアップロードして起動します。
1. バンドルがアクティブ状態であることを確認します。
1. これで、*システムユーザー* が正常に作成され、*サービスユーザーバンドル* もデプロイされました。

   /content へのアクセスを提供するには、システムユーザー (「 data 」) にコンテンツノードに対する読み取り権限を付与します。

   1. [http://localhost:4502/useradmin](http://localhost:4502/useradmin) に移動します。
   1. ユーザー「 data 」を検索します。 これは、前の手順で作成したシステムユーザーと同じです。
   1. ユーザーをダブルクリックし、「権限」タブをクリックします。
   1. 「コンテンツ」フォルダーへの「読み取り」アクセス権を付与します。
   1. サービスユーザーを使用して/content フォルダーにアクセスするには、次のコードを使用します



```java
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
resourceResolver = aemDemoListings.getServiceResolver();
   
// get the resource. This will succeed because we have given ' read ' access to the content node
   
Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
```

バンドル内の/content/dam/data.jsonファイルにアクセスする場合は、次のコードを使用します。 このコードは、/content/dam/ノードの「data」ユーザーに対して読み取り権限が付与されていることを前提としています

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
