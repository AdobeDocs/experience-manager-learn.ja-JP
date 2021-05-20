---
title: AEM for SPA EditorとRemote SPAの設定
description: AEM SPA EditorがリモートSPAをオーサリングできるように、サポートする設定およびコンテンツ要件を設定するには、AEMプロジェクトが必要です。
topic: ヘッドレス、SPA、開発
feature: SPAエディター、コアコンポーネント、API、開発
role: Developer, Architect
level: Beginner
kt: 7631
thumbnail: kt-7631.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '1220'
ht-degree: 1%

---


# AEM for SPA Editorの設定

SPAコードベースはAEMの外部で管理されますが、サポートする設定およびコンテンツ要件を設定するには、AEMプロジェクトが必要です。 この章では、必要な設定を含むAEMプロジェクトの作成に関する手順を説明します。

+ AEM WCMコアコンポーネントプロキシ
+ AEM Remote SPA Page proxy
+ AEMリモートSPAページテンプレート
+ ベースラインリモートSPA AEMページ
+ SPAとAEM URLのマッピングを定義するサブプロジェクト
+ OSGi設定フォルダー

## AEMプロジェクトの作成

設定とベースラインコンテンツを管理するAEMプロジェクトを作成します。

_常に最新バージョンの [AEM Archetypeを使用してください](https://github.com/adobe/aem-project-archetype)。_


```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=27 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/wknd-app/wknd-app ~/Code/wknd-app/com.adobe.aem.guides.wknd-app
```

_最後のコマンドは、AEMプロジェクトフォルダーの名前を変更するだけで、AEMプロジェクトであることが明確になり、Remote SPA__

`frontendModule="react"`を指定した場合、 `ui.frontend`プロジェクトはRemote SPAの使用例には使用されません。 SPAは、AEMの外部で開発および管理され、AEMのみをコンテンツAPIとして使用します。 `frontendModule="react"`フラグは、`spa-project` AEM Java™の依存関係を含むプロジェクトで必要で、リモートSPAページテンプレートを設定します。

AEMプロジェクトアーキタイプは、SPAとの統合のためのAEMの設定に使用する、次の要素を生成します。

+ __AEM WCMコアコンポーネントの__ 特性  `ui.content/src/.../apps/wknd-app/components`
+ __AEM SPA Remote Page__ proxyat  `ui.content/src/.../apps/wknd-app/components/remotepage`
+ __AEM Page__ Templatesat  `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __コンテンツマッピングを定義するサブプロジ__ ェクト  `ui.content/src/...`
+ __ベースラインリモートSPA AEMペ__ ージ  `ui.content/src/.../content/wknd-app`
+ __OSGi設定フォル__ ダー  `ui.config/src/.../apps/wknd-app/osgiconfig`

基本のAEMプロジェクトが生成されると、SPA EditorとRemote SPAの互換性を確保するための調整がいくつかおこなわれます。

## ui.frontendプロジェクトの削除

SPAはリモートSPAなので、AEMプロジェクトの外部で開発および管理されているとします。 競合を避けるには、`ui.frontend`プロジェクトをデプロイから削除します。 `ui.frontend`プロジェクトが削除されない場合、`ui.frontend`プロジェクトとRemote SPAで提供されるデフォルトのSPAである2つのSPAが、AEM SPAエディターに同時に読み込まれます。

1. IDEでAEMプロジェクト(`~/Code/wknd-app/com.adobe.aem.guides.wknd-app`)を開きます。
1. ルート`pom.xml`を開きます。
1. `<modules>`リストから`<module>ui.frontend</module`をコメントアウトします。

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

   `pom.xml`ファイルは次のようになります。

   ![reactor pomからui.frontendモジュールを削除](./assets/aem-project/uifrontend-reactor-pom.png)

1. `ui.apps/pom.xml`を開きます。
1. `<artifactId>wknd-app.ui.frontend</artifactId>`の`<dependency>`をコメントアウトします。

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

   `ui.apps/pom.xml`ファイルは次のようになります。

   ![ui.appsからui.frontend依存関係を削除](./assets/aem-project/uifrontend-uiapps-pom.png)

これらの変更の前にAEMプロジェクトが構築された場合は、`ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`の`ui.apps`プロジェクトから手動で`ui.frontend`生成されたクライアントライブラリを削除します。

## AEMコンテンツマッピング

AEMがSPAエディターにリモートSPAを読み込むには、SPAルートと、コンテンツを開いてオーサリングするために使用されるAEMページ間のマッピングを確立する必要があります。

この設定の重要性については、後で説明します。

マッピングは、`/etc/map`で定義された[Slingマッピング](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1)を使用して実行できます。

1. IDEで、`ui.content`サブプロジェクトを開きます。
1. `src/main/content/jcr_root/etc` に移動します。
1. フォルダーの作成 `map`
1. `map`に、フォルダー`http`を作成します。
1. `http`に、内容を含むファイル`.content.xml`を作成します。

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. `http`に、フォルダー`localhost_any`を作成します。
1. `localhost_any`に、内容を含むファイル`.content.xml`を作成します。

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. `localhost_any`に、フォルダー`wknd-app-routes-adventure`を作成します。
1. `wknd-app-routes-adventure`に、内容を含むファイル`.content.xml`を作成します。

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages will be created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. マッピングノードをAEMパッケージに含まれる`ui.content/src/main/content/META-INF/vault/filter.xml`に追加します。

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

フォルダー構造と`.context.xml`ファイルは次のようになります。

![Slingマッピング](./assets/aem-project/sling-mapping.png)

`filter.xml`ファイルは次のようになります。

![Slingマッピング](./assets/aem-project/sling-mapping-filter.png)

現在は、AEMプロジェクトがデプロイされると、これらの設定が自動的に含まれます。

Slingマッピングは、`http`と`localhost`で実行されるAEMに影響するので、ローカル開発のみをサポートします。 AEMにCloud Serviceとしてデプロイする場合は、ターゲット`https`と適切なAEMをCloud Serviceドメインとして、類似のSlingマッピングを追加する必要があります。 詳しくは、[Slingマッピングのドキュメント](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html)を参照してください。

## クロスオリジンリソース共有セキュリティポリシー

次に、このSPAだけがAEMコンテンツにアクセスできるように、コンテンツを保護するようにAEMを設定します。 C AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html)で[クロスオリジンリソース共有を設定します。

1. IDEで、`ui.config` Mavenサブプロジェクトを開きます。
1. 移動する `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`という名前のファイルを作成します。
1. ファイルに以下を追加します。

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
           "Authorization"
       ]
   }
   ```

`com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`ファイルは次のようになります。

![SPAエディターのCORS設定](./assets/aem-project/cors-configuration.png)

主な設定要素は次のとおりです。

+ `alloworigin` AEMからコンテンツを取得できるホストを指定します。
   + `localhost:3000` ローカルで実行されるSPAをサポートするためにが追加されました。
   + `https://external-hosted-app` は、Remote SPAがホストされているドメインに置き換わるプレースホルダーとして機能します。
+ `allowedpaths` このCORS設定でカバーされるAEM内のパスを指定します。デフォルトでは、AEM内のすべてのコンテンツへのアクセスが許可されますが、次に例を示すように、SPAがアクセスできる特定のパスにのみアクセスできます。`/content/wknd-app`.

## AEMページをリモートSPAページテンプレートに設定

AEMプロジェクトアーキタイプは、AEMとリモートSPAの統合に適したプロジェクトを生成しますが、自動生成されるAEMページ構造を調整するには、小規模ではあるものの重要な調整が必要です。 自動生成されるAEMページのタイプは、__SPAページ__&#x200B;ではなく、__Remote SPA page__&#x200B;に変更する必要があります。

1. IDEで、`ui.content`サブプロジェクトを開きます。
1. `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`に開く
1. この`.content.xml`ファイルを次で更新します。

   ```
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

主な変更は、`jcr:content`ノードの更新です。

+ `cq:template`コピー先：`/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType`コピー先：`wknd-app/components/remotepage`

`src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`ファイルは次のようになります。

![ホームページの.content.xmlの更新](./assets/aem-project/home-content-xml.png)

これらの変更により、AEMのSPAルートであるこのページが、SPAエディターでリモートSPAを読み込むことができます。

>[!NOTE]
>
>このプロジェクトが以前AEMの場合は、`ui.content`プロジェクトが&#x200B;__update__&#x200B;ではなく&#x200B;__merge__&#x200B;ノードに設定されているので、必ずAEMページを&#x200B;__Sites/WKND App/ us/ en/WKND App Home Page__&#x200B;として削除してください。

このページは、AEM自体でリモートSPAページとして削除および再作成することもできますが、このページは`ui.content`プロジェクトで自動作成されるので、コードベースで更新することをお勧めします。

## AEM SDKへのAEMプロジェクトのデプロイ

1. AEMオーサーサービスがポート4502で実行されていることを確認します。
1. コマンドラインから、AEM Mavenプロジェクトのルートに移動します。
1. Mavenを使用して、ローカルのAEM SDKオーサーサービスにプロジェクトをデプロイします。

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn clean install -PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## ルートのAEMページの設定

AEM Projectをデプロイした状態で、SPA Editorを読み込むための最後の手順が1つあります。 AEMで、AEMプロジェクトアーキタイプで生成されたSPAルートに対応するAEMページ`/content/wknd-app/us/en/home`をマークします。

1. AEMオーサーにログインします。
1. __サイト/WKND App > us > en__&#x200B;に移動します。
1. __WKND App Home Page__&#x200B;を選択し、__Properties__&#x200B;をタップします。

   ![WKNDアプリのホームページ — プロパティ](./assets/aem-content/edit-home-properties.png)

1. 「__SPA__」タブに移動します。
1. 「__リモートSPA設定__」に入力します。
   + __SPA Host URL__:  `http://localhost:3000`
      + リモートSPAのルートへのURL

   ![WKNDアプリのホームページ — リモートSPA設定](./assets/aem-content/remote-spa-configuration.png)

1. __「保存して閉じる」__&#x200B;をタップします。

このページの種類を&#x200B;__リモートSPAページ__&#x200B;のタイプに変更しました。これにより、 __ページのプロパティ__&#x200B;に&#x200B;__SPA__&#x200B;タブが表示されます。

この設定は、SPAのルートに対応するAEMページでのみ設定する必要があります。 このページの下にあるすべてのAEMページが値を継承します。

## これで完了です

これで、AEM設定を準備し、ローカルのAEMオーサーにデプロイしました。 次の方法がわかりました。

+ `ui.frontend`内の依存関係をコメントアウトして、AEMプロジェクトアーキタイプで生成されたSPAを削除します。
+ SPAルートをAEMのリソースにマッピングするSlingマッピングのAEMへの追加
+ リモートSPAがAEMのコンテンツを使用できるようにするAEMクロスオリジンリソース共有セキュリティポリシーを設定します
+ ローカルのAEM SDKオーサーサービスにAEMプロジェクトをデプロイする
+ SPA Host URLページのプロパティを使用して、AEMページをRemote SPA rootとしてマークします

## 次の手順

AEMが設定されている場合、AEM SPA Editorを使用して、編集可能な領域をサポートするリモートSPA](./spa-bootstrap.md)をブートストラップすることに重点を置くことができます。[