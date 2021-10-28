---
title: AEMでの Java API のベストプラクティス
description: AEMは、開発時に使用する多くの Java API を公開する、豊富なオープンソースソフトウェアスタックに基づいて構築されています。 この記事では、主な API、およびそれらを使用するタイミングと理由について説明します。
version: 6.2, 6.3, 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
source-git-commit: 967bcf3c4046a17303eb2fe70d7156267a7cbed7
workflow-type: tm+mt
source-wordcount: '2074'
ht-degree: 6%

---

# Java API のベストプラクティス

Adobe Experience Manager（AEM）は、開発時に使用可能な多数の Java API を公開する豊富なオープンソースソフトウェアスタックに基づいて構築されています。この記事では、主な API、およびそれらを使用するタイミングと理由について説明します。

AEMは、4 つのプライマリ Java API セットを基に構築されています。

* **Adobe Experience Manager (AEM)**

   * 製品の抽象概念（ページ、アセット、ワークフローなど）。

* **Apache Sling Web Framework**

   * リソース、値マップ、HTTP 要求など、REST およびリソースベースの抽象概念。

* **JCR(Apache Jackrabbit Oak)**

   * ノード、プロパティ、セッションなどのデータおよびコンテンツの抽象概念。

* **OSGi(Apache Felix)**

   * OSGi アプリケーションコンテナの抽象概念 ( サービスや (OSGi) コンポーネントなど )。

## Java API 環境設定「経験則」

一般的なルールとして、次の順序で API/抽象を優先します。

1. **AEM**
1. **Sling**
1. **JCR**
1. **OSGi**

AEMで API が提供されている場合は、より優先します。 [!DNL Sling]、JCR、OSGi の 3 つのリストが含まれます。 AEMに API がない場合は、 [!DNL Sling] JCR および OSGi 上で

この順序は一般的なルールで、例外が存在します。 このルールから解除する理由として許容できるものは次のとおりです。

* 以下に説明するよく知られた例外。
* 必要な機能は、上位レベルの API では使用できません。
* それ自体であまり優先されない API を使用している既存のコード ( カスタムまたはAEM製品コード ) のコンテキストで動作し、新しい API に移行するためのコストは不当です。

   * 混在を作成するよりも、低レベルの API を一貫して使用する方が効果的です。

## AEM API

* [**AEM API JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM API は、製品化された使用例に固有の抽象概念と機能を提供します。

例： AEM [PageManager](https://www.adobe.io/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) および [ページ](https://www.adobe.io/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) API は以下の抽象概念を提供します。 `cq:Page` web ページを表すAEMのノード。

これらのノードはで使用できますが、 [!DNL Sling] AEM API は、リソースとして API を、ノードとして JCR API を提供し、一般的な使用例の抽象概念を提供します。 AEM API を使用すると、AEMと製品の間で、カスタマイズとAEMの拡張の間で一貫した動作がおこなわれます。

### com.adobe.*と com.day の比較。* API

AEM API は、パッケージ内の環境設定を持ち、次の Java パッケージで識別されます（好みの順）。

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` は製品のユースケースをサポートし、は `com.adobe.granite` は、ワークフローやタスクなど、製品をまたいだプラットフォームの使用例（製品全体で使用される）をサポートします。AEM Assets、サイトなど )。

`com.day.cq` には、「オリジナル」の API が含まれています。 これらの API は、Adobeが [!DNL Day CQ]. これらの API はサポートされており、回避する必要はありません ( ただし、 `com.adobe.cq` または `com.adobe.granite` （新しい）代替案を提供する。

次のような新しい抽象概念 [!DNL Content Fragments] および [!DNL Experience Fragments] は `com.adobe.cq` ～ではなく空間 `com.day.cq` 以下に説明します。

### クエリ API

AEMは複数のクエリ言語をサポートしています。 3 つの主要言語は、 [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html)、XPath および [AEM Query Builder](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

最も重要な問題は、コードベース全体で一貫性のあるクエリ言語を維持し、理解するための複雑さとコストを軽減することです。

すべてのクエリ言語は、 [!DNL Apache Oak] 最終的なクエリ実行のために JCR-SQL2 にトランスパイルします。JCR-SQL2 への変換時間は、クエリ時間自体に比べて非常に短い時間です。

推奨される API は次のとおりです。 [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html)：最高レベルの抽象化で、クエリの結果を構築、実行および取得するための堅牢な API を提供し、次の機能を提供します。

* 単純なパラメータ化されたクエリ構造（Map としてモデル化されたクエリパラメータ）
* ネイティブ [Java API と HTTP API](https://helpx.adobe.com/jp/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [AEM Query Debugger](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [AEM述語](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) 共通のクエリ要件のサポート

* 拡張可能な API （カスタムの開発に使用） [クエリ述語](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* JCR-SQL2 および XPath は、 [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) および [JCR API](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html)、結果を返す [[!DNL Sling] リソース](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) または [JCR ノード](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html)、それぞれ。

>[!CAUTION]
>
>AEM QueryBuilder API は、ResourceResolver オブジェクトをリークします。 この漏れを軽減するには、次の手順に従います [コードサンプル](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).

## [!DNL Sling] API

* [**Apache [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) は、AEMを支える RESTful Web フレームワークです。 [!DNL Sling] は、HTTP リクエストルーティングを提供し、JCR ノードをリソースとしてモデル化し、セキュリティコンテキストなどを提供します。

[!DNL Sling] API には拡張機能向けにビルドされるというメリットが追加されています。つまり、を使用してビルドされたアプリケーションの動作を容易かつ安全に拡張できます。 [!DNL Sling] 拡張性の低い JCR API よりも API が多い。

### の一般的な使用例 [!DNL Sling] API

* JCR ノードへのアクセス [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) を使用し、 [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* を介したセキュリティコンテキストの提供 [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* ResourceResolver の [メソッドの作成/移動/コピー/削除](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* を使用したプロパティの更新 [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* 構築リクエスト処理構築ブロック

   * [サーブレット](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [サーブレットフィルター](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 非同期作業処理の構築ブロック

   * [イベントおよびジョブハンドラ](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [スケジューラ](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling Model](https://sling.apache.org/documentation/bundles/models.html)

* [サービスユーザー](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://www.adobe.io/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

この [JCR(Java Content Repository)2.0 API](https://www.adobe.io/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) は、JCR 実装の仕様の一部です (AEMの場合、 [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/)) をクリックします。 すべての JCR 実装は、これらの API に準拠し実装する必要があります。したがって、AEMコンテンツを操作するための最も低レベルの API です。

JCR 自体は、階層/ツリーベースの NoSQL データストアAEMで、がコンテンツリポジトリとして使用します。 JCR には、コンテンツ CRUD からコンテンツのクエリまで、様々なサポートされている API が多数あります。 この堅牢な API にもかかわらず、上位レベルのAEMや [!DNL Sling] 抽象概念

Apache Jackrabbit Oak API よりも JCR API を常に優先します。 JCR API は、 ***対話*** JCR リポジトリを使用し、Oak API は ***実装*** JCR リポジトリ。

### JCR API に関する一般的な誤解

JCR はAEMコンテンツリポジトリですが、API はコンテンツを操作する際の推奨される方法ではありません。 代わりに、AEM API（ページ、アセット、タグなど）を または Sling Resource API を使用すると、より優れた抽象概念が提供されます。

>[!CAUTION]
>
>AEMアプリケーションでの JCR API のセッションおよびノードインターフェイスの幅広い使用は、コードスメルです。 確認 [!DNL Sling] 代わりに API を使用しないでください。

### JCR API の一般的な使用例

* [アクセス制御管理](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [許可可能な管理（ユーザー/グループ）](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR の監視（JCR イベントのリッスン）
* ディープノード構造の作成

   * Sling API はリソースの作成をサポートしていますが、JCR API には便利なメソッドがあります。 [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) および [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) 深い構造の作成を迅速に行うことができます。

## OSGi API

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declarative Services 1.2 コンポーネント注釈 JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2 メタタイプ注釈 JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi フレームワーク JavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API と上位レベルの API(AEM、 [!DNL Sling]、 、JCR など )、および OSGi API を使用する必要はまれで、高レベルのAEM開発専門知識が必要です。

### OSGi と Apache Felix API の比較

OSGi は、すべての OSGi コンテナが実装し、準拠する必要がある仕様を定義します。 AEM OSGi 実装の Apache Felix は、独自の API もいくつか提供しています。

* OSGi API を優先 (`org.osgi`) を Apache Felix API 経由 (`org.apache.felix`) をクリックします。

### OSGi API の一般的な使用例

* OSGi サービスおよびコンポーネントの宣言用の OSGi 注釈。

   * 優先 [OSGi Declarative Services(DS)1.2 注釈](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) over [Felix SCR 注釈](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) OSGi サービスおよびコンポーネントの宣言用

* 動的なインコード用の OSGi API [OSGi サービス/コンポーネントの登録/解除](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * 条件付き OSGi Service/Component 管理が不要な場合（多くの場合）は、OSGi DS 1.2 注釈の使用をお勧めします。

## ルールの例外

次に、上記の規則に対する一般的な例外を示します。

### OSGi API

OSGi コンポーネントプロパティでの定義や読み取りなど、低レベルの OSGi 抽象概念を扱う場合、次の方法で提供される新しい抽象概念 `org.osgi` は、上位レベルの Sling スクラクションよりも優先されます。 競合する Sling の抽象概念は、 `@Deprecated` そして、 `org.osgi` 代替案。

また、OSGi 設定ノードの定義の方が優先される点にも注意してください。 `cfg.json` を `sling:OsgiConfig` 形式

### AEM Asset API

* 優先 [ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) over [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * また、 `com.day.cq` Assets API には、AEM Asset Management のユースケース向けのより包括的なツールが用意されています。
   * Granite Assets API は、低レベルのアセット管理ユースケース（バージョン、関係）をサポートしています。

### クエリ API

* AEM QueryBuilder は、次のような特定のクエリ関数をサポートしていません。 [候補](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions)、スペルチェックやインデックスヒントなど、一般的でない関数を示します。 これらの関数でクエリを実行する場合は、JCR-SQL2 をお勧めします。

### [!DNL Sling] サーブレットの登録 {#sling-servlet-registration}

* [!DNL Sling] サーブレットの登録、優先 [OSGi DS 1.2 注釈 (@SlingServletResourceTypesを含む )](https://sling.apache.org/documentation/the-sling-engine/servlets.html) over `@SlingServlet`

### [!DNL Sling] フィルター登録 {#sling-filter-registration}

* [!DNL Sling] フィルター登録、優先 [OSGi DS 1.2 注釈 (@SlingServletFilterを含む )](https://sling.apache.org/documentation/the-sling-engine/filters.html) over `@SlingFilter`

## 便利なコードスニペット

以下は、API を使用した一般的な使用例のベストプラクティスを説明する Java コードスニペットです。 また、これらのスニペットでは、優先度の低い API からより優先度の高い API に移行する方法を説明しています。

### JCR セッションの実行先 [!DNL Sling] ResourceResolver

#### Sling ResourceResolver を自動終了する

AEM 6.2 以降、 [!DNL Sling] ResourceResolver は `AutoClosable` 内 [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) 文。 この構文を使用すると、 `resourceResolver .close()` は不要です。

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

#### 手動で閉じた Sling ResourceResolver

ResourceResolvers は、 `finally` ブロック内に含める必要があります。

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

### JCR パス： [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### JCR ノードを [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] アセットをAEM

#### 推奨されるアプローチ

`DamUtil.resolveToAsset(..)`下のリソースを解決します。 `dam:Asset` 必要に応じてツリーを上に移動して、Asset オブジェクトに移動します。

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 別のアプローチ

リソースをアセットに適応させるには、リソース自体が `dam:Asset` ノード。

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] リソースからAEMページ

#### 推奨されるアプローチ

`pageManager.getContainingPage(..)` 下のリソースを解決します。 `cq:Page` 必要に応じてツリーの上に移動し、ページオブジェクトに移動します。

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 別のアプローチ {#alternative-approach-1}

リソースをページに適応させるには、リソース自体が `cq:Page` ノード。

```java
Page page = resource.adaptTo(Page.class);
```

### 読み取りAEM Page プロパティ

Page オブジェクトの getter を使用して、よく知られているプロパティ (`getTitle()`, `getDescription()`など ) および `page.getProperties()` を取得する `[cq:Page]/jcr:content` 他のプロパティを取得するための ValueMap。

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### AEM Asset メタデータプロパティの読み取り

Asset API には、 `[dam:Asset]/jcr:content/metadata` ノード。 これは ValueMap ではなく、2 番目のパラメータ（デフォルト値、自動タイプキャスト）はサポートされていないことに注意してください。

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### 読み取り [!DNL Sling] [!DNL Resource] プロパティ {#read-sling-resource-properties}

AEM API（ページ、アセット）が直接アクセスできない場所（プロパティまたは相対リソース）にプロパティが保存されている場合、 [!DNL Sling] リソースと値マップを使用して、データを取得できます。

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

この場合、AEMオブジェクトを [!DNL Sling] [!DNL Resource] 目的のプロパティまたはサブリソースを効率的に見つけ出す。

#### AEMページ [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### アセットのAEM先 [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### を使用してプロパティを書き込む [!DNL Sling]&#39;s ModifiableValueMap

用途 [!DNL Sling]&#39;s [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) ノードにプロパティを書き込みます。 これは、即時ノードにのみ書き込むことができます（相対プロパティパスはサポートされていません）。

への呼び出しに注意してください。 `.adaptTo(ModifiableValueMap.class)` にはリソースへの書き込み権限が必要です。権限がない場合は null が返されます。

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### AEMページの作成

AEMでページを適切に定義および初期化するには、ページテンプレートを取るページを作成する場合は、必ず PageManager を使用してください。

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### の作成 [!DNL Sling] リソース

ResourceResolver は、リソースを作成するための基本的な操作をサポートします。 より高いレベルの抽象概念 (AEMページ、アセット、タグなど ) を作成する場合、 それぞれの管理者が提供する方法を使用します。

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### の削除 [!DNL Sling] リソース

ResourceResolver は、リソースの削除をサポートします。 上位レベルの抽象概念 (AEMページ、アセット、タグなど ) を作成する場合、 それぞれの管理者が提供する方法を使用します。

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
