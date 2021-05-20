---
title: AEMのJava APIのベストプラクティスについて
description: AEMは、開発時に使用する多くのJava APIを公開する、豊富なオープンソースソフトウェアスタックに基づいて構築されています。 この記事では、主なAPIと、その使用のタイミングと理由について説明します。
version: 6.2, 6.3, 6.4, 6.5
sub-product: 基盤，アセット，サイト
feature: API
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2025'
ht-degree: 8%

---


# Java APIのベストプラクティスについて

Adobe Experience Manager（AEM）は、開発時に使用可能な多数の Java API を公開する豊富なオープンソースソフトウェアスタックに基づいて構築されています。この記事では、主なAPIと、その使用のタイミングと理由について説明します。

AEMは、4つのプライマリJava APIセットを基に構築されています。

* **Adobe Experience Manager (AEM)**

   * ページ、アセット、ワークフローなどの製品の抽象概念

* **[!DNL Apache Sling]Webフレームワーク**

   * リソース、値マップ、HTTP要求など、RESTおよびリソースベースの抽象概念。

* **JCR ([!DNL Apache Jackrabbit Oak])**

   * ノード、プロパティ、セッションなどのデータとコンテンツの抽象化。

* **[!DNL OSGi (Apache Felix)]**

   * サービスや(OSGi)コンポーネントなどのOSGiアプリケーションコンテナの抽象概念。

## Java APIプリファレンス「経験則」

一般的なルールは、次の順序でAPI/抽象を優先することです。

1. **AEM**
1. **[!DNL Sling]**
1. **JCR**
1. **OSGi**

AEMがAPIを提供する場合は、[!DNL Sling]、JCR、OSGiよりもAPIを優先します。 AEMがAPIを提供しない場合は、JCRおよびOSGiよりも[!DNL Sling]を推奨します。

この順序は一般的なルールで、例外が存在します。 このルールを解除する理由として許容できるものは次のとおりです。

* 以下に説明するよく知られた例外。
* 必要な機能は、上位レベルのAPIでは使用できません。
* それ自体があまり優先されないAPIを使用している既存のコード(カスタムまたはAEM製品コード)のコンテキストで動作し、新しいAPIに移行するコストは不当です。

   * ミックスを作成するよりも、低レベルのAPIを一貫して使用する方が効果的です。

## AEM API

* [**AEM API JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM APIは、製品化された使用例に固有の抽象概念と機能を提供します。

例えば、AEM [PageManager](https://helpx.adobe.com/jp/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html)および[Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html) APIは、Webページを表すAEMの`cq:Page`ノードの抽象概念を提供します。

これらのノードはリソースとして[!DNL Sling] APIを介して、ノードとしてJCR APIを介して使用できますが、AEM APIは一般的な使用例の抽象概念を提供します。 AEM APIを使用すると、AEM製品と、AEMのカスタマイズおよび拡張との間で一貫した動作が保たれます。

### com.adobe.*とcom.dayの比較* API

AEM APIは、パッケージ内の環境設定を持ち、次のJavaパッケージで識別されます。

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` は製品のユースケースをサポ `com.adobe.granite` ートするのに対して、ワークフローやタスク（製品間で使用される）などのクロス製品プラットフォームのユースケースをサポートします。AEM Assets、サイトなど)。

`com.day.cq` には、「オリジナル」のAPIが含まれています。これらのAPIは、Adobeが[!DNL Day CQ]を買収する前やその前後に存在した、主な抽象概念と機能に対応しています。 これらのAPIはサポートされており、`com.adobe.cq`または`com.adobe.granite`が（新しい）代替手段を提供しない限り、避けないでください。

[!DNL Content Fragments]や[!DNL Experience Fragments]などの新しい抽象概念は、以下に説明する`com.day.cq`ではなく、`com.adobe.cq`空間に構築されます。

### クエリAPI

AEMは複数のクエリ言語をサポートしています。 3つの主な言語は、[JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html)、XPathおよび[AEM Query Builder](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/querybuilder-api.html)です。

最も重要な懸念事項は、コードベース全体で一貫性のあるクエリ言語を維持し、理解するための複雑さとコストを削減することです。

すべてのクエリ言語は、[!DNL Apache Oak]が最終クエリ実行用にJCR-SQL2にトランスパイルするのと同じパフォーマンスプロファイルを実質的に持ちます。また、JCR-SQL2への変換時間は、クエリ時間自体に比べて非常に短いです。

推奨されるAPIは[AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html)です。これは最も高いレベルの抽象化で、クエリの結果を作成、実行、取得するための堅牢なAPIを提供し、次の機能を提供します。

* 単純なパラメーター化されたクエリの構成（Mapとしてモデル化されたクエリパラメーター）
* ネイティブの[Java APIおよびHTTP API](https://helpx.adobe.com/jp/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [OOTBクエリデバッガ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [一般的なク](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) エリ要件をサポートするOOTB述語

* 拡張可能なAPI。カスタムの[クエリ述語](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)の開発を可能にします。
* JCR-SQL2およびXPathは、 [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-)および[JCR API](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html)を介して直接実行でき、 [[!DNL Sling] Resources](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)または[JCR Nodes](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html)をそれぞれ返します。

>[!CAUTION]
>
>AEM QueryBuilder APIは、ResourceResolverオブジェクトをリークします。 このリークを軽減するには、次の[コードサンプル](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164)に従います。


## [!DNL Sling] API

* [**Apache  [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[ [!DNL Sling]](https://sling.apache.org/) Apacheisは、AEMを支えるRESTful Webフレームワークです。[!DNL Sling] は、HTTPリクエストルーティングを提供し、JCRノードをリソースとしてモデル化し、セキュリティコンテキストなどを提供します。

[!DNL Sling] APIには拡張機能向けに構築されるという利点が追加されています。つまり、拡張性の低いJCR APIよりも、APIを使用して構築されたアプリケーションの動作を拡張する方が簡単で安全で [!DNL Sling] す。

### [!DNL Sling] APIの一般的な使用法

* [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)としてJCRノードにアクセスし、[ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html)を介してデータにアクセスします。

* [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)を介したセキュリティコンテキストの提供。
* ResourceResolverの[create/move/copy/deleteメソッド](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)を使用したリソースの作成と削除。
* [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)を使用してプロパティを更新します。
* 構築リクエスト処理構築ブロック

   * [サーブレット](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [サーブレットフィルター](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 非同期作業処理の構築ブロック

   * [イベントハンドラーとジョブハンドラー](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [スケジューラ](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling Model](https://sling.apache.org/documentation/bundles/models.html)

* [サービスユーザー](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

[JCR(Java Content Repository)2.0 API](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)は、JCR実装の仕様の一部です(AEMの場合は[Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/))。 すべてのJCR実装は、これらのAPIに準拠し実装する必要があるので、AEMコンテンツを操作するための最も低いレベルのAPIです。

JCR自体は、階層/ツリーベースのNoSQLデータストアAEMをコンテンツリポジトリとして使用します。 JCRには、コンテンツCRUDからコンテンツのクエリに至るまで、サポートされているAPIの広範な配列があります。 この堅牢なAPIにもかかわらず、上位レベルのAEMや[!DNL Sling]抽象よりも優先されることはまれです。

Apache Jackrabbit Oak APIよりもJCR APIを常に優先します。 JCR APIはJCRリポジトリと&#x200B;***やり取り***&#x200B;用ですが、Oak APIはJCRリポジトリを&#x200B;***実装***&#x200B;用です。

### JCR APIに関する一般的な誤解

JCRはAEMコンテンツリポジトリですが、APIはコンテンツを操作するための推奨される方法ではありません。 代わりに、AEM API（ページ、アセット、タグなど）を またはSling Resource APIを使用すると、より優れた抽象概念が提供されます。

>[!CAUTION]
>
>JCR APIのセッションインターフェイスとノードインターフェイスをAEMアプリケーションで幅広く使用することは、コード臭です。 代わりに[!DNL Sling] APIを使用しないようにしてください。

### JCR APIの一般的な使用方法

* [アクセス制御管理](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [認証可能な管理（ユーザー/グループ）](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCRの監視（JCRイベントのリッスン）
* ディープノード構造の作成

   * Sling APIはリソースの作成をサポートしていますが、JCR APIには、[JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html)と[JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html)の便利なメソッドがあり、深い構造の作成を促進します。

## OSGi API

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declarative Services 1.2コンポーネント注釈JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2メタタイプ注釈JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGiフレームワークJavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi APIと上位レベルのAPI(AEM、[!DNL Sling]、JCR)の間に重複はほとんどなく、OSGi APIの使用はまれで、高レベルのAEM開発の専門知識が必要です。

### OSGiとApache Felix APIの比較

OSGiは、すべてのOSGiコンテナが実装および準拠する必要がある仕様を定義します。 AEM OSGi実装のApache Felixは、独自のAPIもいくつか提供します。

* Apache Felix API(`org.apache.felix`)よりもOSGi API(`org.osgi`)を優先します。

### OSGi APIの一般的な使用例

* OSGiサービスおよびコンポーネントを宣言するためのOSGi注釈。

   * OSGiサービスおよびコンポーネントを宣言するには、[OSGi Declarative Services(DS)1.2 Annotation](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)を[Felix SCR注釈](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html)よりも優先します。

* 動的なコード内の[のOSGi APIは、OSGiサービス/コンポーネント](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)の登録を解除します。

   * 条件付きOSGiサービス/コンポーネント管理が不要な場合（ほとんどの場合）は、OSGi DS 1.2注釈の使用をお勧めします。

## ルールの例外

次に、上記の規則に対する一般的な例外を示します。

### AEM Asset API

* [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html)よりも[ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html)を好みます。

   * `com.day.cq` Assets APIにはAEM Asset Managementの使用例に対するより補足的なツールが用意されています。
   * Granite Assets APIは、低レベルのアセット管理の使用例（バージョン、関係）をサポートしています。

### クエリAPI

* AEM QueryBuilderは、[提案](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions)、スペルチェック、インデックスヒントなど、一般的でない関数をサポートしていません。 これらの関数でクエリを実行する場合は、JCR-SQL2をお勧めします。

### [!DNL Sling] サーブレットの登録  {#sling-servlet-registration}

* [!DNL Sling] サーブレット登録、@ [SlingServletResourceTypesoverを使用したOSGi DS 1.2注釈の](https://sling.apache.org/documentation/the-sling-engine/servlets.html) 優先  `@SlingServlet`

### [!DNL Sling] フィルターの登録  {#sling-filter-registration}

* [!DNL Sling] 登録のフィルター、@ [SlingServletFilteroverを含むOSGi DS 1.2注釈を](https://sling.apache.org/documentation/the-sling-engine/filters.html) 優先  `@SlingFilter`

## 便利なコードスニペット

以下は、説明したAPIを使用する一般的な使用例のベストプラクティスを示すJavaコードスニペットです。 これらのスニペットでは、優先度の低いAPIからより優先度の高いAPIに移行する方法も示しています。

### [!DNL Sling] ResourceResolverに対するJCRセッション

#### Sling ResourceResolverを自動終了

AEM 6.2以降、 [!DNL Sling] ResourceResolverは[try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)ステートメント内では`AutoClosable`です。 この構文を使用する場合、`resourceResolver .close()`への明示的な呼び出しは不要です。

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

上記の自動終了手法を使用できない場合は、ResourceResolversを`finally`ブロックで手動で閉じる必要があります。

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

### [!DNL Sling] [!DNL Resource]へのJCRパス

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### [!DNL Sling] [!DNL Resource]へのJCRノード

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] アセットをAEM

#### 推奨されるアプローチ

`DamUtil.resolveToAsset(..)`必要に応じてツリーを上 `dam:Asset` に移動し、下のリソースをAssetオブジェクトに解決します。

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 別のアプローチ

リソースをアセットに適応させるには、リソース自体が`dam:Asset`ノードである必要があります。

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Resource to AEM Page

#### 推奨されるアプローチ

`pageManager.getContainingPage(..)` 必要に応じてツリーを上 `cq:Page` に移動し、下のリソースをページオブジェクトに解決します。

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 別のアプローチ{#alternative-approach-1}

リソースをページに適応させるには、リソース自体が`cq:Page`ノードである必要があります。

```java
Page page = resource.adaptTo(Page.class);
```

### AEM Pageプロパティの読み取り

Pageオブジェクトのgetterを使用して、既知のプロパティ（`getTitle()`、`getDescription()`など）を と`page.getProperties()`を使用して、他のプロパティを取得するための`[cq:Page]/jcr:content` ValueMapを取得します。

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### AEM Assetメタデータプロパティの読み取り

Asset APIは、`[dam:Asset]/jcr:content/metadata`ノードからプロパティを読み取る便利なメソッドを提供します。 これはValueMapではなく、2番目のパラメーター（デフォルト値、自動タイプキャスト）はサポートされていないことに注意してください。

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### [!DNL Sling] [!DNL Resource]プロパティ{#read-sling-resource-properties}を読み取ります。

AEM API（ページ、アセット）が直接アクセスできない場所（プロパティまたは相対リソース）にプロパティが保存されている場合、 [!DNL Sling]リソースとValueMapsを使用してデータを取得できます。

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

この場合、AEMオブジェクトを[!DNL Sling] [!DNL Resource]に変換して、目的のプロパティまたはサブリソースを効率的に見つけ出す必要が生じる場合があります。

#### AEMページを[!DNL Sling] [!DNL Resource]に移動

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM Asset to [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### [!DNL Sling]のModifiableValueMapを使用してプロパティを書き込む

[!DNL Sling]の[ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)を使用して、ノードにプロパティを書き込みます。 これは即時ノードにのみ書き込むことができます（相対プロパティパスはサポートされていません）。

`.adaptTo(ModifiableValueMap.class)`への呼び出しには、リソースへの書き込み権限が必要です。権限がない場合はnullが返されます。

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### AEMページの作成

AEMでページを適切に定義および初期化するには、ページテンプレートを取るので、必ずPageManagerを使用してページを作成してください。

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### [!DNL Sling]リソースの作成

ResourceResolverは、リソースを作成するための基本的な操作をサポートします。 より高いレベルの抽象概念(AEMページ、アセット、タグなど)を 各マネージャーが提供する方法を使用します。

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### [!DNL Sling]リソースの削除

ResourceResolverは、リソースの削除をサポートします。 上位レベルの抽象概念(AEMページ、アセット、タグなど)を作成する場合、 各マネージャーが提供する方法を使用します。

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
