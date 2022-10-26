---
title: 単体テスト
description: カスタムコンポーネントのチュートリアルで作成した署名コンポーネントの Sling Model の動作を検証する単体テストを実装します。
version: 6.5, Cloud Service
type: Tutorial
feature: APIs, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
exl-id: b926c35e-64ad-4507-8b39-4eb97a67edda
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '3014'
ht-degree: 27%

---

# 単体テスト {#unit-testing}

このチュートリアルでは、単体テストの実装について説明します。このテストは、 [カスタムコンポーネント](./custom-component.md) チュートリアル

## 前提条件 {#prerequisites}

設定に必要なツールと手順を確認します。 [ローカル開発環境](overview.md#local-dev-environment).

_Java 8 と Java 11 の両方がシステムにインストールされている場合、VS Code テストランナーは、テストの実行時に低い Java ランタイムを選択し、テストが失敗する可能性があります。 この場合は、Java 8 をアンインストールします。_

### スタータープロジェクト

>[!NOTE]
>
> 前の章を正常に完了した場合は、プロジェクトを再利用し、スタータープロジェクトをチェックアウトする手順をスキップできます。

チュートリアルの構築元となるベースラインコードを確認します。

1. 以下を確認します。 `tutorial/unit-testing-start` ～から分岐する [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Maven のスキルを使用して、ローカルのAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 または 6.4 を使用している場合、 `classic` 任意の Maven コマンドに対するプロファイル。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

完成したコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) または、ブランチに切り替えて、コードをローカルでチェックアウトします。 `tutorial/unit-testing-start`.

## 目的

1. 単体テストの基本を理解します。
1. AEMコードのテストに一般的に使用されるフレームワークとツールについて説明します。
1. 単体テストを記述する際のAEMリソースのモッキングまたはシミュレーションのオプションについて説明します。

## 背景 {#unit-testing-background}

このチュートリアルでは、 [単体テスト](https://ja.wikipedia.org/wiki/%E5%8D%98%E4%BD%93%E3%83%86%E3%82%B9%E3%83%88) 署名コンポーネントの [Sling モデル](https://sling.apache.org/documentation/bundles/models.html) ( [カスタムAEMコンポーネントの作成](custom-component.md)) をクリックします。 単体テストは、Java コードの期待される動作を検証するために Java で記述されるビルド時間テストです。各単体テストは通常小さく、期待される結果に対してメソッド（または作業単位）の出力を検証します。

アドビはAEMのベストプラクティスを使用し、以下を採用しています。

* [JUnit 5](https://junit.org/junit5/)
* [Mockito テストフレームワーク](https://site.mockito.org/)
* [wcm.io テストフレームワーク](https://wcm.io/testing/) ( に基づく [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html))

## 単体テストとAdobeCloud Manager {#unit-testing-and-adobe-cloud-manager}

[AdobeCloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=ja) 単体テストの実行と [コードカバレッジレポート](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) を CI/CD パイプラインに統合して、AEMコードの単体テストのベストプラクティスを促進し、促進します。

コードの単体テストはあらゆるコードベースで有益ですが、Cloud Manager を使用している場合は、Cloud Manager で実行できる単体テストを提供して、コード品質のテストや報告機能を活用することが重要です。

## テスト Maven の依存関係を更新する {#inspect-the-test-maven-dependencies}

最初の手順は、Maven の依存関係を調べて、テストの作成と実行をサポートすることです。 次の 4 つの依存関係が必要です。

1. JUnit5
1. Mockito テストフレームワーク
1. Apache Sling Mocks
1. AEM Mocks テストフレームワーク (io.wcm)

この **JUnit5**, **Mockito** および **AEM Mocks** テスト依存関係は、 [AEM Maven アーキタイプ](project-setup.md).

1. これらの依存関係を表示するには、次の場所にある親リアクター POM を開きます。 **aem-guides-wknd/pom.xml**&#x200B;をクリックし、 `<dependencies>..</dependencies>` JUnit、Mockito、Apache Sling Mocks、AEM Mock Tests の依存関係を io.wcm で `<!-- Testing -->`.
1. 以下を確認します。 `io.wcm.testing.aem-mock.junit5` が **4.1.0**:

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
   > アーキタイプ **35** プロジェクトを次の形式で生成します。 `io.wcm.testing.aem-mock.junit5` version **4.1.8**. にダウングレードしてください **4.1.0** この章の残りの部分を追う

1. 開く **aem-guides-wknd/core/pom.xml** 対応するテストの依存関係が使用可能であることを表示します。

   内の並列ソースフォルダー **コア** プロジェクトには、単体テストとサポートするテストファイルが含まれます。 この **test** フォルダーはテストクラスをソースコードから分離しますが、ソースコードと同じパッケージ内にあるようにテストを動作させることができます。

## JUnit テストの作成 {#creating-the-junit-test}

単体テストは一般的に、Java クラスと 1 対 1 でマッピングします。この章では、署名コンポーネントを支える Sling Model である **BylineImpl.java** 用に JUnit テストを記述します。

![単体テスト用の src フォルダー](assets/unit-testing/core-src-test-folder.png)

*単体テストが保存される場所。*

1. 単体テストの作成 `BylineImpl.java` 新しい Java クラスを `src/test/java` テストする Java クラスの場所を反映する Java パッケージフォルダー構造。

   ![新しい BylineImplTest.java ファイルを作成します](assets/unit-testing/new-bylineimpltest.png)

   テスト中なので

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   対応する単体テスト Java クラスを次の場所に作成

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   この `Test` 単体テストファイルのサフィックス `BylineImplTest.java` 我々に許す慣例だ

   1. テストファイルとして容易に識別できます _対象_ `BylineImpl.java`
   1. ただし、テストファイルを区別します _から_ 試験を受けているクラス `BylineImpl.java`



## BylineImplTest.java のレビュー {#reviewing-bylineimpltest-java}

この時点で、JUnit テストファイルは空の Java クラスです。

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

1. 最初のメソッド `public void setUp() { .. }` JUnit の `@BeforeEach`：このクラスの各テストメソッドを実行する前に、JUnit テストランナーにこのメソッドを実行するように指示します。 これは、すべてのテストで必要な一般的なテスト状態を初期化する便利な場所です。

1. 以降のメソッドは、名前の前にが付いたテストメソッドです。 `test` 慣例によって、および `@Test` 注釈。 デフォルトでは、まだ実装されていないので、すべてのテストが失敗するように設定されています。

   まず、テストするクラスのパブリックメソッドごとに 1 つのテストメソッドから始めます。そのため、次のようにします。

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | がテスト対象 | testGetName() |
   | getOccupations() | がテスト対象 | testGetOccupations() |
   | isEmpty() | がテスト対象 | testIsEmpty() |

   これらのメソッドは、必要に応じて拡張できます。詳しくは、この章で後述します。

   この JUnit テストクラス（JUnit テストケースとも呼ばれます）を実行すると、各メソッドは `@Test` は、合格または不合格のどちらかを示すテストとして実行されます。

![生成された BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. JUnit テストケースを実行するには、 `BylineImplTest.java` ファイルとタップ **実行**.
まだ実装されていないので、予想どおり、すべてのテストが失敗します。

   ![junit テストとして実行](assets/unit-testing/run-junit-tests.png)

   *BylineImplTests.java を右クリックし、「実行」を選択します。*

## BylineImpl.java のレビュー {#reviewing-bylineimpl-java}

単体テストを記述する場合、次の 2 つの主なアプローチがあります。

* [TDD またはテスト駆動開発](https://ja.wikipedia.org/wiki/%E3%83%86%E3%82%B9%E3%83%88%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA)。実装を開発する直前に単体テストの増分を記述、テストを記述、実装を記述してテストを合格します。
* 最初に実装をおこなう開発。動作するコードを最初に開発してから、そのコードを検証するテストを記述します。

このチュートリアルでは、後者のアプローチを使用します（前の章で動作する **BylineImpl.java** を作成済みのため）。このため、パブリックメソッドの動作だけでなく、いくつかの実装の詳細についても確認および理解しておく必要があります。これは正しいテストでは入力と出力のみを考慮する必要があるので、反対に聞こえる場合がありますが、AEMで作業する場合、作業テストを構築するためには、実装に関する様々な考慮事項を理解する必要があります。

AEM における TDD には高度な専門知識が必要です。AEM 開発や AEM コードの単体テストを熟知した AEM 開発者が使用することで最大限の効果を発揮できます。

## AEM テストコンテキストの設定  {#setting-up-aem-test-context}

AEM で記述されるコードの大部分は JCR、Sling または AEM API に依存しているので、正常に実行するためには実行中の AEM のコンテキストが必要となります。

単体テストは、実行中のAEMインスタンスのコンテキスト以外で、ビルド時に実行されるので、そのようなコンテキストはありません。 これを容易にするには、 [wcm.io のAEM Mocks](https://wcm.io/testing/aem-mock/usage.html) これらの API がを可能にするモックコンテキストを作成します。 _mostly_ AEMで実行しているかのように動作します。

1. を使用したAEMコンテキストの作成 **wcm.io** `AemContext` in **BylineImplTest.java** を使用して装飾された JUnit 拡張機能として追加する `@ExtendWith` から **BylineImplTest.java** ファイル。 拡張機能は、必要な初期化タスクとクリーンアップタスクをすべて処理します。 のクラス変数の作成 `AemContext` すべてのテストメソッドで使用できます。

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   この変数 `ctx`を使用すると、AEMおよび Sling の多数の抽象概念を提供するモックAEMコンテキストが公開されます。

   * BylineImpl Sling Model は、このコンテキストに登録されます
   * モック JCR コンテンツ構造はこのコンテキストで作成されます。
   * カスタム OSGi サービスはこのコンテキスト内で登録できます。
   * 一般的に必要となる様々なモックオブジェクトおよびヘルパー（SlingHttpServletRequest オブジェクトなど）、様々なモック Sling および AEM OSGi サービス（ModelFactory、PageManager、ページ、テンプレート、ComponentManager、コンポーネント、TagManager、タグなど）を提供します。
      * *これらのオブジェクトのすべてのメソッドが実装されるわけではありません。*
   * [その他](https://wcm.io/testing/aem-mock/usage.html)

   この **`ctx`** オブジェクトは、モックコンテキストのほとんどのエントリポイントとして機能します。

1. 内 `setUp(..)` メソッド。各 `@Test` メソッドで、一般的なモックテスト状態を定義します。

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** テストする Sling モデルをモックAEM Context に登録し、でインスタンス化できるようにします。 `@Test` メソッド。
   * **`load().json`** は、リソース構造をモックコンテキストに読み込み、コードは、実際のリポジトリで提供された場合と同様に、これらのリソースを操作できます。 ファイル **`BylineImplTest.json`** のリソース定義は、**/content** の下でモック JCR コンテキストに読み込まれます。
   * **`BylineImplTest.json`** がまだないので、作成してテストに必要な JCR リソース構造を定義しましょう。

1. モックリソース構造を表す JSON ファイルは、JUnit Java テストファイルと同じパッケージパスに従い、**core/src/test/resources** 配下に保存されます。

   に新しい JSON ファイルを作成します。 `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` 名前付き **BylineImplTest.json** 次の内容を含む

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   この JSON は、署名コンポーネントの単体テスト用のモックリソース（JCR ノード）を定義します。 この時点で、JSON には、署名コンポーネントコンテンツリソースを表すのに必要な最小限のプロパティのセット、つまり `jcr:primaryType` および `sling:resourceType`.

   単体テストを扱う際の一般的なルールは、各テストを満たすのに必要な最小限のモックコンテンツ、コンテキスト、コードのセットを作成することです。 テストを記述する前に完全なモックコンテキストを構築したいと思わないでください。不要なアーティファクトが生じることがよくあります。

   今、 **BylineImplTest.json**、 `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` が実行されると、モックリソース定義がコンテキストのパスに読み込まれます。 **/content.**

## getName() のテスト {#testing-get-name}

基本的なモックコンテキストの設定が完了したところで、**BylineImpl&#39;s getName()** の最初のテストを作成しましょう。このテストでは、メソッド **getName()** がリソースの &quot;**name**&quot; プロパティに保存されている、作成された正しい名前を返すことを確認する必要があります。

1. 次のように、**BylineImplTest.java** で **testGetName**() メソッドを更新します。

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

   * **`String expected`** 期待値を設定します。 これを「**ジェーン完了**&quot;.
   * **`ctx.currentResource`** コードを評価するモックリソースのコンテキストを設定します。そのため、これはに設定されます。 **/content/byline** モック署名コンテンツリソースが読み込まれる場所と同じです。
   * **`Byline byline`** モック Request オブジェクトから適応させて署名 Sling モデルをインスタンス化します。
   * **`String actual`** は、テスト中のメソッドを呼び出します。 `getName()`署名 Sling Model オブジェクトに対して、
   * **`assertEquals`** は、期待される値を、署名 Sling Model オブジェクトが返す値と一致させます。 これらの値が等しくない場合、テストは失敗します。

1. テストを実行すると、 `NullPointerException`.

   このテストは、 `name` プロパティを設定します。これはテストが失敗する原因となりますが、テストの実行がその時点に達していない場合に失敗します。 このテストは、 `NullPointerException` 署名オブジェクト自体に対して

1. 内 `BylineImpl.java`、 `@PostConstruct init()` 例外をスローして Sling Model のインスタンス化を防ぎ、Sling Model オブジェクトが null になるようにします。

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   ModelFactory OSGi サービスは `AemContext` （Apache Sling Context を介して）、以下を含むすべてのメソッドが実装されているわけではありません。 `getModelFromWrappedRequest(...)` BylineImpl の `init()` メソッド。 その結果、 [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html) — 用語の原因 `init()` 失敗し、その結果としての適応が `ctx.request().adaptTo(Byline.class)` は null オブジェクトです。

   指定したモックはコードを受け入れられないので、モックコンテキストを自分で実装する必要があります。そのために、Mockito を使用して、モック Image オブジェクトを返すモック ModelFactory オブジェクトを作成できます。 `getModelFromWrappedRequest(...)` が呼び出されます。

   署名 Sling モデルをインスタンス化する場合でも、このモックコンテキストを配置する必要があるので、これを `@Before setUp()` メソッド。 また、 `MockitoExtension.class` から `@ExtendWith` 上の注釈 **BylineImplTest** クラス。

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** は、テストケースクラスを [Mockito JUnit Jupiter 拡張機能](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) これにより、 @Mock注釈を使用して、クラスレベルでモックオブジェクトを定義できます。
   * **`@Mock private Image`** は、型のモックオブジェクトを作成します `com.adobe.cq.wcm.core.components.models.Image`. 必要に応じて、クラスレベルで定義されます。 `@Test` メソッドでは、必要に応じて動作を変更できます。
   * **`@Mock private ModelFactory`** は、型が ModelFactory のモックオブジェクトを作成します。 これは純粋な Mockito モックであり、メソッドは実装されません。必要に応じて、クラスレベルで定義されます。 `@Test`メソッドでは、必要に応じて動作を変更できます。
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** 次の場合にモック動作を登録 `getModelFromWrappedRequest(..)` が、モック ModelFactory オブジェクトで呼び出されます。 で定義された結果 `thenReturn (..)` は、モック Image オブジェクトを返します。 この動作は、次の場合にのみ呼び出されます。第 1 のパラメータは `ctx`のリクエストオブジェクト、2 番目のパラメーターは任意の Resource オブジェクトで、3 番目のパラメーターはコアコンポーネントの Image クラスである必要があります。 すべてのリソースを受け入れます。これは、テスト全体を通じて、 `ctx.currentResource(...)` を **BylineImplTest.json**. なお、 **lenient()** 厳密性。後で ModelFactory のこの動作を上書きする必要があるためです。
   * **`ctx.registerService(..)`** は、最も高いサービスランキングを持つモック ModelFactory オブジェクトを AemContext に登録します。 BylineImpl で使用される ModelFactory のため、これは必須です `init()` が `@OSGiService ModelFactory model` フィールドに入力します。 AemContext が挿入するために **ur** への呼び出しを処理するモックオブジェクト `getModelFromWrappedRequest(..)`を使用する場合は、そのタイプの最上位の Service(ModelFactory) として登録する必要があります。

1. テストを再実行すると再び失敗しますが、今回は失敗の理由が明白です。

   ![テスト名の失敗のアサーション](assets/unit-testing/testgetname-failure-assertion.png)

   *アサーションによる testGetName() の失敗*

   **AssertionError** が返されます。これはテストでのアサート条件が失敗し、**期待値は &quot;Jane Doe&quot;** で、**実際の値が null** なことを示します。**BylineImplTest.json** のモック **/content/byline** リソース定義に &quot;**name**&quot; プロパティが追加されていないので、この結果は当然です。そこで、これを追加します。

1. 更新 **BylineImplTest.json** 定義する `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. テストを再実行し、 **`testGetName()`** 今度は通り過ぎだ！

   ![テスト名の合格](assets/unit-testing/testgetname-pass.png)


## getOccupations() のテスト {#testing-get-occupations}

成功です。最初のテストはうまくいきました。次に、テストに進みましょう `getOccupations()`. モックコンテキストの初期化は `@Before setUp()`メソッド、すべての `@Test` このテストケースのメソッド。次を含みます。 `getOccupations()`.

このメソッドは、職業プロパティに保存されている職業のリストをアルファベット順（降順）に並べ替えて返します。

1. 更新 **`testGetOccupations()`** 次のように指定します。

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
   * **`ctx.currentResource`** /content/byline にあるモックリソース定義に対するコンテキストを評価する現在のリソースを設定します。 これにより、モックリソースのコンテキストで **BylineImpl.java** が実行されるようにします。
   * **`ctx.request().adaptTo(Byline.class)`** モック Request オブジェクトから適応させて署名 Sling モデルをインスタンス化します。
   * **`byline.getOccupations()`** は、テスト中のメソッドを呼び出します。 `getOccupations()`署名 Sling Model オブジェクトに対して、
   * **`assertEquals(expected, actual)`** は、期待されるリストを実際のリストと同じにアサートします。

1. 次のように覚えておいてください。 **`getName()`** 上に **BylineImplTest.json** 職業を定義していないので、実行するとこのテストは失敗します。 `byline.getOccupations()` 空のリストを返します。

   更新 **BylineImplTest.json** 職業のリストを含め、それらをアルファベット以外の順序で設定し、テストで、職業がアルファベット順に並べ替えられていることを検証する **`getOccupations()`**.

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

   ![職業パスを取得](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations() は成功*

## isEmpty() のテスト {#testing-is-empty}

最後にテストしたメソッド **`isEmpty()`**.

テスト `isEmpty()` は、様々な条件のテストを必要とするので、興味深いものです。 レビュー中 **BylineImpl.java**&#39;s `isEmpty()` メソッドは、次の条件をテストする必要があります。

* 名前が空のときに true を返す。
* 職業が null または空のときに true を返す。
* 画像が空または src URL がない場合 true を返す。
* 名前、職業、および Image（  src URL) が存在する

これにより、`BylineImplTest.json` で特定の条件や新しいモックリソース構造をテストする新しいテストメソッドを作成して、これらのテストを駆動させる必要があります。

このチェックでは、のテストをスキップできたことに注意してください。 `getName()`, `getOccupations()` および `getImage()` は空です。この状態の期待される動作は `isEmpty()`.

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

   **`"empty": {...}`** 「empty」という名前で、 `jcr:primaryType` および `sling:resourceType`.

   読み込みを記憶する `BylineImplTest.json` into `ctx` 各テストメソッドを `@setUp`したがって、この新しいリソース定義は、次の場所のテストですぐに使用できます。 **/content/empty.**

1. 更新 `testIsEmpty()` 次のように、現在のリソースを新しい&#x200B;**空**&quot;モックリソース定義。

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   テストを実行し、成功することを確認します。

1. 次に、必要なデータポイント（名前、職業、または画像）のいずれかが空の場合に、そのデータポイントを確実に使用するメソッドのセットを作成します。 `isEmpty()` は true を返します。

   各テストで、個別のモックリソース定義が使用され、更新されます。 **BylineImplTest.json** と、 **without-name** および **職業のない**.

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

   **`testIsEmpty()`** は、空のモックリソース定義に対してテストし、 `isEmpty()` が true の場合は除外されます。

   **`testIsEmpty_WithoutName()`** は、職業を持つが名前を持たないモックリソース定義に対してテストします。

   **`testIsEmpty_WithoutOccupations()`** は、名前を持つが職業がないモックリソース定義に対してテストします。

   **`testIsEmpty_WithoutImage()`** は、名前と職業を持つモックリソース定義に対してテストを行い、モック画像が null に戻るように設定します。 なお、 `modelFactory.getModelFromWrappedRequest(..)`で定義された動作 `setUp()` を指定して、この呼び出しによって返される画像オブジェクトが null であることを確認します。 Mockito の stubs 機能は厳しく、偶発的なコードを望んでいません。 したがって、モックを **`lenient`** 設定を使用して、 `setUp()` メソッド。

   **`testIsEmpty_WithoutImageSrc()`** は、名前と職業を持つモックリソース定義に対してテストを行いますが、で空白の文字列を返すようにモック画像を設定します。 `getSrc()` が呼び出されます。

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

1. 次に、BylineImplTest.java ファイル内のすべての単体テストを実行し、Java テストレポートの出力を確認します。

![すべてのテストが合格](./assets/unit-testing/all-tests-pass.png)

## ビルドの一部として単体テストを実行する {#running-unit-tests-as-part-of-the-build}

単体テストは、Maven ビルドの一部として実行し、成功する必要があります。これにより、アプリケーションをデプロイする前にすべてのテストが成功することを確認します。パッケージやインストールなどの Maven 目標を実行すると、が自動的に呼び出され、プロジェクト内のすべての単体テストに合格する必要があります。

```shell
$ mvn package
```

![mvn パッケージの成功](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

同様に、テストメソッドが失敗するような変更を加えると、ビルドは失敗し、どのテストが失敗したのか、その理由は何かが報告されます。

![mvn パッケージ失敗](assets/unit-testing/mvn-package-fail.png)

## コードの確認 {#review-the-code}

で完成したコードを表示する [GitHub](https://github.com/adobe/aem-guides-wknd) または、Git ブラッチ上のローカルのにコードを確認してデプロイします。 `tutorial/unit-testing-solution`.
