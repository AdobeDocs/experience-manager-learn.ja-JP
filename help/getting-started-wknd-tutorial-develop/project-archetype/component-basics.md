---
title: AEM Sites の基本を学ぶ - コンポーネントの基本
description: 簡単な「HelloWorld」の例を通して、Adobe Experience Manager（AEM）Sites コンポーネントの基盤となるテクノロジーを説明します。HTL、Sling モデル、クライアントサイドライブラリおよびオーサーダイアログのトピックを調べます。
version: 6.5, Cloud Service
feature: Core Components, Developer Tools
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4081
thumbnail: 30177.jpg
doc-type: Tutorial
exl-id: 7fd021ef-d221-4113-bda1-4908f3a8629f
recommendations: noDisplay, noCatalog
duration: 1778
source-git-commit: 0400242f6a99bc5209a8b483469d5fd88eac077e
workflow-type: tm+mt
source-wordcount: '1192'
ht-degree: 100%

---

# コンポーネントの基本 {#component-basics}

この章では、簡単な `HelloWorld` の例を通して、Adobe Experience Manager（AEM）Sites コンポーネントの基盤となるテクノロジーを調べます。オーサリング、HTL、Sling モデル、クライアントサイドライブラリなどのトピックに対応する小さな変更が、既存のコンポーネントに加えられます。

## 前提条件 {#prerequisites}

[ローカル開発環境](./overview.md#local-dev-environment)の設定に必要なツールや手順を確認します。

ビデオで使用されている IDE は、[Visual Studio Code](https://code.visualstudio.com/) と [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) プラグインです。

## 目的 {#objective}

1. HTML を動的にレンダリングするための HTL テンプレートと Sling モデルの役割を学びます。
1. ダイアログを使用してコンテンツのオーサリングを促進する方法を理解します。
1. コンポーネントをサポートする CSS と JavaScript を組み込むためのクライアントサイドライブラリの基本中の基本を学びます。

## 作ろうとしているもの {#what-build}

この章では、簡単な `HelloWorld` コンポーネントにいくつかの変更を加えます。`HelloWorld` コンポーネントを更新しながら、AEM コンポーネント開発の主要な領域について学びます。

## チャプタースタータープロジェクト {#starter-project}

この章は、[AEM プロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)で生成された汎用プロジェクトに基づいています。以下のビデオを視聴し、[前提条件](#prerequisites)を確認してから始めましょう。

>[!NOTE]
>
> 前の章を正常に完了した場合は、プロジェクトを再利用して、スタータープロジェクトをチェックアウトする手順をスキップできます。

>[!VIDEO](https://video.tv.adobe.com/v/330985?quality=12&learn=on)

新しいコマンドラインターミナルを開き、以下のアクションを実行します。

1. 空のディレクトリで、[aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) リポジトリのクローンを作成します。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > オプションで、前の章[プロジェクトの設定](./project-setup.md)で生成されたプロジェクトを引き続き使用することもできます。

1. `aem-guides-wknd` フォルダー内に移動します。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 次のコマンドを使用して、プロジェクトをビルドし、AEM のローカルインスタンスにデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 または 6.4 を使用している場合は、`classic` プロファイルをすべての Maven コマンドに追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. [ローカル開発環境](overview.md#local-dev-environment)を設定する手順に従って、プロジェクトを好みの IDE に読み込みます。

## コンポーネントのオーサリング {#component-authoring}

コンポーネントは、web ページの小さなモジュール型構築ブロックと考えることができます。 コンポーネントを再利用するには、コンポーネントを設定可能にする必要があります。 これは、オーサーダイアログを通じて実行します。次に、簡単なコンポーネントを作成して、ダイアログ内の値が AEM にどのように保持されるかを調べます。

>[!VIDEO](https://video.tv.adobe.com/v/330986?quality=12&learn=on)

上記のビデオで実行している大まかな手順を以下に示します。

1. **WKND Site** `>` **US** `>` **en** の下に **Component Basics** という名前のページを作成します。
1. 新しく作成したページに **Hello World コンポーネント**&#x200B;を追加します。
1. コンポーネントのダイアログを開き、テキストを入力します。 変更を保存して、ページに表示されるメッセージを確認します。
1. 開発者モードに切り替え、CRXDE-Lite でコンテンツパスを表示し、コンポーネントインスタンスのプロパティを調べます。
1. CRXDE-Lite を使用して、`/apps/wknd/components/content/helloworld` から `cq:dialog` および `helloworld.html` スクリプトを表示します。

## HTL（HTML テンプレート言語）とダイアログ {#htl-dialogs}

HTML テンプレート言語（**[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=ja)**）は、AEM コンポーネントでコンテンツをレンダリングするために使用する軽量のサーバーサイドテンプレート言語です。

**ダイアログ**&#x200B;では、コンポーネントに対して可能な設定を定義します。

次に、テキストメッセージの前に追加の挨拶を表示するように `HelloWorld` HTL スクリプトを更新します。

>[!VIDEO](https://video.tv.adobe.com/v/330987?quality=12&learn=on)

上記のビデオで実行している大まかな手順を以下に示します。

1. IDE に切り替え、`ui.apps` モジュールにプロジェクトを開きます。
1. `helloworld.html` ファイルを開き、HTML マークアップを更新します。
1. [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) などの IDE ツールを使用して、ファイルの変更内容をローカルの AEM インスタンスと同期します。
1. ブラウザーに戻り、コンポーネントのレンダリングが変更されたことを確認します。
1. `HelloWorld` コンポーネントのダイアログを定義する `.content.xml` ファイル（以下に示す場所にある）を開きます。

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. ダイアログを更新して、**Title** という名前のテキストフィールドを `./title` という名前で追加します。

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

1. 下記パスの `HelloWorld` コンポーネントをレンダリングするメイン HTL スクリプトを表すファイル `helloworld.html` を再度開きます。

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. 「**Greeting**」テキストフィールドの値を `H1` タグの一部としてレンダリングするように、`helloworld.html` を更新します。

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. 開発者向けプラグインを使用するか Maven スキルを使用して、AEM のローカルインスタンスに変更内容をデプロイします。

## Sling モデル {#sling-models}

Sling モデルは、JCR から Java™ 変数へのデータのマッピングを容易にする注釈主導の Java™「POJO」（Plain Old Java™ Objects）です。また、AEM のコンテキストで開発する際には、他の詳細情報もいくつか提供します。

次に、JCR に格納された値にビジネスロジックを適用してから値をページに出力するため、`HelloWorldModel` Sling モデルを少し更新します。

>[!VIDEO](https://video.tv.adobe.com/v/330988?quality=12&learn=on)

1. ファイル `HelloWorldModel.java`（`HelloWorld` コンポーネントで使用される Sling モデル）を開きます。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 次の import 文を追加します。

   ```java
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. `DefaultInjectionStrategy` を使用するように `@Model` 注釈を更新します。

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. 以下の行を `HelloWorldModel` クラスに追加して、コンポーネントの JCR プロパティ `title` および `text` の値を Java™ 変数にマッピングします。

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

1. 以下のメソッド `getTitle()` を `HelloWorldModel` クラスに追加します。このメソッドは `title` という名前のプロパティの値を返します。このメソッドは、「Default Value here!」という String 値を返すロジックを追加します。 プロパティが `title` が null または空白の場合：

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. 以下のメソッド `getText()` を `HelloWorldModel` クラスに追加します。このメソッドは `text` という名前のプロパティの値を返します。このメソッドは、文字列をすべて大文字に変換します。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. `core` モジュールからバンドルをビルドしてデプロイします。

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > AEM 6.4／6.5 の場合は、`mvn clean install -PautoInstallBundle -Pclassic` を使用します。

1. `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` のファイル `helloworld.html` を更新して、`HelloWorld` モデルの新しく作成したメソッドを使用するようにします。

   `HelloWorld` モデルは、このコンポーネントインスタンスに対して HTL ディレクティブ `data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel"` を介してインスタンス化され、インスタンスを変数 `model` に保存します。

   `HelloWorld` モデルインスタンスは、`HelloWord` を使用して `model` 変数を介して HTL で使用できるようになりました。これらのメソッドの呼び出しでは、短縮メソッドの構文を使用できます。例えば、`${model.getTitle()}` は `${model.title}` に短縮できます。

   同様に、すべての HTL スクリプトには、Sling Model オブジェクトと同じ構文を使用してアクセスできる [グローバルオブジェクト](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html?lang=ja) が挿入されます。

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
   </div>
   ```

1. Eclipse Developer プラグインまたは Maven スキルを使用して、AEM のローカル インスタンスに変更内容をデプロイします。

## クライアントサイドライブラリ {#client-side-libraries}

クライアントサイドライブラリ（略して `clientlibs`）は、 AEM Sites 実装に必要な CSS および JavaScript ファイルを編成および管理するメカニズムを提供します。クライアントサイドライブラリは、AEM のページに CSS と JavaScript を組み込む標準的な方法です。

[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ja) モジュールは、ビルドプロセスに統合されている独立した [webpack](https://webpack.js.org/) プロジェクトです。これにより、Sass、LESS、TypeScript などの一般的なフロントエンドライブラリを使用できます。 `ui.frontend` モジュールについては、[クライアントサイドライブラリの章](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md)で詳しく説明しています。

次に、 `HelloWorld` コンポーネントの CSS スタイルを更新します。

>[!VIDEO](https://video.tv.adobe.com/v/340750?quality=12&learn=on)

上記のビデオで実行している大まかな手順を以下に示します。

1. 新しいターミナルウィンドウを開き、`ui.frontend` ディレクトリに移動します。

1. `ui.frontend` ディレクトリで、`npm install npm-run-all --save-dev` コマンドを実行して [npm-run-all](https://www.npmjs.com/package/npm-run-all) ノードモジュールをインストールします。この手順は、**アーキタイプ 39 で生成された AEM プロジェクト**&#x200B;では必要ですが、今後のアーキタイプバージョンでは必要ありません。

1. 次に、 `npm run watch` コマンドを実行します。

   ```shell
   $ npm run watch
   ```

1. IDE に切り替え、`ui.frontend` モジュールにプロジェクトを開きます。
1. `ui.frontend/src/main/webpack/components/_helloworld.scss` ファイルを開きます。
1. 赤いタイトルを表示するように、ファイルを更新します。

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. `ui.frontend` モジュールがコンパイルされて変更内容を AEM のローカルインスタンスと同期中であることを示すアクティビティがターミナルに表示されます。

   ```shell
   Entrypoint site 214 KiB = clientlib-site/site.css 8.45 KiB clientlib-site/site.js 206 KiB
   2022-02-22 17:28:51: webpack 5.69.1 compiled successfully in 119 ms
   change:dist/index.html
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   ```

1. ブラウザーに戻り、タイトルの色が変更されたことを確認します。

   ![更新後の Component Basics](assets/component-basics/color-update.png)

## おめでとうございます。 {#congratulations}

おめでとうございます。これで、Adobe Experience Managerでのコンポーネント開発の基本を学びました。

### 次の手順 {#next-steps}

次の章[ページとテンプレート](pages-templates.md)で、Adobe Experience Manager のページとテンプレートについて理解します。コアコンポーネントがプロジェクト内にどのようにプロキシ化されるかを理解し、適切に構造化された Article Page テンプレートを構築するための編集可能なテンプレートの高度なポリシー設定について学びます。

完成したコードを [GitHub](https://github.com/adobe/aem-guides-wknd) で確認するか、コードをレビューして Git ブランチ `tutorial/component-basics-solution` でローカルにデプロイします。
