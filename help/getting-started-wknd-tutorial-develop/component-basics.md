---
title: AEM Sites使用の手引き — コンポーネントの基本
description: シンプルな「HelloWorld」の例を通して、Adobe Experience Manager(AEM)サイトコンポーネントの基盤となる技術を理解します。 HTL、Slingモデル、クライアント側ライブラリ、および作成者ダイアログのトピックについて説明します。
sub-product: サイト
feature: components, sling-models, htl
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
translation-type: tm+mt
source-git-commit: 9825f6c82aac6d57477286f651da94f05a672ea8
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 6%

---


# コンポーネントの基本 {#component-basics}

この章では、簡単な`HelloWorld`例を通して、Adobe Experience Manager(AEM)サイトコンポーネントの基盤となる技術を調べます。 オーサリング、HTL、Slingモデル、クライアント側ライブラリに関するトピックを含む、既存のコンポーネントに対して若干の変更が行われます。

## 前提条件 {#prerequisites}

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。

## 目的 {#objective}

1. HTMLを動的にレンダリングするHTLテンプレートとSlingモデルの役割について説明します。
1. コンテンツのオーサリングを容易にするためのダイアログの使用方法を理解します。
1. コンポーネントをサポートするCSSとJavaScriptを含めるためのクライアント側ライブラリの基本について説明します。

## 作成する内容 {#what-you-will-build}

この章では、非常に単純な`HelloWorld`コンポーネントに対していくつかの変更を行います。 `HelloWorld`コンポーネントを更新する過程で、AEMコンポーネントの開発の主な領域について学びます。

## チャプタースタータープロジェクト{#starter-project}

この章は、[AEMプロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)で生成された汎用プロジェクトに基づいて構築されます。 以下のビデオを視聴し、[前提条件](#prerequisites)を確認して開始してください。

>[!VIDEO](https://video.tv.adobe.com/v/30154/?quality=12&learn=on)

新しいコマンドラインターミナルを開き、次の操作を実行します。

1. 空のディレクトリで、[aem-guides-wknd](https://github.com/adobe/aem-guides-wknd)リポジトリをコピーします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   Cloning into 'aem-guides-wknd'...
   ```

   >[!NOTE]
   >
   > 必要に応じて、[`component-basics/start`](https://github.com/adobe/aem-guides-wknd/archive/component-basics/start.zip)ブランチを直接ダウンロードできます。

1. `aem-guides-wknd`ディレクトリに移動します。

   ```shell
   $ cd aem-guides-wknd
   ```

1. `component-basics/start`ブランチに切り替えます。

   ```shell
   $ git checkout component-basics/start
   Branch component-basics/start set up to track remote branch component-basics/start from origin.
   Switched to a new branch 'component-basics/start'
   ```

1. 次のコマンドを使用して、プロジェクトを構築し、AEMのローカルインスタンスにデプロイします。

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

1. [ローカル開発環境](overview.md#local-dev-environment)を設定する手順に従って、優先IDEにプロジェクトを読み込みます。

## コンポーネントオーサリング{#component-authoring}

コンポーネントは、Webページの小さなモジュラー構成要素と考えることができます。 コンポーネントを再利用するには、コンポーネントを設定できる必要があります。 これは、作成者ダイアログを通して実行されます。 次に、簡単なコンポーネントを作成し、ダイアログの値がAEMでどのように保持されるかを調べます。

>[!VIDEO](https://video.tv.adobe.com/v/30176/?quality=12&learn=on)

以下は、上記のビデオで実行される高レベルの手順です。

1. **WKNDサイト** `>` **US** **en**&#x200B;の下に、**基本コンポーネント**&#x200B;という名前の新しいページを作成します。`>`
1. 新追加しく作成したページの&#x200B;**Hello Worldコンポーネント**。
1. コンポーネントのダイアログを開き、テキストを入力します。 変更を保存して、ページに表示されるメッセージを確認します。
1. 開発者モードに切り替えて、CRXDE-Liteでコンテンツパスを表示し、コンポーネントインスタンスのプロパティを調べます。
1. CRXDE-Liteを使用して、`/apps/wknd/components/content/helloworld`にある`cq:dialog`スクリプトと`helloworld.html`スクリプトを表示します。

## HTML テンプレート言語（HTL）{#htl-templates}

HTML Template Language(HTML Template Language)または[HTL](https://docs.adobe.com/content/help/ja-JP/experience-manager-htl/using/getting-started/getting-started.html)は、AEMコンポーネントでコンテンツをレンダリングする際に使用される、軽量の重み付けのサーバー側テンプレート言語です。

次に、`HelloWorld` HTLスクリプトを更新して、テキストメッセージの前に別のあいさつ文を表示します。

>[!VIDEO](https://video.tv.adobe.com/v/30177/?quality=12&learn=on)

以下は、上記のビデオで実行される高レベルの手順です。

1. Eclipse IDEに切り替えて、`ui.apps`モジュールへのプロジェクトを開きます。
1. `.content.xml`ファイルを開き、`HelloWorld`コンポーネントのダイアログを定義します。次の場所にあります。

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/_cq_dialog/.content.xml
   ```

1. ダイアログを更新し、**Greeting**&#x200B;という名前の追加のテキストフィールドを`./greeting`という名前で追加します。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Properties"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns">
           <items jcr:primaryType="nt:unstructured">
               <column
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/container">
                   <items jcr:primaryType="nt:unstructured">
                       <greeting
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Greeting"
                           name="./greeting"/>
                       <text
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Text"
                           name="./text"/>
                   </items>
               </column>
           </items>
       </content>
   </jcr:root>
   ```

1. ファイル`helloworld.html`を開きます。これは、次の場所にある、`HelloWorld`コンポーネントのレンダリングを担当するメインのHTLスクリプトを表します。

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html
   ```

1. `helloworld.html`を更新して、**Greeting**&#x200B;テキストフィールドの値を`H1`タグの一部としてレンダリングします。

   ```html
   <h1 data-sly-test="${properties.text && properties.greeting}">${properties.greeting} ${properties.text}</h1>
   <pre data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
   HelloWorldModel says:
   ${hello.message}
   </pre>
   ```

1. Eclipse Developerプラグインを使用するか、Mavenのスキルを使用して、AEMのローカルインスタンスに変更をデプロイします。

## Sling Model {#sling-models}

Slingモデルは、JCRからJava変数へのデータのマッピングを容易にし、AEMのコンテキストで開発する際に、様々な点を提供する注釈駆動のJava 「POJO」(Plain Old Java Objects)です。

次に、`HelloWorldModel` Slingモデルを更新し、JCRに保存された値にビジネスロジックを適用してから、ページに出力します。

>[!VIDEO](https://video.tv.adobe.com/v/30189/?quality=12&learn=on)

1. ファイル`HelloWorldModel.java`を開きます。これは、`HelloWorld`コンポーネントで使用されるSlingモデルです。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 次の追加行を`HelloWorldModel`クラスに追加し、コンポーネントのJCRプロパティ`greeting`と`text`の値をJava変数にマッピングします。

   ```java
   ...
   @Model(adaptables = Resource.class)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String greeting;
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String text;
   
           @PostConstruct
           protected void init() {
               ...
   ```

1. &lt;a2/追加>という名前のプロパティの値を返す、次のメソッド`getGreeting()`を`HelloWorldModel`クラスに渡します。 `greeting`このメソッドは、プロパティ`greeting`がnullまたは空白の場合に「Hello」という文字列値を返す追加のロジックを追加します。

   ```java
   /***
   *
   * @return the value of greeting, if null or blank returns "Hello"
   */
   public String getGreeting() {
       return StringUtils.isNotBlank(this.greeting) ? this.greeting : "Hello";
   }
   ```

1. &lt;a2/追加>という名前のプロパティの値を返す、次のメソッド`getTextUpperCase()`を`HelloWorldModel`クラスに渡します。 `text`このメソッドは、文字列をすべてのupperCase文字に変換します。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getTextUpperCase() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html`の`helloworld.html`ファイルを更新し、新しく作成した`HelloWorld`モデルのメソッドを使用します。

   ```html
   <div class="cmp-helloworld" data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 data-sly-test="${hello.textUpperCase}">${hello.greeting} ${hello.textUpperCase}</h1>
       <pre>
       HelloWorldModel says:
       ${hello.message}
       </pre>
   </div>
   ```

1. Eclipse Developerプラグインを使用するか、Mavenのスキルを使用して、AEMのローカルインスタンスに変更をデプロイします。

## クライアントサイドライブラリ {#client-side-libraries}

クライアント側ライブラリ（簡単に言うとclientlib）は、AEM Sitesの実装に必要なCSSおよびJavaScriptファイルを整理および管理するメカニズムを提供します。 クライアント側ライブラリは、AEMのページにCSSとJavaScriptを含める標準的な方法です。

次に、クライアント側ライブラリの基本事項を理解するために、`HelloWorld`コンポーネントのCSSスタイルをいくつか含めます。

>[!VIDEO](https://video.tv.adobe.com/v/30190/?quality=12&learn=on)

以下は、上記のビデオで実行される高レベルの手順です。

1. `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld`の下に、`clientlibs`という名前の新しいノードを作成し、ノードタイプを`cq:ClientLibraryFolder`にします。
1. `clientlibs`の下に、次のようなフォルダーとファイル構造を作成します

   ```plain
   /helloworld
           /clientlibs
               /css/helloworld.css
               /js/helloworld.js
               +js.txt
               +css.txt
   ```

1. `helloworld/clientlibs/css/helloworld.css` に以下を入力します。

   ```css
   .cmp-helloworld h1 {
       color: red;
   }
   ```

1. `helloworld/clientlibs/css.txt` に以下を入力します。

   ```plain
   #base=css
   helloworld.css
   ```

1. `helloworld/clientlibs/js/helloworld.js` に以下を入力します。

   ```js
   console.log("hello world!");
   ```

1. `helloworld/clientlibs/js.txt` に以下を入力します。

   ```plain
   #base=js
   helloworld.js
   ```

1. `clientlibs`ノードのプロパティを更新し、次の2つのプロパティを含めます。

   | 名前 | タイプ | 値 |
   |------|------|-------|
   | categories | String | wknd.base |
   | allowProxy | Boolean | true |

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root
       xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       categories="wknd.base"
       allowProxy="{Boolean}true"/>
   ```

1. Eclipse Developerプラグインを使用するか、Mavenのスキルを使用して、AEMのローカルインスタンスに変更をデプロイします。

## バリデーターが{#congratulations}

おめでとうAdobe Experience Managerでコンポーネント開発の基礎を学びました！

### 次の手順 {#next-steps}

次の章「[ページとテンプレート](pages-templates.md)」で、Adobe Experience Managerのページとテンプレートについて説明します。 コアコンポーネントがプロジェクト内でどのようにプロキシ化されるかを理解し、適切に構成された記事ページテンプレートを作成するための、編集可能なテンプレートの高度なポリシー設定を学びます。

[GitHub](https://github.com/adobe/aem-guides-wknd)上の完了したコードを表示するか、Gitブラック`component-basics/solution`上のローカルにコードを確認して展開します。

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)リポジトリをコピーします。
1. `component-basics/solution`ブランチをチェックアウト
