---
title: AEM for SPA エディターと Remote SPA を設定
description: AEM SPA エディターがリモート SPA を作成できるように、設定およびコンテンツ要件を設定するには、AEM プロジェクトが必要です。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer
level: Beginner
jira: KT-7631
thumbnail: kt-7631.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 0bdb93c9-5070-483c-a34c-f2b348bfe5ae
duration: 297
hide: true
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1229'
ht-degree: 100%

---

# AEM for SPA エディターを設定

{{spa-editor-deprecation}}

SPA コードベースは AEM 外で管理されますが、サポートする設定およびコンテンツ要件を設定するには、AEM プロジェクトが必要です。 この章では、必要な設定を含む AEM プロジェクトの作成に関する手順を説明します。

* AEM WCM コアコンポーネントプロキシ
* AEM リモート SPA ページプロキシ
* AEM リモート SPA ページテンプレート
* ベースラインリモート SPA AEM ページ
* SPA から AEM URL へのマッピングを定義するサブプロジェクト
* OSGi 設定フォルダー

## GitHub から基本プロジェクトをダウンロードします

`aem-guides-wknd-graphql` プロジェクトを Github.com からダウンロードします。このプロジェクトで使用されるベースラインファイルが含まれます。

```
$ mkdir -p ~/Code
$ git clone https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd remote-spa-tutorial
```

## AEM プロジェクトを作成します。

設定とベースラインコンテンツを管理する AEM プロジェクトを作成します。 このプロジェクトは、複製された `aem-guides-wknd-graphql` プロジェクトの `remote-spa-tutorial` フォルダー内に生成されます。

_[AEM アーキタイプ](https://github.com/adobe/aem-project-archetype) の最新バージョンを常に使用してください。_

```
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=39 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/wknd-app ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app
```

_最後のコマンドは、AEM プロジェクトであることを明確にし、リモート SPA** と混同しないよう、AEM プロジェクトフォルダーの名前を変更するだけです

`frontendModule="react"` が指定されている場合、`ui.frontend` プロジェクトは、リモート SPA の使用例には使用されません。SPAは、AEM の外部的な開発と管理を行い、AEM をコンテンツ API としてのみ使用します。 `frontendModule="react"` フラグは、プロジェクトに `spa-project` AEM Java™ 依存関係を組み込み、リモート SPA ページ テンプレートを設定するために必要です。

AEM プロジェクトアーキタイプは、SPA と統合するために AEM を設定するのに使用される次の要素を生成します。

* `ui.apps/src/.../apps/wknd-app/components` の **AEM WCM コアコンポーネントプロキシ**
* `ui.apps/src/.../apps/wknd-app/components/remotepage` の **AEM SPA リモートページプロキシ**
* `ui.content/src/.../conf/wknd-app/settings/wcm/templates` の **AEM ページテンプレート**
* `ui.content/src/...` の **コンテンツマッピングを定義するサブプロジェクト**
* `ui.content/src/.../content/wknd-app` の **ベースラインリモート SPA AEM ページ**
* `ui.config/src/.../apps/wknd-app/osgiconfig` の **OSGi 設定フォルダー**

基本 AEM プロジェクトが生成されると、SPA エディターとリモート SPA の互換性を確保するための調整がいくつか行われます。

## ui.frontend プロジェクトを削除

SPA はリモート SPA なので、AEM プロジェクトの外部で開発および管理されていると仮定します。 競合を避けるために、`ui.frontend` プロジェクトをデプロイから削除します。`ui.frontend` プロジェクトを削除しないと、`ui.frontend` プロジェクトで提供されるデフォルトの SPA とリモート SPA の 2 つの SPA が AEM SPA エディターに同時に読み込まれます。

1. IDE で AEM プロジェクト（`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app`）を開きます
1. ルート `pom.xml` を開きます
1. `<modules>` リストから `<module>ui.frontend</module` をコメントアウトします

   ```
   <modules>
       <module>all</module>
       <module>core</module>
   
       <!-- <module>ui.frontend</module> -->
   
       <module>ui.apps</module>
       <module>ui.apps.structure</module>
       <module>ui.config</module>
       <module>ui.content</module>
       <module>it.tests</module>
       <module>dispatcher</module>
       <module>ui.tests</module>
       <module>analyse</module>
   </modules>
   ```

   `pom.xml` ファイルは次のようになります。

   ![reactor pom から ui.frontend モジュールを削除](./assets/aem-project/uifrontend-reactor-pom.png)

1. `ui.apps/pom.xml` を開きます
1. `<artifactId>wknd-app.ui.frontend</artifactId>` の `<dependency>` をコメントアウトします

   ```
   <dependencies>
   
       <!-- Remote SPA project will provide all frontend resources
       <dependency>
           <groupId>com.adobe.aem.guides.wkndapp</groupId>
           <artifactId>wknd-app.ui.frontend</artifactId>
           <version>${project.version}</version>
           <type>zip</type>
       </dependency>
       --> 
   </dependencies>
   ```

   `ui.apps/pom.xml` ファイルは次のようになります。

   ![ui.apps から ui.frontend 依存関係を削除](./assets/aem-project/uifrontend-uiapps-pom.png)

これらの変更の前に AEM プロジェクトが構築された場合は、`ui.frontend` で生成されたクライアントライブラリを `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react` の `ui.apps` プロジェクトから手動で削除します。

## AEM コンテンツマッピング

AEM がリモート SPA を SPA エディターに読み込むには、SPA のルートと、コンテンツを開いて作成するために使用される AEM ページとの間のマッピングを確立する必要があります。

この設定の重要性については、後で説明します。

マッピングは、`/etc/map` で定義された [Sling Mapping](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) で行うことができます。

1. IDE で `ui.content` サブプロジェクトを開きます
1. `src/main/content/jcr_root` に移動します
1. フォルダー `etc` を作成します
1. `etc` で、フォルダー `map` を作成します
1. `map` で、フォルダー `http` を作成します
1. `http` で、次の内容のファイル `.content.xml` を作成します。

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. `http` で、フォルダー `localhost_any` を作成します
1. `localhost_any` で、次の内容のファイル `.content.xml` を作成します。

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. `localhost_any` で、フォルダー `wknd-app-routes-adventure` を作成します
1. `wknd-app-routes-adventure` で、次の内容のファイル `.content.xml` を作成します。

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages are created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. マッピングノードを `ui.content/src/main/content/META-INF/vault/filter.xml` に追加して、AEM パッケージに含めます。

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd-app" mode="merge"/>
       <filter root="/content/wknd-app" mode="merge"/>
       <filter root="/content/dam/wknd-app/asset.jpg" mode="merge"/>
       <filter root="/content/experience-fragments/wknd-app" mode="merge"/>
   
       <!-- Add the Sling Mapping rules for the WKND App -->
       <filter root="/etc/map" mode="merge"/>
   </workspaceFilter>
   ```

フォルダー構造と `.context.xml` ファイルは次のようになります。

![Sling マッピング](./assets/aem-project/sling-mapping.png)

`filter.xml` ファイルは次のようになります。

![Sling マッピング](./assets/aem-project/sling-mapping-filter.png)

これで AEM プロジェクトがデプロイされると、これらの設定が自動的に含まれるようになりました。

Sling マッピングは `http` および `localhost` で実行される AEM に影響を与えるため、ローカル開発のみをサポートします。AEM as a Cloud Service にデプロイする場合は、`https` と適切な AEM as a Cloud Service ドメインをターゲットとする同様の Sling マッピングを追加する必要があります。 詳細については、[Sling マッピングのドキュメント](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html) を参照してください。

## クロスオリジンリソース共有セキュリティポリシー

次に、AEM を設定して、この SPA だけが AEM コンテンツにアクセスできるようにコンテンツを保護します。[クロスオリジンリソース共有を AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html?lang=ja) で設定します。

1. IDE で `ui.config` Maven サブプロジェクトを開きます
1. `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config` に移動します
1. `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` という名前のファイルを作成します。
1. 次の項目をファイルに追加します。

   ```
   {
       "supportscredentials":true,
       "exposedheaders":[
           ""
       ],
       "supportedmethods":[
           "GET",
           "HEAD",
           "POST",
           "OPTIONS"
       ],
       "alloworigin":[
           "https://external-hosted-app", "localhost:3000"
       ],
       "maxage:Integer":1800,
       "alloworiginregexp":[
           ".*"
       ],
       "allowedpaths":[
           ".*"
       ],
       "supportedheaders":[
           "Origin",
           "Accept",
           "X-Requested-With",
           "Content-Type",
           "Access-Control-Request-Method",
           "Access-Control-Request-Headers",
           "authorization"
       ]
   }
   ```

`com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` ファイルは次のようになります。

![SPA エディターの CORS 設定](./assets/aem-project/cors-configuration.png)

重要な設定要素は次のとおりです。

* `alloworigin` は、AEM からコンテンツを取得できるホストを指定します。
   * `localhost:3000` は、ローカルで実行されている SPA をサポートするために追加されています
   * `https://external-hosted-app` は、リモート SPA がホストされているドメインに置き換わるプレースホルダーとして機能します。
* `allowedpaths` は、この CORS 設定でカバーされる AEM 内のパスを指定します。デフォルトでは、AEM のすべてのコンテンツへのアクセスが許可されますが、これは、SPA がアクセスできる特定のパス（`/content/wknd-app` など）のみに限定できます。

## AEM ページをリモート SPA ページテンプレートとして設定します。

AEM プロジェクトアーキタイプは、AEM とリモート SPA の統合のためのプロジェクトを生成しますが、自動生成される AEM ページ構造を調整するには、小規模ではあるが重要な作業が必要です。 自動生成された AEM ページのタイプは、**SPA ページ** ではなく、**リモート SPA ページ** に変更する必要があります。

1. IDE で `ui.content` サブプロジェクトを開きます。
1. `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` に開きます
1. この `.content.xml` ファイルを次のように更新します。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="cq:Page">
       <jcr:content
           cq:template="/conf/wknd-app/settings/wcm/templates/spa-remote-page"
           jcr:primaryType="cq:PageContent"
           jcr:title="WKND App Home Page"
           sling:resourceType="wknd-app/components/remotepage">
           <root
               jcr:primaryType="nt:unstructured"
               sling:resourceType="wcm/foundation/components/responsivegrid">
               <responsivegrid
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wcm/foundation/components/responsivegrid">
                   <text
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="wknd-app/components/text"
                       text="&lt;p>Hello World!&lt;/p>"
                       textIsRich="true">
                       <cq:responsive jcr:primaryType="nt:unstructured"/>
                   </text>
               </responsivegrid>
           </root>
       </jcr:content>
   </jcr:root>
   ```

主な変更点は、`jcr:content` ノードの更新です。

* `cq:template`コピー先：`/conf/wknd-app/settings/wcm/templates/spa-remote-page`
* `sling:resourceType`コピー先：`wknd-app/components/remotepage`

`src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` ファイルは次のようになります。

![ホームページの .content.xml の更新](./assets/aem-project/home-content-xml.png)

これらの変更により、AEM の SPA ルートであるこのページが、SPA エディターでリモート SPA を読み込むことができます。

>[!NOTE]
>
>このプロジェクトが以前に AEM にデプロイされている場合は、`ui.content` プロジェクトが、**更新**&#x200B;ではなく、**マージ**&#x200B;ノードに設定されているため、AEM ページを&#x200B;**サイト／WKND アプリ／us／en／WKND アプリホームページ**&#x200B;で削除してください。

このページを削除して、AEM 自体のリモート SPA ページとして再作成することもできますが、このページは `ui.content` プロジェクトで自動作成されるため、コードベースで更新することをお勧めします。

## AEM SDK に AEM プロジェクトをデプロイします。

1. AEM オーサーサービスがポート 4502 で実行されていることを確認します。
1. コマンドラインから、AEM Maven プロジェクトのルートに移動します。
1. Maven を使用して、ローカルの AEM SDK オーサーサービスにプロジェクトをデプロイします。

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn clean install -PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## ルート AEM ページを設定します。

AEM プロジェクトがデプロイされたので、最後に、リモート SPA を読み込むように SPA エディターを準備するための手順が残っています。 AEM で、AEM プロジェクト アーキタイプで生成された SPA のルート `/content/wknd-app/us/en/home` に対応する AEM ページをマークします。

1. AEM オーサーにログインします。
1. **Sites／ WKND App／us／en** に移動します。
1. **WKND App Home Page** を選択し、「**プロパティ**」をタップします。

   ![WKND App Home Page - プロパティ](./assets/aem-content/edit-home-properties.png)

1. 「**SPA**」タブに移動します。
1. **リモート SPA 設定**&#x200B;の項目に入力します。
   1. **SPA ホスト URL**：`http://localhost:3000`
      1. リモート SPA のルートへの URL です。

   ![WKND App Home Page - リモート SPA 設定](./assets/aem-content/remote-spa-configuration.png)

1. 「**保存して閉じる**」をタップします。

なお、このページのタイプを&#x200B;**リモート SPA ページ**&#x200B;に変更し、**ページプロパティ**&#x200B;に「**SPA**」タブが表示されるようにしました。

この設定は、SPA のルートに対応する AEM ページでのみ設定する必要があります。このページの下にあるすべての AEM ページが値を継承します。

## これで完了です

これで、AEM の設定を準備して、ローカルの AEM オーサーにデプロイしました。次の方法を学習しました。

* AEM プロジェクトアーキタイプで生成された SPA を削除するには、`ui.frontend` の依存関係をコメントアウトします。
* SPA ルートを AEM のリソースにマッピングする Sling マッピングを AEM に追加します。
* リモート SPA が AEM のコンテンツを使用できるように、AEM のクロスオリジンリソース共有セキュリティポリシーを設定します。
* AEM プロジェクトをローカルの AEM SDK オーサーサービスにデプロイします。
* 「SPA ホスト URL」ページプロパティを使用して、AEM ページをリモート SPA のルートとしてマークします。

## 次の手順

AEM が設定されたので、AEM SPA エディターを使用して、編集可能な領域をサポートする[リモート SPA のブートストラップ](./spa-bootstrap.md)に専念できます。
