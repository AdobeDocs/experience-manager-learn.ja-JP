---
title: AEMのJava APIのベストプラクティスについて
description: AEMは、開発中に使用する多数のJava APIを公開する、リッチなオープンソースソフトウェアスタック上に構築されています。 この記事では、主要なAPIと、その用途をいつ、なぜ説明します。
version: 6.2, 6.3, 6.4, 6.5
sub-product: 基盤，アセット，サイト
feature: API
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
topic: 開発
role: デベロッパー
level: 初心者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 8%

---


# Java APIのベストプラクティスについて

Adobe Experience Manager（AEM）は、開発時に使用可能な多数の Java API を公開する豊富なオープンソースソフトウェアスタックに基づいて構築されています。この記事では、主要なAPIと、その用途をいつ、なぜ説明します。

AEMは、4つのプライマリJava APIセットを基に構築されています。

* **Adobe Experience Manager (AEM)**

   * ページ、アセット、ワークフローなどの製品の抽象概念

* **[!DNL Apache Sling]Webフレームワーク**

   * リソース、値マップ、HTTP要求など、RESTおよびリソースベースの抽象概念。

* **JCR ([!DNL Apache Jackrabbit Oak])**

   * ノード、プロパティ、セッションなどのデータおよびコンテンツの抽象概念。

* **[!DNL OSGi (Apache Felix)]**

   * サービスや(OSGi)コンポーネントなどのOSGiアプリケーションコンテナ抽象概念。

## Java APIプリファレンス「サムのルール」

一般的な規則は、API/抽象を次の順序で優先することです。

1. **AEM**
1. **[!DNL Sling]**
1. **JCR**
1. **OSGi**

AEMがAPIを提供する場合は、[!DNL Sling]、JCR、OSGiよりもAPIを優先します。 AEMがAPIを提供しない場合は、JCRやOSGiよりも[!DNL Sling]を好みます。

この順序は一般的な規則で、例外が存在することを意味します。 このルールから外す理由としては、次のようなものがあります。

* 以下に説明する、よく知られている例外。
* 必要な機能は、より高いレベルのAPIでは使用できません。
* それ自体があまり好ましくないAPIを使用する既存のコード(カスタムまたはAEM製品コード)のコンテキストで動作するので、新しいAPIに移行する際のコストは不当です。

   * ミックスを作成するよりも低レベルのAPIを一貫して使用する方が良いです。

## AEM API

* [**AEM API JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM APIは、生産された使用例に特有の抽象的な機能と機能を提供します。

例えば、AEM [PageManager](https://helpx.adobe.com/jp/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html)および[Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html)APIは、Webページを表すAEMの`cq:Page`ノードの抽象概念を提供します。

これらのノードはリソースとして[!DNL Sling] APIを介して、ノードとしてJCR APIを介して使用できますが、AEM APIは一般的な使用例の抽象概念を提供します。 AEM APIを使用すると、AEM製品間での動作、およびAEMのカスタマイズと拡張機能の一貫性が保たれます。

### com.adobe.*とcom.day* API

AEM APIのパッケージ内環境設定は、次のJavaパッケージで識別され、優先順に示されます。

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` ワークフローやタスク（製品間で使用される）などの製品間の使用例を `com.adobe.granite` サポートするのに対し、製品の使用例をサポートします。(AEM Assets、サイトなど)

`com.day.cq` には、「元の」APIが含まれます。これらのAPIは、Adobeが[!DNL Day CQ]を獲得する前または/またはその前後に存在した、主要な抽象概念と機能に対応しています。 これらのAPIはサポートされており、`com.adobe.cq`または`com.adobe.granite`が（新しい）代替手段を提供しない限り、避けないでください。

[!DNL Content Fragments]や[!DNL Experience Fragments]などの新しい抽象概念は、以下に説明する`com.day.cq`ではなく、`com.adobe.cq`空間に構築されます。

### クエリAPI

AEMは複数のクエリ言語をサポートしています。 3つの主な言語は、[JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html)、XPathおよび[AEMクエリビルダー](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/querybuilder-api.html)です。

最も重要な懸念事項は、コードベース全体で一貫したクエリ言語を維持し、理解するための複雑さとコストを軽減することです。

すべてのクエリ言語は、最終的なクエリ実行のために[!DNL Apache Oak]JCR-SQL2にトランスパイルするのと同じパフォーマンスプロファイルを持ち、JCR-SQL2への変換時間はクエリ時間そのものと比べてごくわずかです。

推奨されるAPIは[AEMクエリビルダー](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html)です。これは最高レベルの抽象で、クエリの結果を構築、実行、取得するための堅牢なAPIを提供し、次の機能を提供します。

* 単純なパラメータ化クエリ構造(Mapとしてモデル化されたクエリパラメータ)
* ネイティブ[Java APIおよびHTTP API](https://helpx.adobe.com/jp/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [OOTBクエリデバッガ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [OOTB](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) 予測一般的なクエリ要件のサポート

* 拡張可能なAPI。カスタム[クエリ述語](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)の開発を可能にします。
* JCR-SQL2およびXPathは、[[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-)および[JCR APIs](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html)を介して直接実行でき、それぞれ[[!DNL Sling] Resources](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)または[JCRノード](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html)が返されます。

>[!CAUTION]
>
>AEM QueryBuilder APIは、ResourceResolverオブジェクトをリークします。 このリークを軽減するために、[コードサンプル](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164)を参照してください。


## [!DNL Sling] API

* [**Apache  [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[ [!DNL Sling]](https://sling.apache.org/) Apacheisは、AEMを支えるRESTful Webフレームワークです。[!DNL Sling] HTTPリクエストルーティングを提供し、JCRノードをリソースとしてモデル化し、セキュリティコンテキストを提供します。その他多くの機能を提供します。

[!DNL Sling] APIには拡張用に構築されるという利点が追加されています。つまり、拡張性の低いJCR APIよりも、多くの場合、 [!DNL Sling] APIを使用して構築されたアプリケーションの動作を増強する方が簡単で安全です。

### [!DNL Sling] APIの一般的な使用

* [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)としてJCRノードにアクセスし、[ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html)を介してデータにアクセスします。

* [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)を介してセキュリティコンテキストを提供します。
* ResourceResolverの[create/move/copy/deleteメソッド](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)を使用したリソースの作成と削除。
* [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)を使用してプロパティを更新しています。
* 要求処理構築ブロックの構築

   * [サーブレット](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [サーブレットフィルター](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 非同期作業処理の構築ブロック

   * [イベントとジョブハンドラ](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [スケジューラー](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling Model](https://sling.apache.org/documentation/bundles/models.html)

* [サービスユーザー](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

[JCR (Java Content Repository) 2.0 APIs](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)は、JCR実装の仕様の一部です(AEMの場合は[Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/))。 すべてのJCR実装は、これらのAPIに準拠し実装する必要があります。したがって、AEMコンテンツと対話するための最も低レベルのAPIです。

JCR自体は、階層/ツリーベースのNoSQLデータストアAEMがコンテンツリポジトリとして使用します。 JCRには、コンテンツCRUDからコンテンツのクエリまで、サポートされるAPIが多数あります。 この堅牢なAPIにもかかわらず、上位レベルのAEMや[!DNL Sling]の抽象概念よりも優先されることは稀です。

Apache Jackrabbit Oak APIよりもJCR APIを常に優先します。 JCR APIはJCRリポジトリと&#x200B;***対話型の***&#x200B;用ですが、Oak APIはJCRリポジトリを&#x200B;***実装する***&#x200B;用です。

### JCR APIに対する一般的な誤解

JCRはAEMコンテンツリポジトリですが、そのAPIはコンテンツを操作するための推奨される方法ではありません。 代わりに、AEM API（ページ、アセット、タグなど）を またはSling Resource APIを使用して、より優れた抽象概念を提供します。

>[!CAUTION]
>
>AEMアプリケーションでJCR APIのセッションインターフェイスとノードインターフェイスを広く使用することは、コード臭です。 代わりに[!DNL Sling] APIを使用しないでください。

### JCR APIの一般的な使用

* [アクセス制御管理](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [承認可能な管理（ユーザー/グループ）](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCRの観察(JCRイベントのリッスン)
* ディープノード構造の作成

   * Sling APIはリソースの作成をサポートしますが、JCR APIは、[JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html)と[JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html)に便利なメソッドを備えており、ディープ構造の作成を促進します。

## OSGi API

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declarative Services 1.2コンポーネント注釈JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2メタ型注釈JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGiフレームワークJavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi APIと上位レベルのAPI(AEM、[!DNL Sling]、JCR)との間で重複が少なく、OSGi APIを使用する必要性は稀で、高いレベルのAEM開発知識が必要です。

### OSGiとApache Felix API

OSGiは、すべてのOSGiコンテナが実装し、準拠する必要のある仕様を定義します。 AEM OSGi実装のApache Felixでは、独自のAPIもいくつか提供しています。

* Apache Felix APIよりOSGi API (`org.osgi`)を優先(`org.apache.felix`)

### OSGi APIの一般的な使用

* OSGiサービスおよびコンポーネントを宣言するためのOSGi注釈。

   * OSGiサービスとコンポーネントを宣言するには、[Felix SCR Annotations](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html)よりも[OSGi Declarative Services (DS) 1.2 Annotations](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)を優先

* 動的にインコードで動作する[OSGiサービス/コンポーネントの登録を解除する/OSGi API](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)。

   * 条件付きOSGiサービス/コンポーネントの管理が不要な場合（ほとんどの場合）は、OSGi DS 1.2 Annotationの使用を推奨します。

## ルールの例外

上記のルールに対する一般的な例外を次に示します。

### AEM Asset API

* [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html)より[ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html)を優先します。

   * `com.day.cq` Assets APIはAEMのアセット管理の使用例に対して、より優れたツールを提供します。
   * Granite Assets APIは、低レベルのアセット管理の使用例（バージョン、リレーション）をサポートしています。

### クエリAPI

* AEM QueryBuilderは、[提案](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions)、スペルチェック、インデックスヒントなど、一般的でない他の関数には、特定のクエリ機能をサポートしていません。 これらの関数をクエリするには、JCR-SQL2を使用することをお勧めします。

### [!DNL Sling] サーブレット登録  {#sling-servlet-registration}

* [!DNL Sling] サーブレット登録， @SlingServletResourceTypesoverを使用した [OSGi DS 1.2注釈を](https://sling.apache.org/documentation/the-sling-engine/servlets.html) 優先  `@SlingServlet`

### [!DNL Sling] フィルタ登録  {#sling-filter-registration}

* [!DNL Sling] フィルター登録、@SlingServletFilteroverを含む [OSGi DS 1.2注釈を](https://sling.apache.org/documentation/the-sling-engine/filters.html) 優先  `@SlingFilter`

## 便利なコードスニペット

次のJavaコードスニペットは、これまで説明してきたAPIを使用する一般的な使用例のベストプラクティスを示します。 また、これらのスニペットでは、あまり好まれないAPIからより好ましいAPIに移行する方法を説明します。

### [!DNL Sling] ResourceResolverへのJCRセッション

#### Sling ResourceResolverを自動終了する

AEM 6.2以降、[!DNL Sling] ResourceResolverは、[try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)ステートメントの`AutoClosable`です。 この構文を使用する場合、`resourceResolver .close()`を明示的に呼び出す必要はありません。

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

try (ResourceResolver resourceResolver = rrf.getResourceResolver(authInfo)) {
    // Do work with the resourceResolver
} catch (LoginException e) { .. }
```

#### 手動で閉じたSling ResourceResolver

上記の自動閉じ方法を使用できない場合は、ResourceResolversを`finally`ブロックで手動で閉じる必要があります。

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

ResourceResolver resourceResolver = null;

try {
    resourceResolver = rrf.getResourceResolver(authInfo);
    // Do work with the resourceResolver
} catch (LoginException e) {
   ...
} finally {
    if (resourceResolver != null) { resourceResolver.close(); }
}
```

### JCR Path to [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### [!DNL Sling] [!DNL Resource]へのJCRノード

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] AEM Asset

#### 推奨される方法

`DamUtil.resolveToAsset(..)`必要に応じてツリーを上に移動するこ `dam:Asset` とで、アセットオブジェクトの下にあるリソースを解決します。

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 別の方法

リソースをアセットに適応させるには、リソース自体が`dam:Asset`ノードである必要があります。

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] リソースからAEMページ

#### 推奨される方法

`pageManager.getContainingPage(..)` 必要に応じてツリーを上に移動するこ `cq:Page` とで、ページオブジェクト以下のリソースを解決します。

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 代替手法{#alternative-approach-1}

リソースをページに適応させるには、リソース自体が`cq:Page`ノードである必要があります。

```java
Page page = resource.adaptTo(Page.class);
```

### AEMページのプロパティを読む

Pageオブジェクトのgetterを使用して、よく知られているプロパティ（`getTitle()`、`getDescription()`など）を取得します。 と`page.getProperties()`を呼び出して、他のプロパティを取得する`[cq:Page]/jcr:content` ValueMapを取得します。

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### AEMアセットメタデータプロパティの読み取り

アセットAPIは、`[dam:Asset]/jcr:content/metadata`ノードからプロパティを読み取る便利なメソッドを提供します。 これはValueMapではないので、2番目のパラメータ（デフォルト値、自動タイプキャスト）はサポートされていません。

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### [!DNL Sling] [!DNL Resource]プロパティ{#read-sling-resource-properties}を読み取ります

プロパティがAEM API（ページ、アセット）が直接アクセスできない場所（プロパティまたは相対リソース）に格納されている場合、[!DNL Sling] ResourcesとValueMapsを使用してデータを取得できます。

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

この場合、目的のプロパティまたはサブリソースを効率的に見つけるために、AEMオブジェクトを[!DNL Sling] [!DNL Resource]に変換する必要があります。

#### AEM [!DNL Sling] [!DNL Resource]へのページ

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM Asset to [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### [!DNL Sling]のModifiableValueMapを使用してプロパティを書き込む

[!DNL Sling]の[ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)を使用して、ノードにプロパティを書き込みます。 これは、即時ノードにのみ書き込むことができます（相対プロパティパスはサポートされていません）。

`.adaptTo(ModifiableValueMap.class)`への呼び出しには、リソースへの書き込み権限が必要です。そうでない場合、nullが返されます。

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### AEMページの作成

AEMでページを適切に定義し初期化するには、必ずPageManagerを使用して、ページテンプレートに従ってページを作成します。

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### [!DNL Sling]リソースの作成

ResourceResolverは、リソース作成の基本操作をサポートしています。 より高度な抽象概念(AEMページ、アセット、タグなど)を作成する場合、 各マネージャが提供する方法を使用します。

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### [!DNL Sling]リソースの削除

ResourceResolverは、リソースの削除をサポートしています。 より高度な抽象概念(AEMページ、アセット、タグなど)を作成する場合、 各マネージャが提供する方法を使用します。

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
