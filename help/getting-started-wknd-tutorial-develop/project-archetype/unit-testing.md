---
title: 単体テスト
description: カスタムコンポーネントのチュートリアルで作成した署名コンポーネントの Sling モデルの動作を検証する単体テストを実装します。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: APIs, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4089
mini-toc-levels: 1
thumbnail: 30207.jpg
doc-type: Tutorial
exl-id: b926c35e-64ad-4507-8b39-4eb97a67edda
recommendations: noDisplay, noCatalog
duration: 706
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '2923'
ht-degree: 100%

---

# 単体テスト {#unit-testing}

このチュートリアルでは、[カスタムコンポーネント](./custom-component.md)のチュートリアルで作成した署名コンポーネントの Sling モデルの動作を検証する単体テストの実装について説明します。

## 前提条件 {#prerequisites}

[ローカル開発環境](overview.md#local-dev-environment)を設定するために必要なツールや説明を確認します。

_Java™ 8 と Java™ 11 の両方がシステムにインストールされている場合、VS Code テストランナーはテストの実行時に低い Java™ランタイムを選択し、テストが失敗する可能性があります。 これが発生した場合は、Java™ 8 をアンインストールします。_

### スタータープロジェクト

>[!NOTE]
>
> 前の章を正常に完了した場合は、プロジェクトを再利用して、スタータープロジェクトをチェックアウトする手順をスキップできます。

チュートリアルの作成元となるベースラインコードをチェックアウトします。

1. [GitHub](https://github.com/adobe/aem-guides-wknd)の `tutorial/unit-testing-start` ブランチをチェックアウトします。

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Maven スキルを使用して、ローカル AEM インスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 または 6.4 を使用している場合は、任意の Maven コマンドに `classic` プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

いつでも、完成したコードを [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) で確認したり、ブランチ `tutorial/unit-testing-start` に切り替えてローカルにチェックアウトしたりできます。

## 目的

1. 単体テストの基本を理解します。
1. AEM コードのテストに一般的に使用されるフレームワークとツールについて学びます。
1. 単体テストを記述する際に AEM リソースのモック作成やシミュレーションを行うオプションについて理解します。

## 背景 {#unit-testing-background}

このチュートリアルでは、署名コンポーネントの [Sling モデル](https://sling.apache.org/documentation/bundles/models.html)（[カスタム AEM コンポーネントの作成](custom-component.md)で作成したもの）の[単体テスト](https://en.wikipedia.org/wiki/Unit_testing)を記述する方法を見ていきます。単体テストとは、Java™ コードの想定される動作を検証するために Java™ で記述されるビルド時テストです。通常、各単体テストは小規模で、想定される結果に対してメソッド（または作業単位）の出力を検証します。

アドビでは、AEM のベストプラクティスを適用し、以下を使用します。

* [JUnit 5](https://junit.org/junit5/)
* [Mockito テストフレームワーク](https://site.mockito.org/)
* [wcm.io テストフレームワーク](https://wcm.io/testing/)（[Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html) を基に構築）

## 単体テストと Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=ja) では、AEM コードの単体テストのベストプラクティスを推奨および促進するために、単体テストの実施と[コードカバレッジレポート](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html?lang=ja)が CI／CD パイプラインに統合されています。

コードの単体テストはあらゆるコードベースで有益ですが、Cloud Manager を使用している場合は、Cloud Manager で実行できる単体テストを提供して、コード品質テストおよびレポートを活用することが重要です。

## テスト Maven 依存関係の更新 {#inspect-the-test-maven-dependencies}

まず最初に、テストの記述と実行をサポートする Maven 依存関係を調べます。必要な依存関係は次の 4 つです。

1. JUnit5
1. Mockito テストフレームワーク
1. Apache Sling Mocks
1. AEM Mocks テストフレームワーク（io.wcm から提供）

**JUnit5**、**Mockito および **AEM Mocks** のテスト用依存関係は、[AEM Maven アーキタイプ](project-setup.md)を使用してセットアップする際に、プロジェクトに自動的に追加されます。

1. これらの依存関係を表示するには、親リアクター POM（**aem-guides-wknd/pom.xml**）を開いて `<dependencies>..</dependencies>` に移動すると、`<!-- Testing -->` の下で JUnit、Mockito、Apache Sling Mocks、AEM Mock Tests（io.wcm から提供）の依存関係を確認できます。
1. `io.wcm.testing.aem-mock.junit5` が **4.1.0** に設定されていることを確認します。

   ```xml
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <version>4.1.0</version>
       <scope>test</scope>
   </dependency>
   ```

   >[!CAUTION]
   >
   > アーキタイプ **35** では、`io.wcm.testing.aem-mock.junit5`バージョン **4.1.8** のプロジェクトが生成されます。**4.1.0** にダウングレードして、この章の残りの部分に従ってください。

1. **aem-guides-wknd/core/pom.xml** を開き、対応するテストの依存関係が使用可能であることを確認します。

   **コア**&#x200B;プロジェクトの並列ソースフォルダーには、単体テストと、サポートするテストファイルが含まれます。この **test** フォルダーにより、テストクラスとソースコードを分離できますが、テストを、ソースコードと同じパッケージに存在するかのように動作させることができます。

## JUnit テストの作成 {#creating-the-junit-test}

単体テストは一般的に、Java™ クラスとの 1 対 1 のマッピングを行います。この章では、署名コンポーネントを支える Sling モデルである **BylineImpl.java** 用に JUnit テストを記述します。

![単体テスト用の src フォルダー](assets/unit-testing/core-src-test-folder.png)

*単体テストの保存場所。*

1. テストする Java™クラスの場所を反映する Java™パッケージフォルダー構造内の `src/test/java` で新しい Java™クラスを作り、`BylineImpl.java` 向けの単体テストを作成します。

   ![新しいBylineImplTest.java ファイルの作成](assets/unit-testing/new-bylineimpltest.png)

   テスト中のため

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   対応する単体テスト Java™クラスを次の場所に作成します：

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   この単体テストファイル `BylineImplTest.java` のサフィックス `Test` は命名規則によるもので、以下が可能になります

   1. テストファイルとして容易に識別できます _対象_ `BylineImpl.java`
   1. ただし、テストファイルを区別します _から_ 試験を受けているクラス `BylineImpl.java`

## BylineImplTest.java のレビュー {#reviewing-bylineimpltest-java}

この時点で、JUnit テストファイルは空の Java™ クラスです。

1. ファイルを次のコードで更新します。

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import static org.junit.jupiter.api.Assertions.*;
   
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   
   public class BylineImplTest {
   
       @BeforeEach
       void setUp() throws Exception {
   
       }
   
       @Test 
       void testGetName() { 
           fail("Not yet implemented");
       }
   
       @Test 
       void testGetOccupations() { 
           fail("Not yet implemented");
       }
   
       @Test 
       void testIsEmpty() { 
           fail("Not yet implemented");
       }
   }
   ```

1. 最初のメソッド `public void setUp() { .. }` には、JUnit の `@BeforeEach` で注釈がつけられます。これは、このクラスの各テストメソッドを実行する前に、JUnit テストランナーにこのメソッドを実行するように指示するものです。 これは、すべてのテストで必要となる一般的なテスト状態を初期化するのに便利な場所です。

1. 以降のメソッドは、命名規則によってプレフィックス `test` が付けられ、 `@Test` の注釈でマークされた名前のテストメソッドです。 まだ実装されていないため、デフォルトですべてのテストが失敗するように設定されています。

   まず、テストするクラスのパブリックメソッドごとに 1 つのテストメソッドから始めます。そのため、次のようにします。

   | BylineImpl.java |              | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | 右記でテスト | testGetName() |
   | getOccupations() | 右記でテスト | testGetOccupations() |
   | isEmpty() | 右記でテスト | testIsEmpty() |

   これらのメソッドは必要に応じて拡張できます。詳しくはこの章で後述します。

   この JUnit テストクラス（別称：JUniit テストケース）が実行されている場合、`@Test` とマークされた各メソッドは、成功または失敗する可能性があるテストとして実行されます。

![生成された BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. JUnit テストケースを実行するには、 `BylineImplTest.java` ファイルを右クリックして「**実行**」をタップ します。
まだ実装されていないので、予想どおりすべてのテストが失敗します。

   ![junit テストとして実行](assets/unit-testing/run-junit-tests.png)

   *BylineImplTests.java／実行を右クリックします。*

## BylineImpl.java のレビュー {#reviewing-bylineimpl-java}

単体テストを作成する際の主なアプローチは次の 2 つです。

* [TDD またはテスト主導型開発](https://ja.wikipedia.org/wiki/Test-driven_development)：実装が開発される直前に、単体テストを増分的に記述します。テストを記述し、そのテストが合格するように実装を記述します。
* 実装優先開発：まず作業用コードを開発したあと、そのコードを検証するテストを記述します。

このチュートリアルでは、後者のアプローチを使用します（動作する **BylineImpl.java** を前の章で既に作成されています）。このため、パブリックメソッドの動作だけでなく、その実装の詳細の一部についても確認し理解しておく必要があります。優れたテストは入力と出力のみを重視する必要があるので、理屈に合わないと思われるかもしれません。AEM で作業する際には、動作するテストを構築するために、実装に関する様々な考慮事項を理解しておく必要があります。

AEM における TDD には高度な専門知識が必要です。AEM 開発や AEM コードの単体テストを熟知した AEM 開発者が使用することで最大限の効果を発揮できます。

## AEM テストコンテキストの設定  {#setting-up-aem-test-context}

AEM で記述されるコードの大部分は JCR、Sling または AEM API に依存しているので、正常に実行するためには実行中の AEM のコンテキストが必要となります。

単体テストは実行中の AEM インスタンスのコンテキスト外にあるビルドで実施されるので、そのようなコンテキストがありません。これを促すために、[wcm.io の AEMContext](https://wcm.io/testing/aem-mock/usage.html) は、これらの API が、ほぼ AEM 内で実行しているかのように動作できるモックコンテキストを作成します。__

1. **BylineImplTest.java** で **wcm.io の** `AemContext`を使用して、`@ExtendWith` でデコレートされた JUnit 拡張機能として **BylineImplTest.java** ファイルに追加することで、AEM コンテキストを作成します。 拡張機能では、必要な初期化タスクとクリーンアップのタスクがすべて処理されます。 すべてのテストメソッドで使用できる `AemContext` のクラス変数を作成します。

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   この変数 `ctx` は、一部の AEM および Sling の抽象化するモック AEM コンテキストを公開します。

   * BylineImpl Sling モデルはこのコンテキストに登録されます。
   * モック JCR コンテンツ構造は、このコンテキストで作成されます。
   * カスタム OSGi サービスは、このコンテキストで登録できます。
   * 一般的に必要となる様々なモックオブジェクトおよびヘルパー（SlingHttpServletRequest オブジェクトなど）、様々なモック Sling および AEM OSGi サービス（ModelFactory、PageManager、ページ、テンプレート、ComponentManager、コンポーネント、TagManager、タグなど）を提供します。
      * *これらのオブジェクトのすべてのメソッドが実装されるわけではありません。*
   * [その他](https://wcm.io/testing/aem-mock/usage.html)

   **`ctx`** オブジェクトは、ほとんどのモックコンテキストのエントリポイントとして機能します。

1. 各 `@Test` メソッドの前に実行される `setUp(..)` メソッドで、モックテストの一般的な状態を定義します。

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** は、テスト対象の Sling モデルをモック AEM コンテキストに登録して、`@Test` メソッドでインスタンス化できるようにします。
   * **`load().json`** は、リソース構造をモックコンテキストに読み込み、コードがこれらのリソースを、実際のリポジトリで提供されたかのように操作できるようになります。ファイル **`BylineImplTest.json`** のリソース定義は、**/content** 下のモック JCR コンテキストに読み込まれます。
   * **`BylineImplTest.json`** がまだ存在しないので、作成して、テストに必要な JCR リソース構造を定義します。

1. モックリソース構造を表す JSON ファイルは、JUnit Java™ テストファイルと同じパッケージパスに従って、**core/src/test/resources** に保存されます。

   次の内容を含んだ **BylineImplTest.json** という名前の JSON ファイルを `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` に作成します。

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   この JSON は、署名コンポーネントの単体テストのモックリソース（JCR ノード）を定義します。この時点で、JSON には署名コンポーネントのコンテンツリソースを表すのに必要なプロパティの最小セットである `jcr:primaryType` と `sling:resourceType` が含まれています。

   単体テストで作業する際の一般的なルールは、各テストに必要なモックコンテキスト、コンテンツおよびコードの最小限のセットを作成することです。テストを記述する前に完全なモックコンテキストを作成したくなりますが、結果的に不要なアーティファクトが生成されることが多いので、避けてください。

   これで **BylineImplTest.json** が存在するので、`ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` が実行されると、モックリソース定義がパス **/content.** のコンテキストに読み込まれます。

## getName() のテスト {#testing-get-name}

基本的なモックコンテキストの設定が完了したので、**BylineImpl の getName()** の最初のテストを作成します。このテストでは、リソースの「**name**」プロパティに保存されている作成済みの正しい名前をメソッド **getName()** が返すことを確認する必要があります。

1. **BylineImplTest.java** の **testGetName**() メソッドを次のように更新します。

   ```java
   import com.adobe.aem.guides.wknd.core.models.Byline;
   ...
   @Test
   public void testGetName() {
       final String expected = "Jane Doe";
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       String actual = byline.getName();
   
       assertEquals(expected, actual);
   }
   ```

   * **`String expected`** は、想定される値を設定します。ここでは、これを「**Jane Done**」に設定します。
   * **`ctx.currentResource`** は、コードを評価する際に対照とするモックリソースのコンテキストを設定します。そのため、これはモック署名コンテンツリソースの読み込み先となる **/content/byline** に設定されます。
   * **`Byline byline`** は、モックリクエストオブジェクトから適応させて、署名 Sling モデルをインスタンス化します。
   * **`String actual`** は、署名 Sling モデルオブジェクト上で、テスト対象であるメソッド `getName()` を呼び出します。
   * **`assertEquals`** は、想定される値が、署名 Sling モデルオブジェクトから返される値と一致するとアサートします。これらの値が等しくない場合、テストは失敗します。

1. テストを実行しますが、`NullPointerException` で失敗します。

   モック JSON で `name` プロパティを定義していないからといって、このテストが失敗するわけではありません。モック JSON はテスト失敗の原因になりますが、このテストはまだそこまで実行されていません。このテストは、署名オブジェクト自体での `NullPointerException` が原因で失敗します。

1. `BylineImpl.java` では、`@PostConstruct init()` が例外をスローすると、Sling モデルをインスタンス化できなくなるので、その Sling モデルオブジェクトが null になります。

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   ModelFactory OSGi サービスは `AemContext` 経由（Apache Sling Context 経由）で提供されますが、BylineImpl の `init()` メソッドで呼び出される `getModelFromWrappedRequest(...)` を含め、すべてのメソッドが実装されているわけではないということがわかりました。この結果、[AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html) が返されることになり、それが原因で `init()` が失敗し、`ctx.request().adaptTo(Byline.class)` の適応結果が null オブジェクトになります。

   提供されているモックではこの例のコードに対応できないので、自分でモックコンテキストを実装する必要があります。そのために、Mockito を使用して、`getModelFromWrappedRequest(...)` が呼び出されたときにモック Image オブジェクトを返すモック ModelFactory オブジェクトを作成します。

   署名 Sling モデルをインスタンス化するためでさえ、このモックコンテキストを適切に用意する必要があるので、`@Before setUp()` メソッドに追加します。また、**BylineImplTest** クラスの上の `@ExtendWith` 注釈に `MockitoExtension.class` を追加する必要もあります。

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import org.mockito.junit.jupiter.MockitoExtension;
   import org.mockito.Mock;
   
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   
   import org.apache.sling.models.factory.ModelFactory;
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   import org.junit.jupiter.api.extension.ExtendWith;
   
   import static org.junit.jupiter.api.Assertions.*;
   import static org.mockito.Mockito.*;
   import org.apache.sling.api.resource.Resource;
   
   @ExtendWith({ AemContextExtension.class, MockitoExtension.class })
   public class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   
       @Mock
       private Image image;
   
       @Mock
       private ModelFactory modelFactory;
   
       @BeforeEach
       public void setUp() throws Exception {
           ctx.addModelsForClasses(BylineImpl.class);
   
           ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   
           lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()), any(Resource.class), eq(Image.class)))
                   .thenReturn(image);
   
           ctx.registerService(ModelFactory.class, modelFactory, org.osgi.framework.Constants.SERVICE_RANKING,
                   Integer.MAX_VALUE);
       }
   
       @Test
       void testGetName() { ...
   }
   ```

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** は、テストケースクラスを [Mockito JUnit Jupiter 拡張機能](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html)で実行するようにマークし、@Mock 注釈を使用してモックオブジェクトをクラスレベルで定義できるようにします。
   * **`@Mock private Image`** はタイプ `com.adobe.cq.wcm.core.components.models.Image` のモックオブジェクトを作成します。これはクラスレベルで定義されているので、必要に応じて、`@Test` メソッドで動作を変更できます。
   * **`@Mock private ModelFactory`** は ModelFactory タイプのモックオブジェクトを作成します。これは純粋な Mockito モックであり、実装されているメソッドはありません。これはクラスレベルで定義されているので、必要に応じて、`@Test` メソッドで動作を変更できます。
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** は、モック ModelFactory オブジェクトで `getModelFromWrappedRequest(..)` が呼び出された場合のモック動作を登録します。`thenReturn (..)` に定義されている結果は、モック Image オブジェクトを返すことです。この動作が呼び出されるのは、最初のパラメータが `ctx` のリクエストオブジェクトと等しく、2 番目のパラメータが任意の Resource オブジェクトであり、3 番目のパラメータがコアコンポーネントの Image クラスである場合のみです。テスト全体を通して、`ctx.currentResource(...)` を **BylineImplTest.json** で定義されている様々なモックリソースに設定しているので、どのような Resource でも使用できます。なお、後で ModelFactory の該当する動作をオーバーライドする必要があるので、**lenient()** の厳密性を追加します。
   * **`ctx.registerService(..)`。** は、モック ModelFactory オブジェクトを最高のサービスランキングで AemContext に登録します。BylineImpl の `init()` で使用される ModelFactory は `@OSGiService ModelFactory model` フィールドを通じて挿入されるので、これが必要になります。AemContext で `getModelFromWrappedRequest(..)` への呼び出しを処理する&#x200B;**今回の**&#x200B;モックオブジェクトを挿入するには、そのタイプ（ModelFactory）の最もランキングの高いサービスとして登録する必要があります。

1. テストを再度実行すると再び失敗しますが、今回は失敗の理由は明白です。

   ![testGetName の失敗のアサーション](assets/unit-testing/testgetname-failure-assertion.png)

   *アサーションによる testGetName() の失敗*

   **AssertionError** が返されます。これはテストでのアサート条件が失敗し、**期待値は &quot;Jane Doe&quot;** で、**実際の値が null** であることを示します。**BylineImplTest.json** のモック&#x200B;**/content/byline** リソース定義に「**name**」プロパティが追加されていないので、この結果は当然です。そこで、これを追加します。

1. **BylineImplTest.json** を更新して `"name": "Jane Doe".` を定義します

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. テストを再実行すると、今回は **`testGetName()`** が成功します。

   ![テスト名の合格](assets/unit-testing/testgetname-pass.png)


## getOccupations() のテスト {#testing-get-occupations}

よくできました。最初のテストは成功しました。先へ進み、`getOccupations()` をテストします。モックコンテキストの初期化が `@Before setUp()` メソッドで行われたため、`getOccupations()` を含むこのテストケースのすべての `@Test` メソッドで利用できるようになります。

このメソッドは、職業プロパティに保存されている職業のリストをアルファベット順（降順）に並べ替えて返します。

1. **`testGetOccupations()`** を次のように更新します。

   ```java
   import java.util.List;
   import com.google.common.collect.ImmutableList;
   ...
   @Test
   public void testGetOccupations() {
       List<String> expected = new ImmutableList.Builder<String>()
                               .add("Blogger")
                               .add("Photographer")
                               .add("YouTuber")
                               .build();
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       List<String> actual = byline.getOccupations();
   
       assertEquals(expected, actual);
   }
   ```

   * **`List<String> expected`**&#x200B;は期待される結果を定義します。
   * **`ctx.currentResource`** は現在のリソースを設定し、/content/byline でモックリソース定義に対してコンテキストを評価します。これにより、モックリソースのコンテキストで **BylineImpl.java** が実行されるようにします。
   * **`ctx.request().adaptTo(Byline.class)`** は、Byline Sling Model をモックリクエストオブジェクトから適応させてインスタンス化します。
   * **`byline.getOccupations()`**&#x200B;は署名 Sling Model オブジェクトでテストしているメソッド `getOccupations()` を呼び出します。
   * **`assertEquals(expected, actual)`** は、期待されているリストが実際のリストと同じであるとアサートします。

1. 上記の **`getName()`**&#x200B;と同様に、**BylineImplTest.json** は職業を定義しません。`byline.getOccupations()` は空のリストを返すため、このテストを実行すると失敗します。

   **BylineImplTest.json** を更新して職業のリストを含めます。すると、**`getOccupations()`** によって職業がアルファベット順に並べ替えられていることをテストで検証できるように、このリストはアルファベット順以外に設定されます。

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe",
       "occupations": ["Photographer", "Blogger", "YouTuber"]
       }
   }
   ```

1. テストを実行すると、再び合格です。職業の並べ替えが正常に機能しているようです。

   ![職業パスを取得](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations() は成功*

## isEmpty() のテスト {#testing-is-empty}

最後のメソッドは **`isEmpty()`** のテストです。

`isEmpty()` のテストは、様々な条件でのテストが必要なので、興味深いものです。 **BylineImpl.java** の `isEmpty()` メソッドをレビューするには、次の条件をテストする必要があります。

* 名前が空の場合は、true を返します。
* 職業が null または空の場合は、true を返します。
* 画像が null の場合または画像に src URL がない場合は、true を返します。
* 名前、職業、画像（src URL を含む）が存在する場合は、false を返します。

これにより、`BylineImplTest.json` で特定の条件や新しいモックリソース構造をテストする新しいテストメソッドを作成して、これらのテストを実施する必要があります。

`getName()`、`getOccupations()` および `getImage()` が空の場合、そのステートの期待される動作が `isEmpty()` 経由でテストされるので、このチェックによってテストをスキップできます。

1. 最初のテストは、プロパティが設定されていない、まったく新しいコンポーネントの条件をテストします。

   `BylineImplTest.json` に新しいリソース定義を追加し、意味のある名前「**empty**」を付けます。

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   **`"empty": {...}`** は、`jcr:primaryType` と `sling:resourceType` のみを持つ「empty」という名前の新しいリソースを定義します。

   `@setUp` での各テストメソッドの実行前に、`BylineImplTest.json` を `ctx` に読み込みます。そのため、新しいリソース定義は、**/content/empty** のテストですぐに利用できるようになります。

1. `testIsEmpty()` を次のように更新し、現在のリソースを新しい「**empty**」というモックリソース定義に設定します。

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   テストを実行し、成功することを確認します。

1. 次に、必要なデータポイント（名前、職業、または画像）が空の場合、`isEmpty()` が true を返すメソッドのセットを作成します。

   各テストで、個別のモックリソース定義が使用され、**without-name** および **without-occupations** の追加リソース定義を使用して **BylineImplTest.json** を更新します。

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       },
       "without-name": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "occupations": "[Photographer, Blogger, YouTuber]"
       },
       "without-occupations": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe"
       }
   }
   ```

   次のテストメソッドを作成し、これらのステートをそれぞれテストします。

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutName() {
       ctx.currentResource("/content/without-name");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutOccupations() {
       ctx.currentResource("/content/without-occupations");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImage() {
       ctx.currentResource("/content/byline");
   
       lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()),
           any(Resource.class),
           eq(Image.class))).thenReturn(null);
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImageSrc() {
       ctx.currentResource("/content/byline");
   
       when(image.getSrc()).thenReturn("");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   **`testIsEmpty()`** は、空のモックリソース定義に対してテストし、`isEmpty()` が true であるとアサートします。

   **`testIsEmpty_WithoutName()`** は、職業を持つが名前がないモックリソース定義に対してテストします。

   **`testIsEmpty_WithoutOccupations()`** は、名前を持つが職業を持たないモックリソース定義に対してテストします。

   **`testIsEmpty_WithoutImage()`** は、名前と職業でモックリソース定義に対してテストし、モック画像が null を返すよう設定します。なお、`setUp()` で定義された動作 `modelFactory.getModelFromWrappedRequest(..)` をオーバーライドして、この呼び出しによって返される画像オブジェクトが null になるようにします。 Mockito のスタブ機能は厳密なため、コードの重複は避けます。 したがって、**`lenient`** 設定を使用してモックを設定し、`setUp()` メソッドでの動作をオーバーライドすることを明示的に示します。

   **`testIsEmpty_WithoutImageSrc()`** は、名前と職業でモックリソース定義をテストし、`getSrc()` を呼び出すと空白の文字列を返すようモック画像を設定します。

1. 最後に、コンポーネントが正しく設定されている場合、**isEmpty()** が false を返すようテストを記述します。この条件の場合は、完全に設定された署名コンポーネントを表す **/content/byline** を再利用できます。

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. 次に、BylineImplTest.java ファイルのすべての単体テストを実行し、Java™ テストレポートの出力を確認します。

![すべてのテストが合格](./assets/unit-testing/all-tests-pass.png)

## ビルドの一部として単体テストを実行する {#running-unit-tests-as-part-of-the-build}

単体テストは、Maven ビルドの一部として実行して成功する必要があります。これにより、アプリケーションをデプロイする前にすべてのテストが成功することを確認します。パッケージやインストールなどの Maven のゴールを実行すると、テストが自動的に呼び出され、プロジェクトのすべての単体テストで成功する必要があります。

```shell
$ mvn package
```

![mvn パッケージ成功](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

同様に、テストメソッドが失敗するような変更を加えると、ビルドは失敗し、どのテストが失敗したのか、その理由は何かが報告されます。

![mvn パッケージ失敗](assets/unit-testing/mvn-package-fail.png)

## コードのレビュー {#review-the-code}

完成したコードを [GitHub](https://github.com/adobe/aem-guides-wknd) で表示するか、Git ブランチ `tutorial/unit-testing-solution` でローカルにコードをデプロイしてレビューします。 
