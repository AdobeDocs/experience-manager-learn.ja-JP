---
title: AEM SPA Editor を使用した開発 — Hello World チュートリアル
description: AEM SPA Editor は、単一ページアプリケーションまたはSPAのコンテキスト内編集をサポートします。 このチュートリアルは、AEM SPA Editor JS SDK で使用するSPA開発の概要です。 このチュートリアルでは、カスタム Hello World コンポーネントを追加して、We.Retail ジャーナルアプリを拡張します。 ユーザーは、React またはAngularフレームワークを使用して、チュートリアルを完了できます。
version: 6.3, 6.4, 6.5
topic: SPA
feature: SPA Editor
role: Developer
level: Beginner
exl-id: e900301d-411c-4c02-8443-2a0fa56b65b5
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '3144'
ht-degree: 3%

---

# AEM SPA Editor を使用した開発 — Hello World チュートリアル {#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> このチュートリアルは、 **非推奨**. 次のいずれかを実行することをお勧めします。 [AEM SPA Editor とAngularの概要](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/angular/overview.html) または [AEM SPA Editor と React の使用の手引き](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/react/overview.html)

AEM SPA Editor は、単一ページアプリケーションまたはSPAのコンテキスト内編集をサポートします。 このチュートリアルは、AEM SPA Editor JS SDK で使用するSPA開発の概要です。 このチュートリアルでは、カスタム Hello World コンポーネントを追加して、We.Retail ジャーナルアプリを拡張します。 ユーザーは、React またはAngularフレームワークを使用して、チュートリアルを完了できます。

>[!NOTE]
>
> シングルページアプリケーション (SPA) エディター機能には、AEM 6.4 Service Pack 2 以降が必要です。
>
> SPA Editor は、SPAフレームワークベースのクライアントサイドレンダリング (React やAngularなど ) が必要なプロジェクトで推奨されるソリューションです。

## 前提条件の読み取り {#prereq}

このチュートリアルでは、SPAコンポーネントをAEMコンポーネントにマッピングして、コンテキスト内編集を有効にするために必要な手順を説明します。 このチュートリアルを開始するユーザーは、Adobe Experience Manager、AEMでの開発の基本概念と、Angularフレームワークの React を使用した開発に関する基本概念に精通している必要があります。 このチュートリアルでは、バックエンドとフロントエンドの両方の開発タスクについて説明します。

このチュートリアルを開始する前に、次のリソースを確認することをお勧めします。

* [SPA Editor 機能ビデオ](spa-editor-framework-feature-video-use.md) - SPA Editor および We.Retail ジャーナルアプリの概要ビデオです。
* [React.js チュートリアル](https://reactjs.org/tutorial/tutorial.html) - React フレームワークを使用した開発の概要です。
* [Angularチュートリアル](https://angular.io/tutorial) -Angularを使用した開発の概要

## ローカル開発環境 {#local-dev}

このチュートリアルは、次の用途で使用します。

[Adobe Experience Manager 6.5](https://helpx.adobe.com/jp/experience-manager/6-5/release-notes.html) または [Adobe Experience Manager 6.4](https://helpx.adobe.com/jp/experience-manager/6-4/sites/deploying/using/technical-requirements.html) + [サービスパック 5](https://helpx.adobe.com/jp/experience-manager/6-4/release-notes/sp-release-notes.html)

このチュートリアルでは、次のテクノロジーおよびツールをインストールする必要があります。

1. [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
2. [Apache Maven - 3.3.1 以降](https://maven.apache.org/)
3. [Node.js - 8.11.1+](https://nodejs.org/ja/) および npm 5.6.0 以降（npm は node.js と共にインストールされます）

新しいターミナルを開き、次のコマンドを実行して、上記のツールのインストールを再確認します。

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

基本的な概念は、SPAコンポーネントをAEMコンポーネントにマッピングすることです。 AEMコンポーネントは、サーバー側で実行され、JSON 形式でコンテンツを書き出します。 JSON コンテンツは、ブラウザーでクライアント側を実行しているSPAによって使用されます。 SPAコンポーネントとAEMコンポーネントの間に 1 対 1 のマッピングが作成されます。

![SPAコンポーネントのマッピング](assets/spa-editor-helloworld-tutorial-use/mapto.png)

人気のあるフレームワーク [React JS](https://reactjs.org/) および [Angular](https://angular.io/) は、標準でサポートされています。 ユーザーは、最も快適に使用できるAngularか React のどちらのフレームワークでも、このチュートリアルを完了できます。

## プロジェクトのセットアップ {#project-setup}

SPAの開発はAEMの開発に一歩入っていて、もう一つは外に出ています。 目標は、SPAの開発を独立して（ほとんど）AEMに依存しないようにすることです。

* SPAプロジェクトは、フロントエンド開発時に、AEMプロジェクトとは独立して動作します。
* Webpack、NPM、 [!DNL Grunt] および [!DNL Gulp]引き続き使用されます。
* AEM向けにビルドするには、SPAプロジェクトがコンパイルされ、AEMプロジェクトに自動的に含まれます。
* SPAをAEMにデプロイするために使用される標準AEMパッケージ。

![アーティファクトとデプロイメントの概要](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*SPAの開発にはAEMの開発が 1 つあり、もう 1 つは単独でSPAの開発を行うことができ、（ほとんどは）AEMに依存しないのです。*

このチュートリアルの目的は、We.Retail ジャーナルアプリを新しいコンポーネントで拡張することです。 まず、We.Retail ジャーナルアプリのソースコードをダウンロードし、ローカルのAEMにデプロイします。

1. **ダウンロード** 最新 [GitHub の We.Retail ジャーナルコード](https://github.com/adobe/aem-sample-we-retail-journal).

   または、コマンドラインからリポジトリを複製します。

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >このチュートリアルは、 **プライマリ** ～と分岐する **1.2.1-SNAPSHOT** プロジェクトのバージョン。

1. 次の構造が表示されます。

   ![プロジェクトフォルダー構造](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   プロジェクトには、次の Maven モジュールが含まれています。

   * `all`:プロジェクト全体を 1 つのパッケージに埋め込んでインストールします。
   * `bundles`:次の 2 つの OSGi バンドルが含まれます。以下を含むコモンとコア [!DNL Sling Models] およびその他の Java コード。
   * `ui.apps`:には、プロジェクトの/apps 部分（JS および CSS の clientlib、コンポーネント、実行モード固有の設定）が含まれます。
   * `ui.content`:構造コンテンツと設定 (`/content`, `/conf`)
   * `react-app`:We.Retail ジャーナル React アプリケーション。 これは、Maven モジュールと Webpack プロジェクトの両方です。
   * `angular-app`:We.Retail ジャーナルAngularアプリケーション。 これは両方とも [!DNL Maven] モジュールと webpack プロジェクト。

1. 新しいターミナルウィンドウを開き、次のコマンドを実行して、で実行しているローカルAEMインスタンスにアプリ全体をビルドおよびデプロイします。 [http://localhost:4502](http://localhost:4502).

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > このプロジェクトでは、プロジェクト全体をビルドおよびパッケージ化する Maven プロファイルが `autoInstallSinglePackage`

1. 次の URL に移動します。

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   We.Retail ジャーナルアプリがAEM Sitesエディター内に表示されます。

1. In [!UICONTROL 編集] モードで、編集するコンポーネントを選択し、コンテンツを更新します。

   ![コンポーネントの編集](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

1. を選択します。 [!UICONTROL ページプロパティ] アイコンをクリックして開きます。 [!UICONTROL ページプロパティ]. 選択 [!UICONTROL テンプレートを編集] をクリックして、ページのテンプレートを開きます。

   ![ページプロパティメニュー](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

1. 最新バージョンのSPA Editor で、 [編集可能なテンプレート](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/page-templates-editable.html) は、従来の Sites 実装と同じ方法で使用できます。 これは後でカスタムコンポーネントで再度表示されます。

   >[!NOTE]
   >
   > AEM 6.5 およびAEM 6.4 以降のみ **サービスパック 5** は、編集可能なテンプレートをサポートします。

## 開発の概要 {#development-overview}

![概要の開発](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

SPAの開発の繰り返しは、AEMとは独立しておこなわれます。 SPAをAEMにデプロイする準備が整ったら、次の大まかな手順を実行します（上図を参照）。

1. AEMプロジェクトビルドが呼び出され、SPAプロジェクトのビルドがトリガーされます。 We.Retail ジャーナルは、 [**frontend-maven-plugin**](https://github.com/eirslett/frontend-maven-plugin).
1. SPAプロジェクトの [**aem-clientlib-generator**](https://www.npmjs.com/package/aem-clientlib-generator) コンパイル済みのSPAをAEMクライアントライブラリとしてAEMプロジェクトに埋め込みます。
1. AEMプロジェクトは、コンパイル済みのSPAと、その他のサポートするAEMコードを含むAEMパッケージを生成します。

## AEMコンポーネントを作成 {#aem-component}

**担当者：AEM 開発者**

最初にAEMコンポーネントが作成されます。 AEMコンポーネントは、React コンポーネントによって読み取られる JSON プロパティのレンダリングを担当します。 また、AEMコンポーネントは、コンポーネントの編集可能なプロパティに対するダイアログを提供します。

使用 [!DNL Eclipse]またはその他 [!DNL IDE]、 We.Retail ジャーナル Maven プロジェクトをインポートします。

1. リアクタを更新する **pom.xml** 除く [!DNL Apache Rat] プラグイン。 このプラグインは、各ファイルをチェックして、License ヘッダーがあることを確認します。 当社の目的では、この機能に関心を持つ必要はありません。

   In **aem-sample-we-retail-journal/pom.xml** 削除 **apache-rate-plugin**:

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

1. 内 **we-retail-journal-content** (`<src>/aem-sample-we-retail-journal/ui.apps`) モジュールは、以下に新しいノードを作成します。 `ui.apps/jcr_root/apps/we-retail-journal/components` 名前付き **helloworld** タイプ **cq:Component**.
1. 次のプロパティを **helloworld** XML (`/helloworld/.content.xml`) を以下に示します。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:description="Hello World Component for We.Retail Journal"
       jcr:primaryType="cq:Component"
       jcr:title="Hello World"
       componentGroup="We.Retail Journal" />
   ```

   ![Hello World コンポーネント](assets/spa-editor-helloworld-tutorial-use/hello-world-component.png)

   >[!NOTE]
   >
   > 編集可能テンプレート機能を説明するために、意図的に `componentGroup="Custom Components"`. 実際のプロジェクトでは、コンポーネントグループの数を最小限に抑えることが最善なので、より良いグループは「[!DNL We.Retail Journal]」と呼ばれ、他のコンテンツコンポーネントに一致させます。
   >
   > AEM 6.5 およびAEM 6.4 以降のみ **サービスパック 5** は、編集可能なテンプレートをサポートします。

1. 次に、カスタムメッセージを **Hello World** コンポーネント。 の下 `/apps/we-retail-journal/components/helloworld` ノード名を追加 **cq:dialog** / **nt:unstructured**.
1. この **cq:dialog** は、という名前のプロパティに対するテキストを保持する単一のテキストフィールドを表示します。 **[!DNL message]**. 新しく作成された **cq:dialog** 以下の XML で表される次のノードおよびプロパティを追加します (`helloworld/_cq_dialog/.content.xml`) :

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

   上記の XML ノード定義では、1 つのテキストフィールドを持つダイアログが作成され、ユーザーが「メッセージ」を入力できるようになります。 プロパティに注意 `name="./message"` 内 `<message />` ノード。 これは、AEM内の JCR に保存されるプロパティの名前です。

1. 次に、空のポリシーダイアログが作成されます (`cq:design_dialog`) をクリックします。 テンプレートエディターでコンポーネントを表示するには、ポリシーダイアログが必要です。 この単純な使用例では、空のダイアログになります。

   の下 `/apps/we-retail-journal/components/helloworld` ノード名を追加 `cq:design_dialog` / `nt:unstructured`.

   設定は、以下の XML で表されます (`helloworld/_cq_design_dialog/.content.xml`)

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

   In [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld) コンポーネントがデプロイされたことを検証するには、以下のフォルダーを調べます。 `/apps/we-retail-journal/components:`

   ![デプロイされたコンポーネント構造をCRXDE Lite](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## Sling モデルを作成 {#create-sling-model}

**担当者：AEM 開発者**

次の a [!DNL Sling Model] が [!DNL Hello World] コンポーネント。 従来の WCM の使用例では、 [!DNL Sling Model] がビジネスロジックを実装し、サーバーサイドレンダリングスクリプト (HTL) が [!DNL Sling Model]. これにより、レンダリングスクリプトが比較的シンプルになります。

[!DNL Sling Models] は、サーバー側のビジネスロジックを実装するためにSPAのユースケースでも使用されます。 違いは [!DNL SPA] 使用例、 [!DNL Sling Models] は、メソッドをシリアル化された JSON として公開します。

>[!NOTE]
>
>ベストプラクティスとして、開発者は次を使用するようにしてください。 [AEMコアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja) 可能な場合は。 他の機能の中でも、コアコンポーネントは [!DNL Sling Models] SPA対応の JSON 出力を使用すると、開発者はフロントエンドプレゼンテーションに専念できます。

1. 任意のエディターで、 **we-retail-journal-commons** プロジェクト ( `<src>/aem-sample-we-retail-journal/bundles/commons`) をクリックします。
1. パッケージ内 `com.adobe.cq.sample.spa.commons.impl.models`:
   * という名前の新しいクラスを作成します。 `HelloWorld`.
   * の実装インターフェイスを追加 `com.adobe.cq.export.json.ComponentExporter.`

   ![新規 Java クラスウィザード](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   この `ComponentExporter` 次の処理を行うには、インターフェイスを実装する必要があります。 [!DNL Sling Model] AEM Content Services との互換性を持たせるために使用します。

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

1. という名前の静的変数を追加します。 `RESOURCE_TYPE` 識別する [!DNL HelloWorld] コンポーネントのリソースタイプ：

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

1. 用の OSGi 注釈の追加 `@Model` および `@Exporter`. この `@Model` 注釈はクラスを [!DNL Sling Model]. この `@Exporter` 注釈は、 [!DNL Jackson Exporter] フレームワーク。

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

1. メソッドの実装 `getDisplayMessage()` JCR プロパティを返すには `message`. 以下を使用： [!DNL Sling Model] 注釈 `@ValueMapValue` プロパティを簡単に取得できるようにする `message` コンポーネントの下に保存されます。 この `@Optional` 注釈は、コンポーネントが最初にページに追加される際に重要です。  `message`  が入力されない。

   ビジネスロジックの一部として、文字列、「**こんにちは**「、の前にメッセージが追加されます。

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
   > メソッド名 `getDisplayMessage` は重要です。 次の場合に [!DNL Sling Model] は [!DNL Jackson Exporter] これは、JSON プロパティとして公開されます。 `displayMessage`. この [!DNL Jackson Exporter] は、すべてをシリアル化して公開します。 `getter` パラメーターを受け取らないメソッド（無視するように明示的にマークされている場合を除く）。 後から React /Angularアプリで、このプロパティ値を読み取り、アプリケーションの一部として表示します。

   メソッド `getExportedType` も重要です。 コンポーネントの値 `resourceType` は、JSON データをフロントエンドコンポーネント (Angular/ React) に「マッピング」するために使用されます。 次の節でこれを調べます。

1. メソッドの実装 `getExportedType()` のリソースタイプを返すには `HelloWorld` コンポーネント。

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   のフルコード [**HelloWorld.java** はここにあります。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

1. Apache Maven を使用してAEMにコードをデプロイします。

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   のデプロイメントと登録を検証します。 [!DNL Sling Model] 移動して [[!UICONTROL ステータス] > [!UICONTROL Sling モデル]](http://localhost:4502/system/console/status-slingmodels) OSGi コンソールの

   次のように表示されます。 `HelloWorld` Sling モデルが `we-retail-journal/components/helloworld` Sling リソースタイプで、 [!DNL Sling Model Exporter Servlet]:

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## React コンポーネントを作成 {#react-component}

**担当者：フロントエンド開発者**

次に、React コンポーネントが作成されます。 を開きます。 **react-app** モジュール ( `<src>/aem-sample-we-retail-journal/react-app`) を使用して、任意のエディターを使用します。

>[!NOTE]
>
> 関心がある場合のみ、このセクションをスキップしてください [Angular開発](#angular-component).

1. 内部 `react-app` フォルダーは、その src フォルダーに移動します。 components フォルダーを展開して、既存の React コンポーネントファイルを表示します。

   ![React コンポーネントのファイル構造](assets/spa-editor-helloworld-tutorial-use/react-components.png)

1. components フォルダーの下に、という名前の新しいファイルを追加します。 `HelloWorld.js`.
1. 次を開きます： `HelloWorld.js`. React コンポーネントライブラリをインポートする import 文を追加します。 2 つ目の import 文を追加して、 `MapTo` ヘルパーはAdobeから提供されます。 この `MapTo` helper は、React コンポーネントの JSON へのマッピングをAEMコンポーネントに提供します。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

1. インポートの下に、という名前の新しいクラスを作成します。 `HelloWorld` React を拡張する `Component` インターフェイス。 必要な `render()` メソッドから `HelloWorld` クラス。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

1. この `MapTo` ヘルパーは、 `cqModel` を React コンポーネントの prop の一部として追加します。 この `cqModel` には、 [!DNL Sling Model].

   を記憶する [!DNL Sling Model] 作成済みにはメソッドが含まれています `getDisplayMessage()`. `getDisplayMessage()` は、 `displayMessage` 出力時

   の実装 `render()` 出力する方法 `h1` の値を含むタグ `displayMessage`. [JSX](https://reactjs.org/docs/introducing-jsx.html)は、JavaScript の構文拡張で、コンポーネントの最終的なマークアップを返すために使用されます。

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

1. 設定編集メソッドを実装します。 このメソッドは、 `MapTo` ヘルパーおよびは、コンポーネントが空の場合にプレースホルダーを表示する情報をAEMエディターに提供します。 これは、コンポーネントがSPAに追加されたが、まだオーサリングされていない場合に発生します。 以下を `HelloWorld` クラス：

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

1. ファイルの最後で、 `MapTo` ヘルパー、 `HelloWorld` クラスと `HelloWorldEditConfig`. これにより、AEMコンポーネントのリソースタイプに基づいて、React コンポーネントがAEMコンポーネントにマッピングされます。 `we-retail-journal/components/helloworld`.

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   の完成したコード [**HelloWorld.js** はここにあります。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

1. `ImportComponents.js` ファイルを開きます。これは、次の場所にあります。 `<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`.

   必要な行を追加 `HelloWorld.js` コンパイル済みの JavaScript バンドルに他のコンポーネントが含まれています。

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

1. 内  `components`  フォルダ作成新しいファイル名： `HelloWorld.css` 兄弟として `HelloWorld.js.` ファイルに以下を入力して、の基本的なスタイル設定を作成します。 `HelloWorld` コンポーネント：

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. 再度開く `HelloWorld.js` を更新し、必要に応じて import 文を更新します。 `HelloWorld.css`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   require('./HelloWorld.css');
   
   ...
   ```

1. Apache Maven を使用してAEMにコードをデプロイします。

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. In [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js) open `/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js`. app.js で HelloWorld をクイック検索して、React コンポーネントがコンパイル済みアプリに含まれていることを確認します。

   >[!NOTE]
   >
   > **app.js** は、バンドルされた React アプリです。 コードが人間が読み取れなくなりました。 この `npm run build` コマンドを実行すると、最新のブラウザーで解釈できるコンパイル済みの JavaScript を出力する最適化されたビルドがトリガーされました。


## angularコンポーネントを作成 {#angular-component}

**担当者：フロントエンド開発者**

>[!NOTE]
>
> React の開発にのみ関心がある場合は、このセクションをスキップしてください。

次に、Angularコンポーネントが作成されます。 を開きます。 **angularアプリ** モジュール (`<src>/aem-sample-we-retail-journal/angular-app`) を使用して、任意のエディターを使用します。

1. 内部 `angular-app` フォルダーに移動して、 `src` フォルダー。 components フォルダーを展開して、既存のAngularコンポーネントファイルを表示します。

   ![Angularファイル構造](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

1. components フォルダーの下に、という名前の新しいフォルダーを追加します。 `helloworld`. の `helloworld` フォルダーを追加して次の名前の新しいファイルを追加 `helloworld.component.css, helloworld.component.html, helloworld.component.ts`.

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

1. 次を開きます： `helloworld.component.ts`. インポート文を追加してAngularをインポート `Component` および `Input` クラス。 新しいコンポーネントを作成し、 `styleUrls` および `templateUrl` から `helloworld.component.css` および `helloworld.component.html`. 最後に、クラスをエクスポートします。 `HelloWorldComponent` 予想される～の入力に伴って `displayMessage`.

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
   > もし [!DNL Sling Model] 以前に作成されたメソッドがありました **getDisplayMessage()**. このメソッドのシリアル化された JSON は次のようになります **displayMessage**：現在、Angularアプリで読んでいます。

1. 開く `helloworld.component.html` 含める `h1` タグ `displayMessage` プロパティ：

   ```html
   <h1 *ngIf="displayMessage" class="cmp-helloworld_message">
       {{displayMessage}}
   </h1>
   ```

1. 更新 `helloworld.component.css` をクリックして、コンポーネントの基本スタイルを含めます。

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

1. 更新 `helloworld.component.spec.ts` 次のテストベッドを使用：

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

1. 次の更新 `src/components/mapping.ts` を含めるには `HelloWorldComponent`. を追加します。 `HelloWorldEditConfig` コンポーネントが設定される前に、AEMエディターにプレースホルダーがマークされます。 最後に、AEMコンポーネントを、 `MapTo` ヘルパー。

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

   のフルコード [**mapping.ts** はここにあります。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

1. 更新 `src/app.module.ts` を更新するには、 **NgModule**. を **`HelloWorldComponent`** as a **宣言** が **AppModule**. また、 `HelloWorldComponent` as a **entryComponent** JSON モデルの処理時にコンパイルされ、動的にアプリに含まれるようにします。

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

   の完成したコード [**app.module.ts** はここにあります。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

1. Maven を使用してAEMにコードをデプロイします。

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. In [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js) open `/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`. クイック検索の実行 **HelloWorld** in `main.js` をクリックして、Angularコンポーネントが含まれていることを確認します。

   >[!NOTE]
   >
   > **main.js** はバンドルされたAngularアプリです。 コードが人間が読み取れなくなりました。 npm run build コマンドにより、最新のブラウザーで解釈できるコンパイル済み JavaScript を出力する最適化されたビルドがトリガーされました。

## テンプレートの更新 {#template-update}

1. React またはAngular版の編集可能テンプレートに移動します。

   * (Angular) [http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * (React) [http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

1. メインを選択 [!UICONTROL レイアウトコンテナ] をクリックし、 [!UICONTROL ポリシー] アイコンをクリックして、ポリシーを開きます。

   ![レイアウトポリシーを選択](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   の下 **[!UICONTROL プロパティ]** > **[!UICONTROL 許可されたコンポーネント]**&#x200B;を検索し、 **[!DNL Custom Components]**. 次のように表示されます。 **[!DNL Hello World]** コンポーネントを選択します。 右上隅のチェックボックスをクリックして、変更を保存します。

   ![レイアウトコンテナポリシーの設定](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

1. 保存後、 **[!DNL HelloWorld]** コンポーネントを [!UICONTROL レイアウトコンテナ].

   ![許可されたコンポーネントが更新されました](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > AEM 6.5 およびAEM 6.4.5 のみが、SPAエディターの編集可能テンプレート機能をサポートしています。 AEM 6.4 を使用している場合は、次のCRXDE Liteを使用して、許可されたコンポーネントのポリシーを手動で設定する必要があります。 `/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default` または `/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   の更新済みポリシー設定を示すCRXDE Lite [!UICONTROL 許可されたコンポーネント] 内 [!UICONTROL レイアウトコンテナ]:

   ![レイアウトコンテナ内の許可されたコンポーネントの更新済みポリシー設定を示すCRXDE Lite](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## まとめ {#putting-together}

1. 次のいずれかの React ページにAngularします。

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

1. 次を検索： **[!DNL Hello World]** コンポーネントをドラッグ&amp;ドロップ **[!DNL Hello World]** コンポーネントをページに追加します。

   ![hello world ドラッグ&amp;ドロップ](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   プレースホルダーが表示されます。

   ![こんにちは、ワールドプレースホルダー](assets/spa-editor-helloworld-tutorial-use/fig10.png)

1. コンポーネントを選択し、ダイアログに「World」または「Your Name」というメッセージを追加します。 変更内容を保存します。

   ![レンダリングコンポーネント](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   文字列「Hello」は常にメッセージの先頭に付加されます。 これは、 `HelloWorld.java` [!DNL Sling Model].

## 次の手順 {#next-steps}

[HelloWorld コンポーネントの完成したソリューション](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* の完全なソースコード [[!DNL We.Retail Journal] （GitHub 上）](https://github.com/adobe/aem-sample-we-retail-journal)
* での React の開発に関する、より詳しいチュートリアルをご覧ください。 [[!DNL Getting Started with the AEM SPA Editor - WKND Tutorial]](https://helpx.adobe.com/jp/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html)

## トラブルシューティング {#troubleshooting}

### Eclipse にプロジェクトを構築できません {#unable-to-build-project-in-eclipse}

**エラー：** 読み込み中にエラーが発生しました [!DNL We.Retail Journal] プロジェクトを Eclipse に移行し、未認識の目標を実行：

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![eclipse エラーウィザード](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**解像度**:「完了」をクリックして、後で解決します。 これにより、チュートリアルが完了することを防ぐことはできません。

**エラー**:React モジュール `react-app`は、Maven のビルド中に正常にビルドされません。

**解像度：** を削除してみてください。 `node_modules` の下のフォルダー **react-app**. Apache Maven コマンドを再実行します。 `mvn  clean install -PautoInstallSinglePackage` プロジェクトのルートから。

### AEMの未満の依存関係 {#unsatisfied-dependencies-in-aem}

![パッケージマネージャーの依存関係エラー](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

AEMの依存関係が満たされていない場合、 **[!UICONTROL AEM Package Manager]** または **[!UICONTROL AEM Web コンソール]** （Felix コンソール）。これは、SPA Editor 機能が使用できないことを示します。

### コンポーネントが表示されません

**エラー**:デプロイメントが正常に完了し、React/Version アプリのコンパイル済みバージョンに更新があることを検証した後でも、Angular/React アプリの `helloworld` コンポーネントをページにドラッグしても、コンポーネントは表示されません。 コンポーネントはAEM UI で確認できます。

**解像度**:ブラウザの履歴/キャッシュをクリアし、新しいブラウザを開くか、匿名モードを使用します。 うまくいかない場合は、ローカルのAEMインスタンスでクライアントライブラリキャッシュを無効にします。 AEMは、効率を上げるために、大きなクライアントライブラリをキャッシュしようとします。 古いコードがキャッシュされる問題を修正するために、キャッシュを手動で無効にする必要が生じる場合があります。

移動先： [http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) 「キャッシュを無効にする」をクリックします。 React/React ページに戻り、Angularを更新します。

![クライアントライブラリの再構築](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)
