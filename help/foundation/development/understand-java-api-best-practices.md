---
title: AEM における Java™ API のベストプラクティス
description: AEM は、開発時に使用できる多数の Java™ API を公開している機能豊富なオープンソースソフトウェアスタックに基づいて構築されています。この記事では、主要な API とそれらを使用するタイミングおよび理由について説明します。
version: 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
doc-type: Article
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
duration: 416
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 100%

---

# Java™ API のベストプラクティス

Adobe Experience Manager（AEM）は、開発時に使用できる多数の Java™ API を公開していいる機能豊富なオープンソースソフトウェアスタックに基づいて構築されています。この記事では、主要な API とそれらを使用するタイミングおよび理由について説明します。

AEM は、4 つの主要な Java™ API セットに基づいて構築されています。

* **Adobe Experience Manager（AEM）**

   * 製品の抽象概念（ページ、アセット、ワークフローなど）。

* **Apache Sling Web フレームワーク**

   * REST およびリソースベースの抽象概念（リソース、値マップ、HTTP リクエストなど）。

* **JCR（Apache Jackrabbit Oak）**

   * データとコンテンツの抽象概念（ノード、プロパティ、セッションなど）。

* **OSGi（Apache Felix）**

   * OSGi アプリケーションコンテナの抽象概念（サービスや（OSGi）コンポーネントなど）。

## Java™ API 環境設定の「経験則」

一般的なルールとして、次の順序で API／抽象概念を優先します。

1. **AEM**
1. **Sling**
1. **JCR**
1. **OSGi**

API が AEM から提供される場合は、API を [!DNL Sling]、JCR および OSGi よりも優先します。AEM が API を提供しない場合は、[!DNL Sling] を JCR や OSGi よりも優先します。

この順序は一般的なルールで、例外が存在します。 このルールから逸脱する理由として容認できるものは、次のとおりです。

* よく知られた例外（以下で説明します）。
* 必要な機能が上位レベルの API で使用できない。
* あまり優先されない API をそれ自体で使用している既存コード（カスタムコードまたは AEM 製品コード ）のコンテキストで動作しており、新しい API に移行するためのコストが正当と認められない。

   * 混在させるよりも、低レベルの API を一貫して使用する方が適切です。

## AEM API

* [**AEM API Javadoc**](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

AEM API では、製品化されたユースケースに固有の抽象概念と機能を提供します。

例：AEM の [PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) API と [Page](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) API は、web ページを表す AEM の `cq:Page` ノードの抽象概念を提供します。  

これらのノードは [!DNL Sling] API を介してリソースとして使用でき、JCR API を介してノードとして使用できますが、AEM の API は一般的なユースケースの抽象概念を提供します。AEM API を使用することで、AEM 製品と AEM のカスタマイズおよび拡張機能の間で一貫した動作が保証されます。

### com.adobe.&#42; API と com.day.&#42; API の比較

AEM API には、次の Java™ パッケージ（優先度順）で示すように、パッケージ内の優先順位があります。

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` パッケージでは製品のユースケースをサポートしているのに対して、 `com.adobe.granite` では、（AEM Assets、Sites などの複数の製品を横断して使用される）ワークフローやタスクなど、複数の製品にまたがるプラットフォームのユースケースをサポートしています。

 `com.day.cq` パッケージには「オリジナル」の API が含まれています。 これらの API は、アドビが [!DNL Day CQ] を買収する前や買収前後に存在していた中核概念および機能に対応しています。これらの API はサポートされており、 `com.adobe.cq` または `com.adobe.granite` パッケージに（新しい）代替手段が用意されていない場合を除いて、使用を避けてください。

[!DNL Content Fragments] や [!DNL Experience Fragments] などの新しい抽象概念は、下記の `com.day.cq` ではなく `com.adobe.cq` 空間に構築されています。

### クエリ API

AEM では複数のクエリ言語をサポートしています。 主な言語は、[JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html)、XPath、[AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=ja) の 3 つです。

最も重要な関心事は、コードベース全体で一貫性のあるクエリ言語を維持して、理解するうえでの複雑さとコストを軽減することです。

すべてのクエリ言語は事実上同じパフォーマンスプロファイルを持っています。クエリの最終的な実行のために [!DNL Apache Oak] によってクエリ言語が JCR-SQL2 にトランスパイルされ、JCR-SQL2 への変換時間がクエリ時間そのものと比較して無視できるほど短いからです。

推奨される API は [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=ja) です。これは最高レベルの抽象概念であり、クエリを作成、実行およびクエリ結果を取得するための堅牢な API を提供しています。次のものが用意されています。

* 単純なパラメーター化されたクエリ構成（クエリパラメーターをマップとしてモデル化）
* ネイティブ [Java™ API および HTTP API](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=ja)
* [AEM Query Debugger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=ja)
* 一般的なクエリ要件をサポートする [AEM 述語](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html?lang=ja) 

* カスタム[クエリ述語](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=ja)の開発を可能にする拡張可能な API
* JCR-SQL2 と XPath は、[[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) および [JCR API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) を介して直接実行でき、それぞれ[[!DNL Sling] リソース](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)または [JCR ノード](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html)として結果を返します。

>[!CAUTION]
>
>AEM QueryBuilder API は ResourceResolver オブジェクトをリークします。 このリークを軽減するには、次の[コード例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164)に従います。
>

## [!DNL Sling] API

* [**Apache [!DNL Sling] API Javadoc**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) は、AEM を支える RESTful web フレームワークです。 [!DNL Sling] は、HTTP リクエストルーティングを提供したり、JCR ノードをリソースとしてモデル化したり、セキュリティコンテキストを提供したりするなど、多くの機能を提供します。

[!DNL Sling] API には、拡張に対応するように構築されているというメリットが付加されています。つまり、多くの場合、[!DNL Sling] API を使用して構築されたアプリケーションの動作は、拡張性のより低い JCR API よりも容易かつ安全に拡張できます。

### [!DNL Sling] API の一般的な用途

* [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) としてJCR ノードにアクセスし、[ValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html) を介してそのデータにアクセスします。

* [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html) を介してセキュリティコンテキストを提供します。
* ResourceResolver の [create／move／copy／delete メソッド](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)を介してリソースを作成および削除します。
* [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) を介してプロパティを更新します。
* リクエスト処理の構築ブロックを作成します。

   * [サーブレット](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [サーブレットフィルター](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 非同期作業処理の構築ブロックを作成します。

   * [イベントおよびジョブハンドラー](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [スケジューラー](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling モデル](https://sling.apache.org/documentation/bundles/models.html)

* [サービスユーザー](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html?lang=ja)

## JCR API

* **[JCR 2.0 Javadoc](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

[JCR（Java™ Content Repository）2.0 API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) は、JCR 実装（AEM の場合、[Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/)）の仕様の一部です。すべての JCR 実装は、これらの API に準拠し API を実装する必要があります。したがって、これは AEM のコンテンツを操作するための最も低レベルの API になります。

JCR 自体は階層型／ツリーベースの NoSQL データストアで、AEM ではこれをコンテンツリポジトリとして使用します。 JCR には、コンテンツの CRUD からコンテンツのクエリまで、サポートされている様々な API があります。 この堅牢な API にもかかわらず、AEM や [!DNL Sling] の高レベルの抽象概念より優先されることはまれです。

Apache Jackrabbit Oak API よりも JCR API を常に優先します。 JCR API が JCR リポジトリと&#x200B;***やり取り***&#x200B;するためのものであるのに対して、Oak API は JCR リポジトリを&#x200B;***実装***&#x200B;するためのものです。

### JCR API に関するよくある誤解

JCR は AEM のコンテンツリポジトリですが、JCR API はコンテンツを操作するための望ましい方法ではありません。 より優れた抽象概念を提供するので、代わりに AEM API（Page、Assets、Tag など）や Sling Resource API を優先します。

>[!CAUTION]
>
>AEM アプリケーションで JCR API の Session インターフェイスや Node ノードインターフェイスを幅広く使用することは、コードスメルになります。 代わりに [!DNL Sling] API を使用してください。

### JCR API の一般的な用途

* [アクセス制御管理](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html?lang=ja)
* [許可可能な管理（ユーザー／グループ）](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR の監視（JCR イベントのリッスン）
* ディープノード構造の作成

   * Sling API はリソースの作成をサポートしていますが、JCR API の [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) および [JcrUtil](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html) には、深い構造を迅速に作成できる便利なメソッドがあります。 

## OSGi API

* [**OSGi R6 Javadoc**](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declarative Services 1.2 コンポーネント注釈 Javadoc](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2 メタタイプ注釈 Javadoc](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi フレームワーク Javadoc**](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API と上位レベルの API（AEM、[!DNL Sling] および JCR）はほとんど重複せず、OSGi API を使用する必要はめったにありません。使用する場合は、AEM 開発の高度な専門知識が必要です。

### OSGi API と Apache Felix API の比較

OSGi では、すべての OSGi コンテナが実装し準拠する必要がある仕様を定義しています。 AEM の OSGi 実装である Apache Felix では、独自の API もいくつか提供しています。

* OSGi API（`org.osgi`）を Apache Felix API（`org.apache.felix`）より優先します。

### OSGi API の一般的な用途

* OSGi サービスおよびコンポーネントを宣言するための OSGi 注釈。

   * OSGi サービスおよびコンポーネントを宣言する場合は、[Felix SCR 注釈](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html)より [OSGi Declarative Services（DS）1.2 注釈](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)を優先します。

* [OSGi サービス／コンポーネントの登録／登録解除](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)をコード内で動的に行うための OSGi API。

   * 条件付きの OSGi サービス／コンポーネント管理が不要な場合は（ほとんどの場合は不要）、OSGi DS 1.2 注釈の使用をお勧めします。

## ルールの例外

上記の規則に対する一般的な例外を以下に示します。

### OSGi API

OSGi コンポーネントプロパティでの定義や読み取りなど、低レベルの OSGi 抽象概念を扱う場合、`org.osgi` で提供される新しい抽象概念は、上位レベルの Sling の抽象概念よりも優先されます。 競合する Sling の抽象概念は `@Deprecated` としてマークされておらず、`org.osgi` の代替案が推奨されています。

また、OSGi 設定ノードの定義では `sling:OsgiConfig` 形式より `cfg.json` が優先される点にも注意してください。

### AEM Asset API

* [`com.adobe.granite.asset.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html) より [`com.day.cq.dam.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html) を優先します。

   * `com.day.cq` Assets API には、AEM のアセット管理ユースケースに対するより相補的なツールが用意されているのに対して、。
   * Granite Assets API では、低レベルのアセット管理ユースケース（バージョン、関係）をサポートしています。

### クエリ API

* AEM QueryBuilder では、[候補](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions)、スペルチェック、インデックスヒントといったそれほど一般的でない機能など、特定のクエリ関数をサポートしていません。これらの機能を使用してクエリを実行する場合は、JCR-SQL2 をお勧めします。

### [!DNL Sling] サーブレットの登録 {#sling-servlet-registration}

* [!DNL Sling] サーブレットの登録では、`@SlingServlet` より [OSGi DS 1.2 注釈（@SlingServletResourceTypes を含む）](https://sling.apache.org/documentation/the-sling-engine/servlets.html)を優先します。

### [!DNL Sling] フィルター登録 {#sling-filter-registration}

* [!DNL Sling] フィルター登録では、`@SlingFilter` より [OSGi DS 1.2 注釈（@SlingServletFilter を含む）](https://sling.apache.org/documentation/the-sling-engine/filters.html)を優先します。

## 有用なコードスニペット

ここで紹介した API を使用する一般的なユースケースのベストプラクティスの例を示す有用な Java™ コードスニペットを以下に列挙します。また、これらのスニペットでは、優先度の低い API からより優先度の高い API に移行する方法も示しています。

### JCR Session から [!DNL Sling] ResourceResolver へ

#### Sling ResourceResolver の自動終了

AEM 6.2 以降、[!DNL Sling]ResourceResolver`AutoClosable` は [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) 文にあります。この構文を使用すると、`resourceResolver .close()` への明示的な呼び出しは不要です。

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

#### Sling ResourceResolver の手動終了

上記の方法で自動終了できない場合、ResourceResolvers は `finally` ブロックで手動終了する必要があります。

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

### JCR Path から [!DNL Sling] [!DNL Resource] へ

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### JCR Node から [!DNL Sling] [!DNL Resource] へ

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] から AEM Asset へ

#### 推奨アプローチ

この `DamUtil.resolveToAsset(..)` 関数は、必要に応じてツリーを上に移動して、`dam:Asset` 下のあらゆるリソースを Asset オブジェクトに解決します。

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 代替アプローチ

リソースを Asset に適合させるには、リソース自体が `dam:Asset` ノードである必要があります。

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Resource から AEM Page へ

#### 推奨アプローチ

`pageManager.getContainingPage(..)` は、必要に応じてツリーを上に移動して、`cq:Page` 下のあらゆるリソースを Page オブジェクトに解決します。

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 代替アプローチ {#alternative-approach-1}

リソースを Page に適合させるには、リソース自体が `cq:Page` ノードである必要があります。

```java
Page page = resource.adaptTo(Page.class);
```

### AEM Page のプロパティの読み取り

Page オブジェクトのゲッター（`getTitle()`、`getDescription()` など）を使用すると、よく知られているプロパティを取得でき、`page.getProperties()` を使用すると、その他のプロパティを取得するための `[cq:Page]/jcr:content` ValueMap を取得できます。

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### AEM Asset のメタデータプロパティの読み取り

Asset API には、`[dam:Asset]/jcr:content/metadata` ノードからプロパティを読み取るための便利なメソッドが用意されています。これは ValueMap ではなく、2 番目のパラメーター（デフォルト値、自動タイプキャスト）はサポートされていません。

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### [!DNL Sling] [!DNL Resource] のプロパティの読み取り {#read-sling-resource-properties}

AEM API（Page、Asset）から直接アクセスできない場所（プロパティまたは相対リソース）にプロパティが格納されている場合は、[!DNL Sling] Resource と ValueMap を使用してデータを取得できます。

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

この場合、目的のプロパティまたはサブリソースを効率的に見つけるために、AEM オブジェクトを [!DNL Sling] [!DNL Resource] に変換する必要がある可能性があります。

#### AEM Page から [!DNL Sling] [!DNL Resource] へ

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM Asset から [!DNL Sling] [!DNL Resource] へ

```java
Resource resource = asset.adaptTo(Resource.class);
```

### [!DNL Sling] の ModifiableValueMap を使用したプロパティの書き込み

[!DNL Sling] の [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) を使用すると、ノードにプロパティを書き込むことができます。これは、直接ノードにのみ書き込むことができます（相対的なプロパティパスはサポートされていません）。

なお、`.adaptTo(ModifiableValueMap.class)` への呼び出しにはリソースへの書き込み権限が必要であり、権限がない場合は null が返されます。

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### AEM Page オブジェクトの作成

AEM でページを適切に定義および初期化するために必要なページテンプレートを使用するので、必ず PageManager を使用してページを作成します。

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### [!DNL Sling] Resource オブジェクトの作成

ResourceResolver では、リソースを作成するための基本的な操作をサポートしています。上位レベルの抽象概念（AEM Page、Asset、Tag など）を作成する場合は、それぞれのマネージャーに用意されているメソッドを使用します。

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### [!DNL Sling] Resource オブジェクトの削除

ResourceResolver では、リソースの削除をサポートしています。上位レベルの抽象概念（AEM Page、Asset、Tag など）を作成する場合は、それぞれのマネージャーに用意されているメソッドを使用します。

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
