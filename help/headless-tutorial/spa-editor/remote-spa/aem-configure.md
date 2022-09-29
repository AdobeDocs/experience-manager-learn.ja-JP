---
title: AEM for SPA Editor と Remote SPAの設定
description: AEM SPA Editor がリモートSPAを作成できるように、設定およびコンテンツ要件をセットアップするには、AEMプロジェクトが必要です。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7631
thumbnail: kt-7631.jpeg
exl-id: 0bdb93c9-5070-483c-a34c-f2b348bfe5ae
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1215'
ht-degree: 2%

---

# AEM for SPA Editor の設定

SPAコードベースはAEM外で管理されますが、サポートする設定およびコンテンツ要件を設定するには、AEMプロジェクトが必要です。 この章では、必要な設定を含むAEMプロジェクトの作成に関する手順を説明します。

+ AEM WCM コアコンポーネントプロキシ
+ AEM Remote SPA Page proxy
+ AEM Remote SPA Page Templates
+ ベースラインリモートSPA AEMページ
+ SPAからAEM URL へのマッピングを定義するサブプロジェクト
+ OSGi 設定フォルダー

## AEMプロジェクトの作成

設定とベースラインコンテンツを管理するAEMプロジェクトを作成します。

_常に最新バージョンの [AEM Archetype](https://github.com/adobe/aem-project-archetype)._


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

_最後のコマンドは、AEMプロジェクトフォルダーの名前を変更するだけで、AEMプロジェクトであることが明確になり、Remote SPAと混同しないようにします__

While `frontendModule="react"` が指定されている場合、 `ui.frontend` プロジェクトは、Remote SPAの使用例には使用されません。 SPAは、AEMの外部的な開発と管理を行い、AEMをコンテンツ API としてのみ使用します。 この `frontendModule="react"` フラグが必要なのは、  `spa-project` AEM Java™の依存関係を確認し、リモートSPAページテンプレートを設定します。

AEMプロジェクトアーキタイプは、AEMをSPAと統合するように設定するために使用する、次の要素を生成します。

+ __AEM WCM コアコンポーネントプロキシ__ 時刻 `ui.apps/src/.../apps/wknd-app/components`
+ __AEM SPA Remote Page Proxy__ 時刻 `ui.apps/src/.../apps/wknd-app/components/remotepage`
+ __AEM Page Templates__ 時刻 `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __コンテンツマッピングを定義するサブプロジェクト__ 時刻 `ui.content/src/...`
+ __ベースラインリモートSPA AEMページ__ 時刻 `ui.content/src/.../content/wknd-app`
+ __OSGi 設定フォルダー__ 時刻 `ui.config/src/.../apps/wknd-app/osgiconfig`

基本AEMプロジェクトが生成されると、SPA Editor と Remote SPAの互換性を確保するための調整がいくつかおこなわれます。

## ui.frontend プロジェクトを削除

SPAは Remote SPAなので、AEMプロジェクトの外部で開発および管理されていると仮定します。 競合を避けるには、 `ui.frontend` プロジェクトをデプロイできませんでした。 この `ui.frontend` プロジェクトが削除されず、2 つのSPA( `ui.frontend` プロジェクトと Remote SPAは、AEM SPA Editor に同時に読み込まれます。

1. AEMプロジェクト (`~/Code/wknd-app/com.adobe.aem.guides.wknd-app`) を IDE に追加します。
1. ルートを開く `pom.xml`
1. コメントを `<module>ui.frontend</module` 外から `<modules>` リスト

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

   この `pom.xml` ファイルは次のようになります。

   ![reactor pom から ui.frontend モジュールを削除](./assets/aem-project/uifrontend-reactor-pom.png)

1. を開きます。 `ui.apps/pom.xml`
1. コメントアウト `<dependency>` オン `<artifactId>wknd-app.ui.frontend</artifactId>`

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

   この `ui.apps/pom.xml` ファイルは次のようになります。

   ![ui.apps から ui.frontend 依存関係を削除](./assets/aem-project/uifrontend-uiapps-pom.png)

これらの変更の前にAEMプロジェクトが構築されていた場合は、手動で `ui.frontend` 生成されたクライアントライブラリ `ui.apps` ～に投影する `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`.

## AEMコンテンツマッピング

AEMがSPAエディターにリモートSPAを読み込むには、SPAルートと、コンテンツの開封と作成に使用されるAEMページとの間のマッピングを確立する必要があります。

この設定の重要性については、後で説明します。

マッピングは、 [Sling マッピング](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) 次で定義： `/etc/map`.

1. IDE で、 `ui.content` subproject
1. `src/main/content/jcr_root` に移動します。
1. フォルダーの作成 `etc`
1. In `etc`、フォルダーの作成 `map`
1. In `map`、フォルダーの作成 `http`
1. In `http`、ファイルの作成 `.content.xml` 次の内容で、

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. In `http` 、フォルダーの作成 `localhost_any`
1. In `localhost_any`、ファイルの作成 `.content.xml` 次の内容で、

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. In `localhost_any` 、フォルダーの作成 `wknd-app-routes-adventure`
1. In `wknd-app-routes-adventure`、ファイルの作成 `.content.xml` 次の内容で、

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

1. マッピングノードを次に追加： `ui.content/src/main/content/META-INF/vault/filter.xml` をAEMパッケージに含めます。

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

この `filter.xml` ファイルは次のようになります。

![Sling マッピング](./assets/aem-project/sling-mapping-filter.png)

現在は、AEMプロジェクトがデプロイされると、これらの設定が自動的に含まれます。

Sling マッピングエフェクトAEMが `http` および `localhost`の場合は、ローカル開発のみをサポートします。 AEM as a Cloud Serviceにデプロイする場合は、同様の Sling マッピングをターゲットに追加する必要があります `https` および適切なAEMas a Cloud Serviceドメイン。詳しくは、 [Sling マッピングドキュメント](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html).

## クロスオリジンリソース共有セキュリティポリシー

次に、このSPAだけがAEMコンテンツにアクセスできるように、コンテンツを保護するようにAEMを設定します。 設定 [AEMでのクロスオリジンリソース共有](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html?lang=ja).

1. IDE で、 `ui.config` Maven サブプロジェクト
1. 移動する `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. という名前のファイルを作成します。 `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`
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

この `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` ファイルは次のようになります。

![SPAエディターの CORS 設定](./assets/aem-project/cors-configuration.png)

主な設定要素は次のとおりです。

+ `alloworigin` AEMからコンテンツを取得できるホストを指定します。
   + `localhost:3000` ローカルで実行されるSPAをサポートするために、が追加されました。
   + `https://external-hosted-app` は、Remote SPAがホストされているドメインに置き換わるプレースホルダーとして機能します。
+ `allowedpaths` この CORS 設定でカバーされるAEM内のパスを指定します。 デフォルトでは、AEM内のすべてのコンテンツへのアクセスが許可されますが、次のように、SPAがアクセスできる特定のパスのみに対して範囲を指定することもできます。 `/content/wknd-app`.

## AEMページをリモートSPAページテンプレートとして設定

AEMプロジェクトアーキタイプは、AEMとリモートSPAの統合のためのプロジェクトを生成しますが、自動生成されるAEMページ構造を調整するには、小規模ではあるが重要な作業が必要です。 自動生成されるAEMページのタイプをに変更する必要があります。 __リモートSPAページ__&#x200B;ではなく __SPAページ__.

1. IDE で、 `ui.content` subproject
1. 開く場所 `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. 更新 `.content.xml` 次のファイルを含む：

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

主な変更点は、 `jcr:content` ノードの：

+ `cq:template`コピー先：`/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType`コピー先：`wknd-app/components/remotepage`

この `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` ファイルは次のようになります。

![ホームページの.content.xml の更新](./assets/aem-project/home-content-xml.png)

これらの変更により、AEMのSPAルートであるこのページが、SPAエディターで Remote SPAを読み込むことができます。

>[!NOTE]
>
>このプロジェクトが以前にAEMにデプロイされている場合は、AEMページを必ず __サイト/ WKND アプリ/米国/英語/ WKND アプリのホームページ__、 `ui.content`  プロジェクトが __結合__ ノードは、ではなく __更新__.

このページはAEM自体でリモートSPAページとして削除し、再作成することもできますが、このページは `ui.content` プロジェクトを更新する場合は、コードベースを使用することをお勧めします。

## AEM SDK へのAEMプロジェクトのデプロイ

1. AEM オーサーサービスがポート 4502 で実行されていることを確認します。
1. コマンドラインから、AEM Maven プロジェクトのルートに移動します。
1. Maven を使用して、ローカルのAEM SDK Author サービスにプロジェクトをデプロイします。

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn clean install -PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## ルートAEMページの設定

AEMプロジェクトがデプロイされた状態で、SPA Editor を準備してリモートSPAを読み込むための最後の手順が 1 つあります。 AEMで、SPAルートに対応するAEMページをマークします。`/content/wknd-app/us/en/home`( AEMプロジェクトアーキタイプで生成されます )

1. AEM オーサーにログインします。
1. に移動します。 __サイト/ WKND アプリ/米国/en__
1. を選択します。 __WKND アプリのホームページ__&#x200B;をタップし、 __プロパティ__

   ![WKND アプリのホームページ — プロパティ](./assets/aem-content/edit-home-properties.png)

1. 次に移動： __SPA__ タブ
1. 次の項目に入力します。 __リモートSPA設定__
   + __SPA Host URL__: `http://localhost:3000`
      + リモートSPAのルートへの URL

   ![WKND アプリホームページ — リモートSPA設定](./assets/aem-content/remote-spa-configuration.png)

1. タップ __保存して閉じる__

このページのタイプをのタイプに変更したことに注意してください __リモートSPAページ__&#x200B;それが我々が見ることを可能にする __SPA__ タブ __ページプロパティ__.

この設定は、SPAのルートに対応するAEMページでのみ設定する必要があります。 このページの下にあるすべてのAEMページが値を継承します。

## これで完了です

これで、AEM設定が準備され、ローカルのAEMオーサーにデプロイされました。 次の方法を理解できました。

+ AEMプロジェクトアーキタイプで生成されたSPAを削除します。それには、 `ui.frontend`
+ SPAルートをAEMのリソースにマッピングする Sling マッピングをAEMに追加
+ リモートSPAがAEMのコンテンツを使用できるように、AEMクロスオリジンリソース共有セキュリティポリシーを設定します
+ ローカルのAEM SDK Author サービスにAEMプロジェクトをデプロイする
+ SPA Host URL ページプロパティを使用して、AEMページを Remote SPAルートとしてマークします

## 次の手順

AEMが設定されている場合は、 [リモートSPAのブートストラップ](./spa-bootstrap.md) AEM SPA Editor を使用した編集可能領域のサポート
