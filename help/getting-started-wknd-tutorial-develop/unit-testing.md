---
title: 単体テスト
description: このチュートリアルでは、「カスタムコンポーネント」チュートリアルで作成したBylineコンポーネントのSlingモデルの動作を検証する単体テストの導入について説明します。
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '3544'
ht-degree: 39%

---


# 単体テスト {#unit-testing}

This tutorial covers the implementation of a Unit Test that validates the behavior of the Byline component&#39;s Sling Model, created in the [Custom Component](./custom-component.md) tutorial.

## 前提条件 {#prerequisites}

チュートリアルが構築する基本行コードを調べます。

1. github.com/adobe/aem-guides-wknd [リポジトリをコピーします](https://github.com/adobe/aem-guides-wknd) 。
1. ブランチをチェックアウトし `unit-testing/start` ます

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
$ cd ~/code/aem-guides-wknd
$ git checkout unit-testing/start
```

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd/tree/unit-testing/solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `unit-testing/solution`ます。

## 目的

1. ユニットテストの基本を理解します。
1. AEMコードのテストに一般的に使用されるフレームワークとツールについて説明します。
1. ユニットテストを記述する際にAEMリソースをモッキングまたはシミュレーションするオプションを理解します。

## 背景 {#unit-testing-background}

このチュートリアルでは、Bylineコンポーネントの [Slingモデル](https://ja.wikipedia.org/wiki/%E5%8D%98%E4%BD%93%E3%83%86%E3%82%B9%E3%83%88) (カスタムAEMコンポーネントの [作成で作成)の](https://sling.apache.org/documentation/bundles/models.html)[](custom-component.md)単体テストの作成方法を学びます。 単体テストは、Java コードの期待される動作を検証するために Java で記述されるビルド時間テストです。通常、各ユニットテストは小さく、メソッド（または作業単位）の出力を期待される結果と比較して検証します。

ここでは、AEM ベストプラクティスおよび以下を使用します。

* [JUnit 5](https://junit.org/junit5/)
* [Mockito テストフレームワーク](https://site.mockito.org/)
* [wcm.ioテストフレームワーク](https://wcm.io/testing/) ( [](https://sling.apache.org/documentation/development/sling-mock.html)Apache Sling Mock上に構築)

>[!VIDEO](https://video.tv.adobe.com/v/30207/?quality=12&learn=on)

## ユニットテストとAdobeクラウドマネージャー {#unit-testing-and-adobe-cloud-manager}

[AdobeCloud Manager](https://docs.adobe.com/content/help/ja-JP/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html) は、ユニットテストの実行と [](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) コードカバレッジのレポートをCI/CDパイプラインに統合して、ユニットテスト用のAEMコードのベストプラクティスを促し、推進します。

コードの単体テストはあらゆるコードベースで有益ですが、Cloud Manager を使用している場合は、Cloud Manager で実行できる単体テストを提供して、コード品質のテストや報告機能を活用することが重要です。

## Inspect the test Maven dependencies {#inspect-the-test-maven-dependencies}

最初の手順は、Mavenの依存関係を調べて、テストの記述と実行をサポートすることです。 次の4つの依存関係が必要です。

1. JUnit5
1. Mockito Test Framework
1. Apache Sling Mocks
1. AEM Mocks Test Framework (by io.wcm)

**JUnit5**、 **Mockito** 、およびAEM Mocks **テストの依存関係は、設定時に**[](project-setup.md)AEM Maven archetype editorを使用して自動的にプロジェクトに追加されます。

1. これらの依存関係を表示するには、 aem-guides-wknd/pom.xmlにある親リアクタのPOMを開き **、に移動して次の依存関係が定義さ**`<dependencies>..</dependencies>` れていることを確認します。

   ```xml
   <dependencies>
       ...
       <!-- Testing -->
       <dependency>
           <groupId>org.junit</groupId>
           <artifactId>junit-bom</artifactId>
           <version>5.5.2</version>
           <type>pom</type>
           <scope>import</scope>
       </dependency>
       <dependency>
           <groupId>org.slf4j</groupId>
           <artifactId>slf4j-simple</artifactId>
           <version>1.7.25</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-core</artifactId>
           <version>2.25.1</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-junit-jupiter</artifactId>
           <version>2.25.1</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>junit-addons</groupId>
           <artifactId>junit-addons</artifactId>
           <version>1.4</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>io.wcm</groupId>
           <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
           <!-- Prefer the latest version of AEM Mock Junit5 dependency -->
           <version>2.5.2</version>
           <scope>test</scope>
       </dependency>
       ...
   </dependencies>
   ```

1. aem-guides-wknd/core/pom.xml **を開き** 、対応するテスト依存関係が使用可能であることを確認します。

   ```xml
   ...
   <dependency>
       <groupId>org.junit.jupiter</groupId>
       <artifactId>junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-core</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>junit-addons</groupId>
       <artifactId>junit-addons</artifactId>
   </dependency>
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
   </dependency>
   ...
   ```

   A parallel source folder in the **core** project will contain the unit tests and any supporting test files. この **test** フォルダーはテストクラスをソースコードから分離しますが、ソースコードと同じパッケージ内にあるようにテストを動作させることができます。

## JUnit テストの作成 {#creating-the-junit-test}

単体テストは一般的に、Java クラスと 1 対 1 でマッピングします。この章では、署名コンポーネントを支える Sling Model である **BylineImpl.java** 用に JUnit テストを記述します。

![単体テストパッケージエクスプローラー](assets/unit-testing/core-src-test-folder.png)

*単体テストが保存される場所。*

1. We can do this in Eclipse, by right-clicking on the Java class to test, and selecting the **New > Other > Java > JUnit > JUnit Test Case**.

   ![BylineImpl.javaを右クリックして、単体テストを作成します。](assets/unit-testing/junit-test-case-1.png)

1. 最初のウィザード画面で以下を検証します。

   * The JUnit test type is **New JUnit Jupiter test** as these are the JUnit Maven dependencies set up in our **pom.xml&#39;s**.
   * The **package** is the java package of the class being tested (`BylineImpl.java`)
   * The Source folder points to the **core** project, (`aem-guides-wknd.core/src/test/java`) which instructs Eclipse where the unit test files are stored.
   * The `setUp()` method stub will be created manually; we&#39;ll see how this is used later.
   * And the class under test is `BylineImpl.java`, as this is the Java class we want to test.

   ![ユニットテストウィザードの手順2](assets/unit-testing/junit-wizard-testcase.png)

   *「JUnitテストケース」ウィザード — 手順2*

1. ウィザード下部にある「**次へ**」ボタンをクリックします。

   次のステップは、テストメソッドの自動生成に役立ちます。通常、Javaクラスの各パブリックメソッドは、少なくとも1つの対応するテストメソッドを持ち、その動作を検証します。 単体テストには 1 つのパブリックメソッドをテストする複数のテストメソッドがあり、それぞれが異なる入力または状態を表すということがよくあります。

   In the wizard, select all the methods under `BylineImpl`, with the exception of `init()` which is a method used by the Sling Model internally (via `@PostConstruct`). We will effectively test the `init()` by testing all other methods, as the other methods rely on `init()` executing successfully.

   新しいテストメソッドはいつでも JUnit テストクラスに追加できます。ウィザードのこのページは便宜上のものです。

   ![ユニットテストウィザードの手順3](assets/unit-testing/junit-test-case-3.png)

   *JUnit テストケースウィザード（続き）*

1. JUnit5 テストファイルを生成するには、ウィザードの下部にある「終了」ボタンをクリックします。
1. JUnit5テストファイルが、 **aem-guides-wknd.core** >/src/test/javaの対応するパッケージ構造に、という名前のファイルとして作成されていることを確認し ****`BylineImplTest.java`ます。

## BylineImplTest.java のレビュー {#reviewing-bylineimpltest-java}

テストファイルには多数の自動生成メソッドがあります。この時点で、この JUnit テストファイルには AEM 固有のものはありません。

1つ目の方法は、注釈 `public void setUp() { .. }` を付ける方法 `@BeforeEach`です。

The `@BeforeEach` annotation is a JUnit annotation that instructs the JUnit test running to execute this method before running each test method in this class.

The subsequent methods are the test methods themselves and are marked as such with the `@Test` annotation. デフォルトでは、すべてのテストが失敗するように設定されています。

When this JUnit test class (also known as a JUnit Test Case) is run, each method marked with the `@Test` will execute as a test which can either pass or fail.

![生成されたBylineImplTest](assets/unit-testing/bylineimpltest-new.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. クラス名を右クリックし、**実行ユーザー／JUnit テスト**&#x200B;を選択して、JUnit テストケースを実行します。

   ![junitテストとして実行](assets/unit-testing/run-as-junit-test.png)

   *BylineImplTests.java ／実行ユーザー／JUnit テストを右クリックします。*

1. 想定どおり、すべてのテストは失敗します。

   ![試験の失敗](assets/unit-testing/all-tests-fail.png)

   *Eclipse／Window／ビューを表示／Java／JUnit の JUnit ビュー*

## BylineImpl.java のレビュー {#reviewing-bylineimpl-java}

ユニット・テストを記述する場合、主に次の2つの方法があります。

* [TDD またはテスト駆動開発](https://ja.wikipedia.org/wiki/%E3%83%86%E3%82%B9%E3%83%88%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA)。実装を開発する直前に単体テストの増分を記述、テストを記述、実装を記述してテストを合格します。
* 最初に実装をおこなう開発。動作するコードを最初に開発してから、そのコードを検証するテストを記述します。

このチュートリアルでは、後者のアプローチを使用します（前の章で動作する **BylineImpl.java** を作成済みのため）。このため、パブリックメソッドの動作だけでなく、いくつかの実装の詳細についても確認および理解しておく必要があります。優れたテストは入力と出力のみを重視する必要があるので、理屈に合わないと思われるかもしれません。AEM で作業する際には、動作するテストを構築するために、実装に関する様々な考慮事項を理解しておく必要があります。

AEM における TDD には高度な専門知識が必要です。AEM 開発や AEM コードの単体テストを熟知した AEM 開発者が使用することで最大限の効果を発揮できます。

>[!VIDEO](https://video.tv.adobe.com/v/30208/?quality=12&learn=on)

## AEM テストコンテキストの設定  {#setting-up-aem-test-context}

AEM で記述されるコードの大部分は JCR、Sling または AEM API に依存しているので、正常に実行するためには実行中の AEM のコンテキストが必要となります。

単位テストはビルド時に実行されるので、実行中のAEMインスタンスのコンテキスト外では、そのようなリソースはありません。 To facilitate this, [wcm.io&#39;s AEM Mocks](https://wcm.io/testing/aem-mock/usage.html) creates a mock context that allows these APIs to mostly act as if they are running in AEM.

1. BylineImplTest.javaファイルにデコレートされたJUnit拡張子として **BylineImplTest.javaファイルに** wcm.ioの `AemContext`**wcm.ioを使用してAEMコンテキスト** を作成します( `@ExtendWith`**** BylineImplTest.java)。 拡張機能は、必要な初期化とクリーンアップのタスクをすべて処理します。 すべてのテストメソッドで使用 `AemContext` できるクラス変数を作成します。

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   This variable, `ctx`, exposes a mock AEM context that provides a number of AEM and Sling abstractions:

   * BylineImpl Sling Model はこのコンテキストに登録されます。
   * モック JCR コンテンツ構造はこのコンテキストで作成されます。
   * カスタム OSGi サービスはこのコンテキスト内で登録できます。
   * 一般的に必要となる様々なモックオブジェクトおよびヘルパー（SlingHttpServletRequest オブジェクトなど）、様々なモック Sling および AEM OSGi サービス（ModelFactory、PageManager、ページ、テンプレート、ComponentManager、コンポーネント、TagManager、タグなど）を提供します。
      * *これらのオブジェクトのすべてのメソッドが実装されるわけではありません。*
   * [その他](https://wcm.io/testing/aem-mock/usage.html)

   The **`ctx`** object will act as the entry point for most of our mock context.

1. In the `setUp(..)` method, which is executed prior to each `@Test` method, define a common mock testing state:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** テストするSlingモデルをAEMコンテキストに登録し、メソッドでインスタンス化できるようにし `@Test` ます。
   * **`load().json`** では、リソース構造をモックコンテキストにロードし、コードが実際のリポジトリから提供されたかのようにこれらのリソースを操作できるようにします。 ファイル **`BylineImplTest.json`** のリソース定義は、**/content** の下でモック JCR コンテキストに読み込まれます。
   * **`BylineImplTest.json`** がまだないので、作成してテストに必要な JCR リソース構造を定義しましょう。

1. モックリソース構造を表す JSON ファイルは、JUnit Java テストファイルと同じパッケージパスに従い、**core/src/test/resources** 配下に保存されます。

   Create a new JSON file at **core/test/resources/com/adobe/aem/guides/wknd/core/models/impl** named **BylineImplTest.json** with the following content:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   この JSON は、署名コンポーネント単体テストのモックリソースを定義します。この時点で、JSON には、署名コンポーネントコンテンツリソースを表す最小限のプロパティのセットである `jcr:primaryType` および `sling:resourceType`。

   単体テストを扱う場合の一般的な規則は、各テストを満たすのに必要なモックコンテンツ、コンテキスト、コードの最小セットを作成することです。 テストを記述する前に完全なモックコンテキストを作成したいという誘惑は避けてください。不要なアーチファクトが生じることがよくあります。

   Now with the existence of **BylineImplTest.json**, when `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` is executed, the mock resource definitions are loaded into the context at the path **/content.**

## Testing getName() {#testing-get-name}

基本的なモックコンテキストの設定が完了したところで、**BylineImpl&#39;s getName()** の最初のテストを作成しましょう。このテストでは、メソッド **getName()** がリソースの &quot;**name**&quot; プロパティに保存されている、作成された正しい名前を返すことを確認する必要があります。

1. 次のように、**BylineImplTest.java** で **testGetName**() メソッドを更新します。

   ```java
   import com.adobe.aem.guides.wknd.core.components.Byline;
   import static org.junit.jupiter.api.Assertions.assertEquals;
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

   * **`String expected`** は、期待値を設定します。 We will set this to &quot;**Jane Done**&quot;.
   * **`ctx.currentResource`** コードを評価するモックリソースのコンテキストを設定します。このコンテキストは、モックバイラインコンテンツリソースが読み込まれる **/content/byline** に設定されます。
   * **`Byline byline`** mock RequestオブジェクトからByline Slingモデルを適用してインスタンス化します。
   * **`String actual`** Byline Sling Modelオブジェクトでテスト中のメソッド `getName()`を呼び出します。
   * **`assertEquals`** は、期待値がbyline Sling Modelオブジェクトが返す値と一致することをアサートします。 これらの値が等しくない場合、テストは失敗します。

1. テストを実行すると、...が表示されて失敗し `NullPointerException`ます。

   Note that this test does NOT fail because we never defined a `name` property in the mock JSON, that will cause the test to fail however the test execution hasn&#39;t gotten to that point! This test fails due to a `NullPointerException` on the byline object itself.

1. In the [Reviewing BylineImpl.java](#reviewing-bylineimpl-java) video above, we discuss how if `@PostConstruct init()` throws an exception it prevents the Sling Model from instantiating, and that is what&#39;s happening here.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   It turns out that while the ModelFactory OSGi service is provided via the `AemContext` (by way of the Apache Sling Context), not all methods are implemented, including `getModelFromWrappedRequest(...)` which is called in the BylineImpl&#39;s `init()` method. This results in an [AbstractMethodError](https://docs.oracle.com/javase/8/docs/api/java/lang/AbstractMethodError.html), which in term causes `init()` to fail, and the resulting adaption of the `ctx.request().adaptTo(Byline.class)` is a null object.

   Since the provided mocks cannot accommodate our code, we must implement the mock context ourselves For this, we can use Mockito to create a mock ModelFactory object, that returns a mock Image object when `getModelFromWrappedRequest(...)` is invoked upon it.

   Since in order to even instantiate the Byline Sling Model, this mock context must be in place, we can add it to the `@Before setUp()` method. We also need to add the `MockitoExtension.class` to the `@ExtendWith` annotation above the **BylineImplTest** class.

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
   
   import static org.junit.jupiter.api.Assertions.assertEquals;
   import static org.junit.jupiter.api.Assertions.fail;
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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** Mockito JUnit Jupiter Extension [](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) （Mockito JUnit Jupiter拡張）で実行されるテストケースクラスにマークを付けます。これにより、@Mock注釈を使用して、クラスレベルでモックオブジェクトを定義できます。
   * **`@Mock private Image`** タイプがmockオブジェクトを作成 `com.adobe.cq.wcm.core.components.models.Image`します。 Note that this is defined at the class level so that, as needed, `@Test` methods can alter its behavior as needed.
   * **`@Mock private ModelFactory`** モデルファクトリ型のモックオブジェクトを作成します。 これは純粋な Mockito モックであり、メソッドは実装されません。Note that this is defined at the class level so that, as needed, `@Test`methods can alter its behavior as needed.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** モックModelFactoryオブジェクトで呼び出さ `getModelFromWrappedRequest(..)` れたときのモック動作を登録します。 に定義された結果 `thenReturn (..)` は、モック画像オブジェクトを返します。 この動作は、次の場合にのみ呼び出されます。1番目のパラメーターは `ctx`のリクエストオブジェクトと等しく、2番目のパラメーターは任意のResourceオブジェクトで、3番目のパラメーターはCore Components Imageクラスである必要があります。 We accept any Resource because throughout our tests we will be setting the `ctx.currentResource(...)` to various mock resources defined in the **BylineImplTest.json**. 後でModelFactoryのこの動作を上書きしたいので、 **** lenient()の厳密性を追加します。
   * **`ctx.registerService(..)`.** モックのModelFactoryオブジェクトをAemContextに登録し、最も高いサービスランクを付けます。 This is required since the ModelFactory used in the BylineImpl&#39;s `init()` is injected via the `@OSGiService ModelFactory model` field. AemContextがへの呼び出しを処理する **モックオブジェクトを挿入するために**`getModelFromWrappedRequest(..)`は、そのタイプ(ModelFactory)の最上位のServiceとして登録する必要があります。

1. テストを再実行すると再び失敗しますが、今回は失敗の理由が明白です。

   ![テスト名失敗アサーション](assets/unit-testing/testgetname-failure-assertion.png)

   *アサーションによる testGetName() の失敗*

   **AssertionError** が返されます。これはテストでのアサート条件が失敗し、**期待値は &quot;Jane Doe&quot;** で、**実際の値が null** なことを示します。**BylineImplTest.json** のモック **/content/byline** リソース定義に &quot;**name**&quot; プロパティが追加されていないので、この結果は当然です。そこで、これを追加します。

1. Update **BylineImplTest.json** to define `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. Re-run the test, and **`testGetName()`** now passes!

## Testing getOccupations() {#testing-get-occupations}

成功です。最初のテストはうまくいきました。Let&#39;s move on and test `getOccupations()`. Since the initialization of the mock context was does in the `@Before setUp()`method, this will be available to all `@Test` methods in this Test Case, including `getOccupations()`.

このメソッドは、職業プロパティに保存されている職業のリストをアルファベット順（降順）に並べ替えて返します。

1. Update **`testGetOccupations()`** as follows:

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

   * **`List<String> expected`** 期待される結果を定義します。
   * **`ctx.currentResource`** /content/bylineにあるモックリソース定義に対するコンテキストを評価する現在のリソースを設定します。 これにより、モックリソースのコンテキストで **BylineImpl.java** が実行されるようにします。
   * **`ctx.request().adaptTo(Byline.class)`** mock RequestオブジェクトからByline Slingモデルを適用してインスタンス化します。
   * **`byline.getOccupations()`** Byline Sling Modelオブジェクトでテスト中のメソッド `getOccupations()`を呼び出します。
   * **`assertEquals(expected, actual)`** 期待されるリストは、実際のリストと同じであるとアサートします。

1. Remember, just like **`getName()`** above, the **BylineImplTest.json** does not define occupations, so this test will fail if we run it, since `byline.getOccupations()` will return an empty list.

   **BylineImplTest.json** を更新して職業のリストを含めます。すると、**`getOccupations()`** () によって職業が並べ替えられていることをテストで検証できるよう、このリストはアルファベット順以外に設定されます。

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

1. テストを実行すると、再び成功します。職業を並べ替えたことが良かったようです。

   ![Get Accupationsパス](assets/unit-testing/testgetoccupations-success.png)

   *testGetOccupations() は成功*

## Testing isEmpty() {#testing-is-empty}

The last method to test **`isEmpty()`**.

Testing `isEmpty()` is interesting as it requires testing for a variety of conditions. Reviewing **BylineImpl.java**&#39;s `isEmpty()` method the following conditions must be tested:

* 名前が空のときに true を返す。
* 職業が null または空のときに true を返す。
* 画像が空または src URL がない場合 true を返す。
* 名前、職業、および Image（  src URL)が存在する

これにより、`BylineImplTest.json` で特定の条件や新しいモックリソース構造をテストする新しいテストメソッドを作成して、これらのテストを駆動させる必要があります。

Note that this check allowed us to skip testing for when `getName()`, `getOccupations()` and `getImage()` are empty since the expected behavior of that state is tested via `isEmpty()`.

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

   **`"empty": {...}`** とのみを含む「empty」という名前の新しいリソース定義を定義 `jcr:primaryType` し `sling:resourceType`ます。

   Remember we load `BylineImplTest.json` into `ctx` before the execution of each test method in `@setUp`, so this new resource definition is immediately available to us in tests at **/content/empty.**

1. Update `testIsEmpty()` as follows, setting the current resource to the new &quot;**empty**&quot; mock resource definition.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   テストを実行し、成功することを確認します。

1. Next, create a set of methods to ensure that if any of the required data points (name, occupations, or image) are empty, `isEmpty()` returns true.

   For each test, a discrete mock resource definition is used, update **BylineImplTest.json** with the additional resource definitions for **without-name** and **without-occupations**.

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

   次のテストメソッドを作成し、これらの状態をそれぞれテストします。

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

   **`testIsEmpty()`** は空のモックリソース定義に対してテストを行い、真であ `isEmpty()` ると断言します。

   **`testIsEmpty_WithoutName()`** 職業を持つが名前を持たないモックリソース定義に対するテスト。

   **`testIsEmpty_WithoutOccupations()`** 名前は付けられているが職業がないモックリソース定義に対してテストを行います。

   **`testIsEmpty_WithoutImage()`** 名前と職業を持つモックリソース定義に対してテストを行いますが、モック画像をnullに戻すように設定します。 で定義されている `modelFactory.getModelFromWrappedRequest(..)`動作を上書きして、この呼び出しによって返されるImageオブジェクト `setUp()` がnullであることを確認します。 Mockitoのスタブ機能は厳しく、意味のあるコードを必要としません。 したがって、メソッドでの動作の上書きを明示的に示す **`lenient`** 設定をモックに設定しま `setUp()` す。

   **`testIsEmpty_WithoutImageSrc()`** 名前と職業を持つモックリソース定義に対してテストを行いますが、呼び出し時にモックイメージが空白の文字列を返すように設定 `getSrc()` します。

1. 最後に、コンポーネントが正しく設定されている場合、**isEmpty()** が false を返すようテストを記述します。この条件の場合は、完全に設定された署名コンポーネントを表す **/content/byline** を再使用できます。

   ```java
   @Test
   public void testIsNotEmpty() {
   ctx.currentResource("/content/byline");
   when(image.getSrc()).thenReturn("/content/bio.png");
   
   Byline byline = ctx.request().adaptTo(Byline.class);
   
   assertFalse(byline.isEmpty());
   }
   ```

## コードの有効範囲 {#code-coverage}

コードの有効範囲とは、単体テストの対象となるソースコードの量のことです。最近の IDE は、単体テストでどのソースコードが実行されたかを自動的にチェックするツールを提供します。コードの有効範囲自体はコード品質を表すものではありませんが、単体テストの対象とならない、ソースコードの重要な領域があるかどうかを理解するのに役立ちます。

1. Eclipse のプロジェクトエクスプローラーで **BylineImplTest.java** を右クリックし、**有効範囲の設定／JUnit テスト**&#x200B;を選択します。

   有効範囲概要ビューが開いていることを確認します（Window／ビューを表示／その他／Java／有効範囲）。

   これにより、このファイル内で単体テストを実行し、コードの有効範囲を示すレポートを提供します。クラスやメソッドを掘り下げると、ファイルのどの部分がテストされ、どの部分がテストされていないかが明確にわかります。

   ![コードカバレージとして実行する](assets/unit-testing/bylineimpl-coverage.png)

   *コードの有効範囲の概要*

   Eclipse では、単体テストでカバーされる各クラスとメソッドの範囲をすばやく確認できます。Eclipse はコード行をカラーコード化します。

   * **緑**&#x200B;は、1 つ以上のテストで実行されたコードです。
   * **黄色**&#x200B;は、どのテストでも検証されていないブランチを示します。
   * **赤**&#x200B;は、どのテストでの実行されていないコードを示します。

1. 有効範囲レポートでは、職業フィールドが null で空のリストを返したブランチが、一度も評価されていないことがわかります。これは、黄色の線571,86で示され、if/elseの分岐が実行されないことを示し、赤の線75で示され、コードの線は実行されないことを示す。

   ![カバー範囲の色分け](assets/unit-testing/coverage-color-coding.png)

1. This can be remedied by adding a test for `getOccupations()` that asserts an empty list is returned when there is no occupations value on the resource. 次の新しいテストメソッドを **BylineImplTests.java** に追加します。

   ```java
   @Test
   public void testGetOccupations_WithoutOccupations() {
       List<String> expected = Collections.emptyList();
   
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       List<String> actual = byline.getOccupations();
   
       assertEquals(expected, actual);
   }
   ```

   **`Collections.emptyList();`** は、期待値を空のリストに設定します。

   **`ctx.currentResource("/content/empty")`** 現在のリソースを/content/emptyに設定します。これは、「occumentations」プロパティが定義されていないことがわかっています。

1. Re-running the Coverage As, it reports that **BylineImpl.java** is now at 100% coverage, however there is still one branch that is not evaluated in isEmpty() which again has to do with the occupations. In this case, the occupations == null is being evaluated, however the occupations.isEmpty() is not since there is no mock resource definition that sets `"occupations": []`.

   ![testGetOccupations_WithoutOccupations() の有効範囲](assets/unit-testing/getoccupations-withoutoccupations.png)

   *testGetOccupations_WithoutOccupations() の有効範囲*

1. この問題は、職業を空のアレイに設定するモックリソース定義を使用する別のテストメソッドを作成することで簡単に解決できます。

   Add a new mock resource definition to **BylineImplTest.json** that is a copy of **&quot;without-occupations&quot;** and add a occupations property set to the empty array, and name it **&quot;without-occupations-empty-array&quot;**.

   ```json
   "without-occupations-empty-array": {
      "jcr:primaryType": "nt:unstructured",
      "sling:resourceType": "wknd/components/content/byline",
      "name": "Jane Doe",
      "occupations": []
    }
   ```

   Create a new **@Test** method in `BylineImplTest.java` that uses this new mock resource, asserts `isEmpty()` returns true.

   ```java
   @Test
   public void testIsEmpty_WithEmptyArrayOfOccupations() {
       ctx.currentResource("/content/without-occupations-empty-array");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   ![testIsEmpty_WithEmptyArrayOfOccupations() の有効範囲](assets/unit-testing/testisempty_withemptyarrayofoccupations.png)

   *testIsEmpty_WithEmptyArrayOfOccupations() の有効範囲*

1. 最後に追加した内容により、`BylineImpl.java` は条件パスをすべて評価して、100 ％のコード有効範囲を利用できます。

   The tests validate the expected behavior of `BylineImpl` without while relying on a minimal set of implementation details.

## ビルドの一部として単体テストを実行する {#running-unit-tests-as-part-of-the-build}

単体テストは、Maven ビルドの一部として実行し、成功する必要があります。これにより、アプリケーションをデプロイする前にすべてのテストが成功することを確認します。パッケージやインストールなどのMavenの目標を実行すると、自動的に呼び出され、プロジェクト内のすべての単体テストが合格する必要があります。

```shell
$ mvn package
```

![mvnパッケージの成功](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

同様に、テストメソッドが失敗するような変更を加えると、ビルドは失敗し、どのテストが失敗したのか、その理由は何かが報告されます。

![mvnパッケージの失敗](assets/unit-testing/mvn-package-fail.png)

## コードの確認 {#review-the-code}

GitHubに完了したコードを表示する [か](https://github.com/adobe/aem-guides-wknd) 、コードをローカルのGitブラッチに確認してデプロイ `unit-testing/solution`します。
