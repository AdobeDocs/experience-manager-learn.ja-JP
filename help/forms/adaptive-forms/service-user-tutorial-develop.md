---
title: AEM Formsでのサービスユーザーを使用した開発
seo-title: AEM Formsでのサービスユーザーを使用した開発
description: この記事では、AEM Formsでのサービスユーザーの作成プロセスについて説明します
seo-description: この記事では、AEM Formsでのサービスユーザーの作成プロセスについて説明します
uuid: 996f30df-3fc5-4232-a104-b92e1bee4713
feature: アダプティブフォーム
topics: development,administration
audience: implementer,developer
doc-type: article
activity: setup
discoiquuid: 65bd4695-e110-48ba-80ec-2d36bc53ead2
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 3%

---


# AEM Formsでのサービスユーザーを使用した開発

この記事では、AEM Formsでのサービスユーザーの作成プロセスについて説明します

以前のバージョンのAdobe Experience Manager(AEM)では、管理リソースリゾルバーが、リポジトリへのアクセスを必要とするバックエンド処理に使用されていました。 AEM 6.3では、管理リソースリゾルバーの使用は廃止されています。代わりに、リポジトリ内の特定の権限を持つシステムユーザーが使用されます。

この記事では、システムユーザーの作成と、ユーザーマッパーのプロパティの設定に関する手順を説明します。

1. [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)に移動します。
1. 「 admin 」としてログインします。
1. 「ユーザー管理」をクリックします。
1. 「システムユーザーの作成」をクリックします。
1. useridタイプを「 data 」に設定し、緑のアイコンをクリックして、システムユーザーの作成プロセスを完了します。
1. [configMgrを開きます。](http://localhost:4502/system/console/configMgr)
1. 「 Apache Sling Service User Mapper Service 」を検索し、クリックしてプロパティを開きます。
1. *+*&#x200B;アイコン（プラス）をクリックして、次のサービスマッピングを追加します。

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. 「保存」をクリックします。

上記の設定では、 DevelopingWithServiceUser.coreがバンドルのシンボリック名になります。 getresourceresolverはサブサービス名です。dataは、前の手順で作成したシステムユーザーです。

また、fd-serviceユーザーの代わりにリソースリゾルバーを取得することもできます。 このサービスユーザーは、ドキュメントサービスに使用されます。 例えば、使用権限等を認証/適用する場合、fd-serviceユーザーのリソースリゾルバーを使用して操作を実行します

1. [この記事に関連付けられているzipファイルをダウンロードして解凍します。](assets/developingwithserviceuser.zip)
1. [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)に移動します。
1. OSGiバンドルをアップロードして起動します。
1. バンドルがアクティブ状態であることを確認します。
1. これで、*システムユーザー*&#x200B;が正常に作成され、*サービスユーザーバンドル*&#x200B;もデプロイされました。

   /contentへのアクセスを提供するには、システムユーザー(「 data 」)にコンテンツノードに対する読み取り権限を付与します。

   1. [http://localhost:4502/useradmin](http://localhost:4502/useradmin)に移動します。
   1. ユーザー「 data 」を検索します。 これは、前の手順で作成したのと同じシステムユーザーです。
   1. ユーザーをダブルクリックし、「権限」タブをクリックします。
   1. 「コンテンツ」フォルダーへの「読み取り」アクセス権を付与します。
   1. サービスユーザーを使用して/contentフォルダーにアクセスするには、次のコードを使用します

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
   ResourceResolver serviceResolver = getResolver.getServiceResolver();
   // to get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();
   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
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
     HashMap<String, Object> param = new HashMap<String, Object>();
     param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
     ResourceResolver resolver = null;
     try {
      resolver = resolverFactory.getServiceResourceResolver(param);
     } catch (LoginException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
     }
     return resolver;
    }
   ```

