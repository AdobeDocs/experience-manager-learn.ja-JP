---
title: AEM SPAエディタを使用した開発 — Hello Worldチュートリアル
description: AEM SPAエディタは、単一ページアプリケーションまたはSPAのコンテキスト内編集をサポートします。 このチュートリアルでは、AEM SPAエディターJS SDKで使用するSPA開発の概要を説明します。 このチュートリアルでは、カスタムHello Worldコンポーネントを追加してWe.Retailジャーナルアプリを拡張します。 ユーザは、ReactフレームワークまたはAngularフレームワークを使用して、このチュートリアルを完了できます。
sub-product: サイト、content services
feature: spa-editor
topics: development, single-page-applications
audience: developer
doc-type: tutorial
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '3181'
ht-degree: 3%

---


# AEM SPAエディタを使用した開発 — Hello Worldチュートリアル {#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> このチュートリアルは **廃止されました**。 次のいずれかを行うことをお勧めします。 [AEM SPAエディタの使用の手引きとAEM SPAエディタの使用の手引き](https://docs.adobe.com/content/help/en/experience-manager-learn/spa-angular-tutorial/overview.html) (Angular [)または使用の手引き(Getting Started with the  SPA Editor)とReact](https://docs.adobe.com/content/help/en/experience-manager-learn/spa-react-tutorial/overview.html)

AEM SPAエディタは、単一ページアプリケーションまたはSPAのコンテキスト内編集をサポートします。 このチュートリアルでは、AEM SPAエディターJS SDKで使用するSPA開発の概要を説明します。 このチュートリアルでは、カスタムHello Worldコンポーネントを追加してWe.Retailジャーナルアプリを拡張します。 ユーザは、ReactフレームワークまたはAngularフレームワークを使用して、このチュートリアルを完了できます。

>[!NOTE]
>
> シングルページアプリケーション(SPA)エディタ機能を使用するには、AEM 6.4 Service Pack 2以降が必要です。
>
> SPAエディターは、SPAフレームワークベースのクライアント側レンダリング（ReactやAngularなど）を必要とするプロジェクトに推奨されるソリューションです。

## 前提条件の読み取り {#prereq}

このチュートリアルでは、SPAコンポーネントをAEMコンポーネントにマップし、コンテキスト内編集を有効にするために必要な手順を説明します。 このチュートリアルを開始するユーザは、AngularフレームワークのReactを使用した開発と、Adobe Experience Manager、AEMの開発に関する基本的な概念を理解している必要があります。 このチュートリアルでは、バックエンドおよびフロントエンドの開発タスクについて説明します。

このチュートリアルを開始する前に、次のリソースを確認することをお勧めします。

* [SPA Editor機能ビデオ](spa-editor-framework-feature-video-use.md) - SPA EditorおよびWe.Retailジャーナルアプリの概要を説明するビデオ。
* [React.jsチュートリアル](https://reactjs.org/tutorial/tutorial.html) - Reactフレームワークを使用した開発の紹介です。
* [角度チュートリアル](https://angular.io/tutorial) - Angularを使用した開発の概要

## ローカル開発環境 {#local-dev}

このチュートリアルは、次の目的で開発されています。

[Adobe Experience Manager6.5](https://helpx.adobe.com/jp/experience-manager/6-5/release-notes.html) または [Adobe Experience Manager6.4](https://helpx.adobe.com/jp/experience-manager/6-4/sites/deploying/using/technical-requirements.html) + [Service Pack 5](https://helpx.adobe.com/jp/experience-manager/6-4/release-notes/sp-release-notes.html)

このチュートリアルでは、次のテクノロジーとツールをインストールする必要があります。

1. [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
2. [Apache Maven - 3.3.1以降](https://maven.apache.org/)
3. [Node.js - 8.11.1+](https://nodejs.org/en/) およびnpm 5.6.0+（npmはnode.jsと共にインストールされます）

重複は、新しいターミナルを開いて次のコマンドを実行し、上記のツールのインストールを確認します。

```shell
$ java -version
java version "11 +"

$ mvn -version
Apache Maven 3.3.9

$ node --version
v8.11.1

$ npm --version
6.1.0
```

## 概要 {#overview}

基本的な概念は、SPAコンポーネントをAEMコンポーネントにマップすることです。 AEMコンポーネントは、サーバーサイドで実行し、JSON形式でコンテンツを書き出します。 JSONコンテンツは、ブラウザーでクライアント側を実行するSPAで使用されます。 SPAコンポーネントとAEMコンポーネント間の1:1マッピングが作成されます。

![SPAコンポーネントマッピング](assets/spa-editor-helloworld-tutorial-use/mapto.png)

一般的なフレームワーク [React JS](https://reactjs.org/) 、 [Angular](https://angular.io/) は初期状態でサポートされています。 ユーザは、最も使いやすいフレームワークのAngularまたはReactで、このチュートリアルを完了できます。

## プロジェクトのセットアップ {#project-setup}

SPA開発はAEM開発では1フィート、SPA開発では1フィートです。 目標は、SPAの開発を独立して行い、AEMにとって（ほとんど）不可知な状態で行うことです。

* SPAプロジェクトは、フロントエンド開発時にAEMプロジェクトとは独立して動作します。
* Webpack、NPMなどのフロントエンドビルドツールおよびテクノロジ [!DNL Grunt] ーを使用し、 [!DNL Gulp]引き続き使用できます。
* AEM用にビルドするには、SPAプロジェクトをコンパイルし、AEMプロジェクトに自動的に含めます。
* SPAをAEMに展開するために使用する標準AEMパッケージ。

![アーティファクトとデプロイメントの概要](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*SPA開発はAEM開発の1つの段階を占め、もう1つの段階を占め、SPA開発は独立して行われ、AEMは（ほとんどが）独立して行われます。*

このチュートリアルの目標は、We.Retailジャーナルアプリを新しいコンポーネントで拡張することです。 Web.Retailジャーナルアプリのソースコードをダウンロードし、ローカルAEMに展開することで開始します。

1. **最新の** We.RetailジャーナルコードをGitHubからダウンロードします [](https://github.com/adobe/aem-sample-we-retail-journal)。

   または、次のコマンド・ラインからリポジトリをコピーします。

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >このチュートリアルでは、 **1.2.1-SNAPSHOT** versionのプロジェクトを持つ **** マスターブランチに対して作業を行います。

1. 次の構造が表示されます。

   ![プロジェクトフォルダ構造](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   プロジェクトには、次のMavenモジュールが含まれています。

   * `all`:プロジェクト全体を1つのパッケージに埋め込んでインストールします。
   * `bundles`:2つのOSGiバンドルが含まれます。コモンとコア。この中には、およ [!DNL Sling Models] びその他のJavaコードが含まれています。
   * `ui.apps`:には、プロジェクトの/apps部分、ie JSおよびCSSクライアントライブラリ、コンポーネント、runmode固有の設定が含まれます。
   * `ui.content`:構造コンテンツと構成を含む(`/content`、 `/conf`)
   * `react-app`:We.小売ジャーナルの反応申し込み。 これはMavenモジュールとWebPackプロジェクトの両方です。
   * `angular-app`:We.小売ジャーナルの角度適用 これは、 [!DNL Maven] モジュールとWebPackの両方のプロジェクトです。

1. 新しいターミナルウィンドウを開き、次のコマンドを実行して、http://localhost:4502で実行されているローカルのAEMインスタンスにアプリケーション全体を構築してデプロイし [ます](http://localhost:4502)。

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > このプロジェクトでは、プロジェクト全体を構築しパッケージ化するMavenプロファイルが `autoInstallSinglePackage`

   >[!CAUTION]
   >
   > ビルド中にエラーが発生した場合は、Maven settings.xmlファイルにAdobeのMavenアーティファクトリポジトリが含まれていることを [確認してください](https://helpx.adobe.com/jp/experience-manager/kb/SetUpTheAdobeMavenRepository.html)。

1. 次の URL に移動します。

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   We.RetailジャーナルアプリがAEM Sitesエディター内に表示されます。

1. 編集 [!UICONTROL モードで] 、編集するコンポーネントを選択し、コンテンツを更新します。

   ![コンポーネントの編集](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

1. 「 [!UICONTROL ページプロパティ] 」アイコンを選択して、「 [!UICONTROL ページプロパティ]」を開きます。 「テンプレート [!UICONTROL の編集] 」を選択して、ページのテンプレートを開きます。

   ![ページプロパティメニュー](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

1. 最新バージョンのSPA Editorでは、 [編集可能なテンプレート](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/page-templates-editable.html) は、従来のサイト実装と同じ方法で使用できます。 これは、後でカスタムコンポーネントで再訪問します。

   >[!NOTE]
   >
   > 編集可能なテンプレートは、AEM 6.5およびAEM 6.4 + **Service Pack 5** でのみサポートされます。

## 開発の概要 {#development-overview}

![概要開発](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

SPA開発反復は、AEMとは独立して発生します。 SPAをAEMに導入する準備ができたら、次の高レベルの手順を実行します（上図を参照）。

1. AEMプロジェクトビルドが呼び出され、SPAプロジェクトのビルドがトリガされます。 We.Retailジャーナルは、 [**フロントエンドMaven-pluginを使用します**](https://github.com/eirslett/frontend-maven-plugin)。
1. SPAプロジェクトの [**aem-clientlib-generator**](https://www.npmjs.com/package/aem-clientlib-generator) は、コンパイル済みのSPAをAEMクライアントライブラリとしてAEMプロジェクトに埋め込みます。
1. AEMプロジェクトは、コンパイル済みのSPAと他のサポートするAEMコードを含むAEMパッケージを生成します。

## AEMコンポーネントを作成 {#aem-component}

**担当者：AEM 開発者**

最初にAEMコンポーネントが作成されます。 AEMコンポーネントは、Reactコンポーネントによって読み取られるJSONプロパティのレンダリングを担当します。 AEMコンポーネントは、コンポーネントの編集可能なプロパティに関するダイアログを提供する役割も果たします。

We.RetailジャーナルMavenプロジェクト [!DNL Eclipse]を使用する、またはその他 [!DNL IDE]の方法でインポートします。

1. reactor **pom.xmlを更新し、プラグインを削除し**[!DNL Apache Rat] ます。 このプラグインは、各ファイルをチェックして、ライセンスヘッダーがあることを確認します。 当社の目的上、この機能に関心を持つ必要はありません。

   aem-sample-we-retail-journal/pom.xml **で、** apache **-rate-plugin**：を削除します。

   ```xml
   <!-- Remove apache-rat-plugin -->
   <plugin>
           <groupId>org.apache.rat</groupId>
           <artifactId>apache-rat-plugin</artifactId>
           <configuration>
               <excludes combine.children="append">
                   <exclude>*</exclude>
                       ...
               </excludes>
           </configuration>
           <executions>
                   <execution>
                       <phase>verify</phase>
                       <goals>
                           <goal>check</goal>
                       </goals>
               </execution>
           </executions>
       </plugin>
   ```

1. **we-retail-** ジャーナルコンテンツ`<src>/aem-sample-we-retail-journal/ui.apps`( `ui.apps/jcr_root/apps/we-retail-journal/components` )モジュールでは、 **cq:Component****** cqのタイプのhelloworldという名前の新しいノードを作成します。
1. 次追加のXML( **)で表される、** helloworld`/helloworld/.content.xml`コンポーネントの次のプロパティ。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:description="Hello World Component for We.Retail Journal"
       jcr:primaryType="cq:Component"
       jcr:title="Hello World"
       componentGroup="We.Retail Journal" />
   ```

   ![Hello Worldコンポーネント](assets/spa-editor-helloworld-tutorial-use/hello-world-component.png)

   >[!NOTE]
   >
   > 編集可能なテンプレート機能を理解するために、を意図的に設定し `componentGroup="Custom Components"`ました。 実際のプロジェクトでは、コンポーネントグループの数を最小限に抑えることが最善の方法なので、他のコンテンツコンポーネントに合わせるには、「[!DNL We.Retail Journal]」が適切なグループにします。
   >
   > 編集可能なテンプレートは、AEM 6.5およびAEM 6.4 + **Service Pack 5** でのみサポートされます。

1. 次に、 **Hello World** コンポーネント用にカスタムメッセージを設定できるダイアログが作成されます。 ノード名 `/apps/we-retail-journal/components/helloworld` cq:dialog **of** nt:unstructured ****&#x200B;を追加します。
1. **cq:dialog** は、という名前のプロパティにテキストを保持する1つのテキストフィールドを表示 **[!DNL message]**&#x200B;します。 新しく作成した **cq:dialogの下に** 、以下のXMLで示すノードとプロパティを追加します(`helloworld/_cq_dialog/.content.xml`)。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="We.Retail Journal - Hello World"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/tabs"
                   maximized="{Boolean}true">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Properties"
                           sling:resourceType="granite/ui/components/coral/foundation/container"
                           margin="{Boolean}true">
                           <items jcr:primaryType="nt:unstructured">
                               <columns
                                   jcr:primaryType="nt:unstructured"
                                   sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                   margin="{Boolean}true">
                                   <items jcr:primaryType="nt:unstructured">
                                       <column
                                           jcr:primaryType="nt:unstructured"
                                           sling:resourceType="granite/ui/components/coral/foundation/container">
                                           <items jcr:primaryType="nt:unstructured">
                                               <message
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldLabel="Message"
                                                   name="./message"
                                                   required="{Boolean}true"/>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </properties>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   ![ファイル構造](assets/spa-editor-helloworld-tutorial-use/updated-with-dialog.png)

   上記のXMLノード定義は、1つのテキストフィールドを持つダイアログを作成し、ユーザーが「メッセージ」を入力できるようにします。 ノード `name="./message"` 内のプロパティをメモしておき `<message />` ます。 AEM内のJCRに保存されるプロパティの名前です。

1. 次に、空のポリシーダイアログが作成されます(`cq:design_dialog`)。 テンプレートエディターでコンポーネントを表示するには、ポリシーダイアログが必要です。 この単純な使用例では、空のダイアログになります。

   の下にノード名を `/apps/we-retail-journal/components/helloworld` 追加 `cq:design_dialog` し `nt:unstructured`ます。

   設定は次のXMLで表されます(`helloworld/_cq_design_dialog/.content.xml`)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
   jcr:primaryType="nt:unstructured" />
   ```

1. コマンドラインからAEMにコードベースをデプロイします。

   ```shell
   $ cd <src>/aem-sample-we-retail-journal/content
   $ mvn -PautoInstallPackage clean install
   ```

   「 [CRXDE Lite中](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld) 」で、「 `/apps/we-retail-journal/components:`

   ![CRXDE Liteにデプロイされたコンポーネント構造](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## Slingモデルを作成 {#create-sling-model}

**担当者：AEM 開発者**

次に、コンポー [!DNL Sling Model][!DNL Hello World] ネントをバックするためのが作成されます。 従来のWCMの使用例では、がビジネスロジックを [!DNL Sling Model][!DNL Sling Model]実装し、サーバー側レンダリングスクリプト(HTL)が これにより、レンダリングスクリプトが比較的単純になります。

[!DNL Sling Models] は、サーバー側のビジネスロジックを実装する際に、SPAの使用例でも使用されます。 違いは、使用 [!DNL SPA] 例では、そのメソッドをシリアライズされたJSONとして [!DNL Sling Models] 公開する点です。

>[!NOTE]
>
>ベストプラクティスとして、開発者は [AEMコアコンポーネントを可能な限り使用するようにします](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html) 。 特に、コアコンポーネントは「SPA対応」 [!DNL Sling Models] のJSON出力を提供し、開発者はフロントエンドでのプレゼンテーションに集中できます。

1. In the editor of your choice, open the **we-retail-journal-commons** project ( `<src>/aem-sample-we-retail-journal/bundles/commons`).
1. パッケージ内 `com.adobe.cq.sample.spa.commons.impl.models`:
   * という名前の新しいクラスを作成し `HelloWorld`ます。
   * ～追加の実装インターフェース `com.adobe.cq.export.json.ComponentExporter.`

   ![新規Javaクラスウィザード](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   AEM Content Servicesと互換性を持たせるには、 `ComponentExporter` インターフェイス [!DNL Sling Model] を実装する必要があります。

   ```java
    package com.adobe.cq.sample.spa.commons.impl.models;
   
    import com.adobe.cq.export.json.ComponentExporter;
   
    public class HelloWorld implements ComponentExporter {
   
        @Override
        public String getExportedType() {
            return null;
        }
    }
   ```

1. コンポー追加ネントのリソースタイプ `RESOURCE_TYPE`[!DNL HelloWorld] を識別するために名前を付けた静的変数：

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

1. と追加のOSGi注釈を参照し `@Model` てくだ `@Exporter`さい。 この `@Model` 注釈はクラスをに登録し [!DNL Sling Model]ます。 注 `@Exporter` 釈は、フレームワークを使用して、シリアライズされたJSONとしてメソッドを公開し [!DNL Jackson Exporter] ます。

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Exporter;
   import org.apache.sling.models.annotations.Model;
   import com.adobe.cq.export.json.ExporterConstants;
   ...
   
   @Model(
           adaptables = SlingHttpServletRequest.class,
           adapters = {ComponentExporter.class},
           resourceType = HelloWorld.RESOURCE_TYPE
   )
   @Exporter(
           name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
           extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class HelloWorld implements ComponentExporter {
   
   ...
   ```

1. JCRプロパティ `getDisplayMessage()` を返すメソッドを実装 `message`します。 の [!DNL Sling Model] 注釈を使用す `@ValueMapValue` ると、コンポーネントの下に `message` 保存されたプロパティを簡単に取得できます。 この `@Optional` 注釈は、コンポーネントが最初にページに追加される際には入力されないので、重要 `message` です。

   ビジネスロジックの一部として、メッセージの前に「**Hello**」という文字列が付加されます。

   ```java
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.Optional;
   
   ...
   
   public class HelloWorld implements ComponentExporter {
   
      static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
      private static final String PREPEND_MSG = "Hello";
   
       @ValueMapValue @Optional
       private String message;
   
       public String getDisplayMessage() {
           if(message != null && message.length() > 0) {
               return PREPEND_MSG + " "  + message;
           }
           return null;
       }
   
   ...
   ```

   >[!NOTE]
   >
   > メソッド名 `getDisplayMessage` は重要です。 は、を使用してシリアライズ [!DNL Sling Model] する [!DNL Jackson Exporter] と、JSONプロパティとして公開されます。 `displayMessage`. は、(無視 [!DNL Jackson Exporter] するように明示的にマークされていない限り)パラメーターを受け取らないすべての `getter` メソッドをシリアル化して公開します。 後のReact / Angularアプリで、このプロパティ値を読み取り、アプリケーションの一部として表示します。

   この方法 `getExportedType` も重要です。 コンポーネントの値 `resourceType` は、JSONデータをフロントエンドコンポーネントに「マップ」するために使用されます（角度/反応）。 次のセクションで、この点について説明します。

1. コンポーネントのリソースタイプ `getExportedType()` を返すメソッドを実装し `HelloWorld` ます。

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   HelloWorld.javaの完全なコードは [**こちら** 。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

1. Apache Mavenを使用してAEMにコードをデプロイします。

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   OSGiコンソールで、 [!DNL Sling Model] Status [[!UICONTROL /] Sling Models ](http://localhost:4502/system/console/status-slingmodels) に移動して、のデプロイメントと登録を確認します。

   SlingモデルがSlingリソースタイプにバインドされ、 `HelloWorld` Slingモデルが `we-retail-journal/components/helloworld` Slingリソースタイプに登録されていることを確認する必要があります [!DNL Sling Model Exporter Servlet]。

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## 反応コンポーネントを作成 {#react-component}

**担当者：フロントエンド開発者**

次に、Reactコンポーネントが作成されます。 任意のエディターを使用して、リ **アクトアプリ** モジュール( `<src>/aem-sample-we-retail-journal/react-app`)を開きます。

>[!NOTE]
>
> Angular開発にのみ関心がある場合は、この節を省略して [ください](#angular-component)。

1. フォルダー内で、srcフォルダーに移動し `react-app` ます。 componentsフォルダーを展開して、既存のReactコンポーネントファイルを表示します。

   ![reactコンポーネントファイル構造](assets/spa-editor-helloworld-tutorial-use/react-components.png)

1. componentsフォルダー追加の下にあるという名前の新しいファイル `HelloWorld.js`。
1. 開く `HelloWorld.js`. Reactコンポー追加ネントライブラリを読み込むための読み込みステートメント。 Adobeが提供する `MapTo` ヘルパーを読み込む2つ目の読み込みステートメント。 ヘルパーは、ReactコンポーネントとAEMコンポーネントのJSONとのマッピングを提供します。 `MapTo`

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

1. インポートの下に、Reactインターフェイスを拡張するという名前の新しいクラス `HelloWorld` が作成され `Component` ます。 クラス追加に必要な `render()``HelloWorld` メソッド。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

1. この `MapTo` ヘルパーには、Reactコンポーネントのpropの一部として名前が付い `cqModel` たオブジェクトが自動的に含まれます。 には、によって公開されるすべてのプロパティが `cqModel` 含まれ [!DNL Sling Model]ます。

   前に [!DNL Sling Model] 作成したファイルにはメソッドが含まれてい `getDisplayMessage()`ます。 `getDisplayMessage()` は、出力時に名前が付けられたJSONキーとして変換 `displayMessage` されます。

   の値を含む `render()` タグを出力するメソッドを実装し `h1``displayMessage`ます。 [JavaScriptの構文拡張子であるJSX](https://reactjs.org/docs/introducing-jsx.html)(JSX)は、コンポーネントの最終的なマークアップを返すために使用されます。

   ```js
   ...
   
   class HelloWorld extends Component {
       render() {
   
           if(this.props.displayMessage) {
               return (
                   <div className="cmp-helloworld">
                       <h1 className="cmp-helloworld_message">{this.props.displayMessage}</h1>
                   </div>
               );
           }
           return null;
       }
   }
   ```

1. 設定の編集メソッドを実装します。 このメソッドは `MapTo` ヘルパーを介して渡され、コンポーネントが空の場合にプレースホルダーを表示する情報をAEMエディターに提供します。 これは、コンポーネントがSPAに追加されているが、まだオーサリングされていない場合に発生します。 ク `HelloWorld` ラスの追加下に次のように記述します。

   ```js
   ...
   
   class HelloWorld extends Component {
       ...
   }
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   ```

1. ファイルの最後で、 `MapTo` ヘルパーを呼び出し、 `HelloWorld` クラスとを渡し `HelloWorldEditConfig`ます。 これにより、AEMコンポーネントのリソースタイプに基づいて、React ComponentがAEMコンポーネントにマッピングされます。 `we-retail-journal/components/helloworld`.

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   HelloWorld.jsの完成したコ [**ードは** 、こちらを参照してください。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

1. Open the file `ImportComponents.js`. はを参照してください `<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`。

   コンパ追加イル済みJavaScriptバンドル内の他のコンポーネントと共に必要な行： `HelloWorld.js`

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

1. フォルダー内に、「ファイル `components` に次の値を `HelloWorld.css` 入力」の兄弟として名前を付けた新しいファイルを作成し、コンポー `HelloWorld.js.``HelloWorld` ネントの基本的なスタイルを作成します。

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. インポート文の下で再び開 `HelloWorld.js` き、更新して次の条件を満たすようにし `HelloWorld.css`ます。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   require('./HelloWorld.css');
   
   ...
   ```

1. Apache Mavenを使用してAEMにコードをデプロイします。

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. CRXDE-Liteで [開き](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js) ま `/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js`す。 app.jsでHelloWorldのクイック検索を実行し、コンパイル済みアプリにReactコンポーネントが含まれていることを確認します。

   >[!NOTE]
   >
   > **app.js** はバンドルされたReactアプリです。 コードが人間に読み取り可能でなくなりました。 この `npm run build` コマンドは、最新のブラウザーで解釈できるコンパイル済みJavaScriptを出力する、最適化されたビルドをトリガーしました。


## 角度コンポーネントを作成 {#angular-component}

**担当者：フロントエンド開発者**

>[!NOTE]
>
> Reactの開発に関心がある場合は、このセクションを省略してかまいません。

次に、Angularコンポーネントが作成されます。 任意のエディターを使用して、 **angular-app** module(`<src>/aem-sample-we-retail-journal/angular-app`)を開きます。

1. フォルダー内で、そのフォル `angular-app``src` ダーに移動します。 componentsフォルダを展開し、既存のAngularコンポーネントファイルを表示します。

   ![Angularファイル構造](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

1. Add a new folder beneath the components folder named `helloworld`. フォルダーの下に、という名前の新しいファイルを `helloworld` 追加 `helloworld.component.css, helloworld.component.html, helloworld.component.ts`します。

   ```plain
   /angular-app
       /src
           /app
               /components
   +                /helloworld
   +                    helloworld.component.css
   +                    helloworld.component.html
   +                    helloworld.component.ts
   ```

1. 開く `helloworld.component.ts`. Angular `Component``Input` およびクラスをインポートするインポ追加ート文。 とを指して、新しいコンポーネント `styleUrls` を作成 `templateUrl` し `helloworld.component.css` ま `helloworld.component.html`す。 最後に、期待される入力値 `HelloWorldComponent` のクラスをエクスポート `displayMessage`します。

   ```js
   //helloworld.component.ts
   
   import { Component, Input } from '@angular/core';
   
   @Component({
     selector: 'app-helloworld',
     host: { 'class': 'cmp-helloworld' },
     styleUrls:['./helloworld.component.css'],
     templateUrl: './helloworld.component.html',
   })
   
   export class HelloWorldComponent {
     @Input() displayMessage: string;
   }
   ```

   >[!NOTE]
   >
   > 以前に [!DNL Sling Model] 作成したメソッドを思い出すと、 **getDisplayMessage()メソッドがありました**。 このメソッドのシリアル化されたJSONは、 **displayMessage**&#x200B;です。現在、Angularアプリで読み取っています。

1. を開き、 `helloworld.component.html` プロパティを印刷する `h1``displayMessage` タグを含めます。

   ```html
   <h1 *ngIf="displayMessage" class="cmp-helloworld_message">
       {{displayMessage}}
   </h1>
   ```

1. コンポーネント `helloworld.component.css` に基本的なスタイルを含めるように更新します。

   ```css
   :host-context {
       display: block;
   };
   
   .cmp-helloworld {
       display:block;
   }
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. 次 `helloworld.component.spec.ts` のテストベッドで更新します。

   ```js
   import { async, ComponentFixture, TestBed } from '@angular/core/testing';
   
   import { HelloWorldComponent } from './helloworld.component';
   
       describe('HelloWorld', () => {
       let component: HelloWorldComponent;
       let fixture: ComponentFixture<HelloWorldComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           declarations: [ HelloWorldComponent ]
           })
           .compileComponents();
       }));
   
       beforeEach(() => {
           fixture = TestBed.createComponent(HelloWorldComponent);
           component = fixture.componentInstance;
           fixture.detectChanges();
       });
   
       it('should create', () => {
           expect(component).toBeTruthy();
       });
   });
   ```

1. を含め `src/components/mapping.ts` るための次の更新 `HelloWorldComponent`。 コンポーネントが追加設定される前に、AEMエディタでプレースホルダーを示すマークが付きます。 `HelloWorldEditConfig` 最後に、ヘルパーを使用してAEMコンポーネントをAngularコンポーネントにマップする線を追加し `MapTo` ます。

   ```js
   // src/components/mapping.ts
   
   import { HelloWorldComponent } from "./helloworld/helloworld.component";
   
   ...
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   
   MapTo('we-retail-journal/components/helloworld')(HelloWorldComponent, HelloWorldEditConfig);
   ```

   mapping.tsの完全な [**コードは** 、こちらを参照してください。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

1. NgModule `src/app.module.ts` を更新するには、 **を更新します**。 AppModule **`HelloWorldComponent`** に属する **宣言** と追加して **宣言**&#x200B;します。 また、JSONモデルの処理 `HelloWorldComponent` 時にアプリに動的に含まれるように、を **entryComponent** として追加します。

   ```js
   import { HelloWorldComponent } from './components/helloworld/helloworld.component';
   
   ...
   
   @NgModule({
     imports: [BrowserModule.withServerTransition({ appId: 'we-retail-sample-angular' }),
       SpaAngularEditableComponentsModule,
     AngularWeatherWidgetModule.forRoot({
       key: "37375c33ca925949d7ba331e52da661a",
       name: WeatherApiName.OPEN_WEATHER_MAP,
       baseUrl: 'http://api.openweathermap.org/data/2.5'
     }),
       AppRoutingModule,
       BrowserTransferStateModule],
     providers: [ModelManagerService,
       { provide: APP_BASE_HREF, useValue: '/' }],
     declarations: [AppComponent,
       TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MenuComponent,
       MainContentComponent,
       HelloWorldComponent],
     entryComponents: [TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MainContentComponent,
       HelloWorldComponent],
     bootstrap: [AppComponent]
    })
   ```

   app.module.ts [**の完成したコードは** 、こちらを参照してください。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

1. Mavenを使用してAEMにコードをデプロイします。

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. CRXDE-Liteで [開き](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js) ま `/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`す。 でHelloWorldのクイック検索を実行し **て** 、Angularコンポーネントが含まれ `main.js` ていることを確認します。

   >[!NOTE]
   >
   > **main.js** は、バンドルされているAngularアプリです。 コードが人間に読み取り可能でなくなりました。 npm run buildコマンドは、最新のブラウザーで解釈できるコンパイル済みJavaScriptを出力する最適化ビルドをトリガーしました。

## テンプレートの更新 {#template-update}

1. ReactまたはAngularバージョンの編集可能テンプレートに移動します。

   * （角度） [http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * (React) [http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

1. メインの [!UICONTROL レイアウトコンテナを選択し] 、 [!UICONTROL ポリシー] アイコンを選択してポリシーを開きます。

   ![レイアウトポリシーの選択](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   「 **[!UICONTROL プロパティ]** / **[!UICONTROL 許可されているコンポーネント]**」で、検索を実行し **[!DNL Custom Components]**&#x200B;ます。 コンポーネントが表示されたら、 **[!DNL Hello World]** 選択します。 右上隅のチェックボックスをクリックして、変更を保存します。

   ![レイアウトコンテナポリシーの設定](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

1. 保存後は、 **[!DNL HelloWorld]** コンポーネントが許可コンポーネントとして [!UICONTROL レイアウトコンテナに表示されます]。

   ![許可されているコンポーネントが更新されました](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > AEM 6.5およびAEM 6.4.5のみが、SPAエディタの編集可能なテンプレート機能をサポートしています。 AEM 6.4を使用している場合は、次のCRXDE Liteを介して、許可されたコンポーネントのポリシーを手動で設定する必要があります。 `/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default` または `/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   レイ [!UICONTROL アウトコンテナの] 許可されたコンポーネントの更新されたポリシー設定を示すCRXDE Lite :

   ![レイアウトコンテナの許可されたコンポーネントの更新されたポリシー設定を示すCRXDE Lite](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## まとめ {#putting-together}

1. AngularページまたはReactページに移動します。

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

1. コンポーネントを見つけ **[!DNL Hello World]** て、そのコンポー **[!DNL Hello World]** ネントをページにドラッグ&amp;ドロップします。

   ![hello worldドラッグ&amp;ドロップ](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   プレースホルダーが表示されます。

   ![ハローワールドプレースホルダー](assets/spa-editor-helloworld-tutorial-use/fig10.png)

1. コンポーネントを選択し、ダイアログに「World」や「Your Name」などのメッセージを追加します。 変更内容を保存します。

   ![レンダリングコンポーネント](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   「Hello」という文字列は、常にメッセージの先頭に付加されます。 これは、のロジックの結果で `HelloWorld.java`[!DNL Sling Model]す。

## 次の手順 {#next-steps}

[HelloWorldコンポーネントの完了したソリューション](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* GitHub [[!DNL We.Retail Journal] 上の完全なソースコード](https://github.com/adobe/aem-sample-we-retail-journal)
* React with the AEM SPA Editor - WKND Tutorial]の開発に関する詳細なチュートリアルをご覧く [ださい。](https://helpx.adobe.com/jp/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html)

## トラブルシューティング {#troubleshooting}

### Eclipseにプロジェクトを構築できません {#unable-to-build-project-in-eclipse}

**エラー：** 認識されない目標実行に対して [!DNL We.Retail Journal] プロジェクトをEclipseに読み込むときにエラーが発生しました：

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![eclipseエラーウィザード](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**解像度**:後でこれらを解決するには、「完了」をクリックします。 これにより、チュートリアルが完了します。

**エラー**:Reactモジュール `react-app`は、Mavenのビルド中は正常にビルドされません。

**解像度：** リアクトアプリの下の `node_modules` フォルダーを削除してみ **てください**。 プロジェクトのルート `mvn  clean install -PautoInstallSinglePackage` からApache Mavenコマンドを再実行します。

### AEMで満たされていない依存関係 {#unsatisfied-dependencies-in-aem}

![パッケージマネージャーの依存関係エラー](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

AEMの依存関係が満たされない場合は、 **[!UICONTROL AEM Package Manager]** 、またはAEM Webコンソール **** （Felixコンソール）で、SPAエディタ機能を使用できないことを示しています。

### コンポーネントが表示されません

**エラー**:配置が正常に完了し、React/Angularアプリケーションのコンパイル済みバージョンに更新済みのコンポーネントがあることを確認した後でも、コンポーネントをページにドラッグしてもコンポーネントが表示されません。 `helloworld` コンポーネントはAEM UIで確認できます。

**解像度**:ブラウザの履歴/キャッシュをクリアしたり、新しいブラウザを開いたり、匿名モードを使用したりします。 それでも問題が発生しない場合は、ローカルのAEMインスタンスのクライアントライブラリキャッシュを無効にします。 AEMは、大きなクライアントライブラリをキャッシュして効率を高めようとします。 古いコードがキャッシュされる問題を修正するために、手動でキャッシュを無効にする必要がある場合があります。

移動先： [http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) 、「キャッシュを無効にする」をクリックします。 React/Angularページに戻り、ページを更新します。

![クライアントライブラリの再構築](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)
