---
title: AEM Sites使用の手引き — コンポーネントの基本
description: シンプルな「HelloWorld」の例を通して、Adobe Experience Manager(AEM)Sitesコンポーネントの基盤となるテクノロジーを理解します。 HTL、Slingモデル、クライアント側ライブラリ、オーサーダイアログのトピックを確認します。
sub-product: サイト
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: コアコンポーネント、開発者ツール
topic: コンテンツ管理、開発
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '1152'
ht-degree: 4%

---


# コンポーネントの基本 {#component-basics}

この章では、簡単な`HelloWorld`例を通して、Adobe Experience Manager(AEM)Sitesコンポーネントの基盤となるテクノロジーについて説明します。 オーサリング、HTL、Slingモデル、クライアント側ライブラリのトピックを含め、既存のコンポーネントに小さな変更が加えられます。

## 前提条件 {#prerequisites}

[ローカル開発環境](./overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。

このビデオで使用されるIDEは、[Visual Studio Code](https://code.visualstudio.com/)と[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)プラグインです。

## 目的 {#objective}

1. HTMLを動的にレンダリングするHTLテンプレートとSlingモデルの役割について説明します。
1. コンテンツのオーサリングを容易にするためにダイアログが使用される方法を理解します。
1. CSSとJavaScriptを含めてコンポーネントをサポートするためのクライアント側ライブラリの基本について説明します。

## 作成する内容 {#what-you-will-build}

この章では、非常に単純な`HelloWorld`コンポーネントに対していくつかの変更を行います。 `HelloWorld`コンポーネントを更新する過程で、AEMコンポーネント開発の主な領域について学びます。

## チャプタースタータープロジェクト{#starter-project}

この章は、[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)で生成された汎用プロジェクトに基づいて構築されます。 以下のビデオを見て、[前提条件](#prerequisites)を確認してください。

>[!NOTE]
>
> 前の章を正常に完了した場合は、プロジェクトを再利用し、スタータープロジェクトをチェックアウトする手順をスキップできます。

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

新しいコマンドラインターミナルを開き、次の操作を実行します。

1. 空のディレクトリで、[aem-guides-wknd](https://github.com/adobe/aem-guides-wknd)リポジトリのクローンを作成します。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > 必要に応じて、前の章[プロジェクトの設定](./project-setup.md)で生成したプロジェクトを引き続き使用できます。

1. `aem-guides-wknd`フォルダーに移動します。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 次のコマンドを使用して、プロジェクトを構築し、AEMのローカルインスタンスにデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5または6.4を使用している場合は、任意のMavenコマンドに`classic`プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. [ローカル開発環境](overview.md#local-dev-environment)を設定する手順に従って、優先IDEにプロジェクトを読み込みます。

## コンポーネントオーサリング{#component-authoring}

コンポーネントは、Webページの小さなモジュール式構成要素と考えることができます。 コンポーネントを再利用するには、コンポーネントを設定できる必要があります。 これは、オーサーダイアログを通じて実行します。 次に、単純なコンポーネントを作成し、ダイアログの値がAEMでどのように保持されるかを調べます。

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

上記のビデオで実行する高レベルの手順を以下に示します。

1. **WKND Site** `>` **US** `>` **en**&#x200B;の下に、**Component Basics**&#x200B;という名前の新しいページを作成します。
1. 新しく作成したページに&#x200B;**Hello Worldコンポーネント**&#x200B;を追加します。
1. コンポーネントのダイアログを開き、テキストを入力します。 変更を保存して、ページに表示されるメッセージを確認します。
1. 開発者モードに切り替えて、CRXDE-Liteでコンテンツパスを表示し、コンポーネントインスタンスのプロパティを調べます。
1. CRXDE-Liteを使用して、`/apps/wknd/components/content/helloworld`にある`cq:dialog`スクリプトと`helloworld.html`スクリプトを表示します。

## HTL（HTMLテンプレート言語）とダイアログ{#htl-dialogs}

HTML Template Language(**[HTL](https://docs.adobe.com/content/help/ja-JP/experience-manager-htl/using/getting-started/getting-started.html)**)は、コンテンツをレンダリングするためにAEMコンポーネントで使用される、軽量のサーバー側テンプレート言語です。

**** ダイアログでは、コンポーネントに対して作成できる設定を定義します。

次に、`HelloWorld` HTLスクリプトを更新し、テキストメッセージの前に追加の挨拶文を表示します。

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

上記のビデオで実行する高レベルの手順を以下に示します。

1. IDEに切り替え、`ui.apps`モジュールを開きます。
1. `helloworld.html`ファイルを開き、HTMLマークアップを変更します。
1. [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)などのIDEツールを使用して、ファイルの変更をローカルのAEMインスタンスと同期します。
1. ブラウザーに戻り、コンポーネントのレンダリングが変更されたことを確認します。
1. 次の場所にある`HelloWorld`コンポーネントのダイアログを定義する`.content.xml`ファイルを開きます。

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. **Title**&#x200B;という名前のテキストフィールドを`./title`という名前で追加するようにダイアログを更新します。

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
                       <title
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Title"
                           name="./title"/>
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

1. `helloworld.html`ファイルを再度開きます。このファイルは、次の場所にある`HelloWorld`コンポーネントのレンダリングを担当するメインのHTLスクリプトを表しています。

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. `helloworld.html`を更新して、**Greeting**&#x200B;テキストフィールドの値を`H1`タグの一部としてレンダリングします。

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. 開発者プラグインまたはMavenのスキルを使用して、変更をAEMのローカルインスタンスにデプロイします。

## Sling Model {#sling-models}

Slingモデルは、JCRからJava変数へのデータのマッピングを容易にし、AEMのコンテキストで開発する際に他の多くの点を提供する、注釈駆動型のJava「POJO」(Plain Old Java Objects)です。

次に、ページに出力する前に、JCRに格納された値にビジネスロジックを適用するために、 `HelloWorldModel` Sling Modelを更新します。

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. ファイル`HelloWorldModel.java`を開きます。これは、`HelloWorld`コンポーネントで使用されるSling Modelです。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 次のimport文を追加します。

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. `@Model`注釈を更新して、`DefaultInjectionStrategy`を使用します。

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. `HelloWorldModel`クラスに次の行を追加して、コンポーネントのJCRプロパティ`title`と`text`の値をJava変数にマッピングします。

   ```java
   ...
   @Model(adaptables = Resource.class,
   defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue
       private String title;
   
       @ValueMapValue
       private String text;
   
       @PostConstruct
       protected void init() {
           ...
   ```

1. `title`という名前のプロパティの値を返すメソッド`getTitle()`を`HelloWorldModel`クラスに追加します。 このメソッドは、「Default Value here!」のString値を返すロジックを追加します。 プロパティ`title`がnullまたは空白の場合：

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. `text`という名前のプロパティの値を返すメソッド`getText()`を`HelloWorldModel`クラスに追加します。 このメソッドは、文字列をすべて大文字に変換します。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. `core`モジュールからバンドルをビルドし、デプロイします。

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > AEM 6.4/6.5を使用している場合は、`mvn clean install -PautoInstallBundle -Pclassic`を使用します。

1. `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html`の`helloworld.html`ファイルを更新し、`HelloWorld`モデルの新しく作成したメソッドを使用します。

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld"
   data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 class="cmp-helloworld__title">${model.title}</h1>
       <div class="cmp-helloworld__item" data-sly-test="${properties.text}">
           <p class="cmp-helloworld__item-label">Text property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.text}</pre>
       </div>
       <div class="cmp-helloworld__item" data-sly-test="${model.text}">
           <p class="cmp-helloworld__item-label">Sling Model getText() property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${model.text}</pre>
       </div>
       <div class="cmp-helloworld__item"  data-sly-test="${model.message}">
           <p class="cmp-helloworld__item-label">Model message:</p>
           <pre class="cmp-helloworld__item-output"data-cmp-hook-helloworld="model">${model.message}</pre>
       </div>
   </div>
   ```

1. Eclipse DeveloperプラグインまたはMavenスキルを使用して、AEMのローカルインスタンスに変更をデプロイします。

## クライアントサイドライブラリ {#client-side-libraries}

クライアント側ライブラリ(clientlib)は、AEM Sitesの実装に必要なCSSおよびJavaScriptファイルを整理および管理するメカニズムを提供します。 クライアント側ライブラリは、AEMのページにCSSとJavaScriptを含める標準的な方法です。

次に、クライアント側ライブラリの基本を理解するために、 `HelloWorld`コンポーネントのCSSスタイルをいくつか含めます。

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

上記のビデオで実行する高レベルの手順を以下に示します。

1. `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs`の下に、`clientlib-helloworld`という名前の新しいフォルダーを作成します。
1. `clientlibs`の下に、次のようなフォルダーとファイル構造を作成します。

   ```plain
   /clientlib-helloworld
       /css/helloworld.css
       /js/helloworld.js
       +js.txt
       +css.txt
       +.content.xml
   ```

1. `helloworld.css` に以下を入力します。

   ```css
   .cmp-helloworld .cmp-helloworld__title {
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
   console.log("Hello World from Javascript!");
   ```

1. `helloworld/clientlibs/js.txt` に以下を入力します。

   ```plain
   #base=js
   helloworld.js
   ```

1. `clientlib-helloworld/.conten.xml`ファイルを更新して、次のプロパティを含めます。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.helloworld]" />
   ```

1. `clientlibs/clientlib-base/.content.xml`ファイルを&#x200B;**embed**&#x200B;に更新し、`wknd.helloworld`カテゴリをします。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.base]"
       embed="[core.wcm.components.accordion.v1,core.wcm.components.tabs.v1,core.wcm.components.carousel.v1,core.wcm.components.image.v2,core.wcm.components.breadcrumb.v2,core.wcm.components.search.v1,core.wcm.components.form.text.v2,core.wcm.components.pdfviewer.v1,core.wcm.components.commons.datalayer.v1,wknd.grid,wknd.helloworld]"/>
   ```

1. 開発者プラグインまたはMavenのスキルを使用して、変更をAEMのローカルインスタンスにデプロイします。

   >[!NOTE]
   >
   > パフォーマンス上の理由から、CSSとJavaScriptはブラウザーによって頻繁にキャッシュされます。 クライアントライブラリの変更がすぐに確認されない場合は、ハードリフレッシュを実行し、ブラウザのキャッシュをクリアします。 新しいキャッシュを確実に作成するには、匿名ウィンドウを使用すると便利です。

## バリデーターが {#congratulations}

おめでとう、Adobe Experience Managerでのコンポーネント開発の基本を学びました。

### 次の手順 {#next-steps}

次の章[ページとテンプレート](pages-templates.md)で、Adobe Experience Managerのページとテンプレートについて説明します。 コアコンポーネントがプロジェクト内でどのようにプロキシされるかを理解し、適切に構造化された記事ページテンプレートを構築するための、編集可能なテンプレートの高度なポリシー設定について学びます。

[GitHub](https://github.com/adobe/aem-guides-wknd)で完成したコードを表示するか、Gitブランチ`tutorial/component-basics-solution`のローカルにあるコードを確認してデプロイします。
