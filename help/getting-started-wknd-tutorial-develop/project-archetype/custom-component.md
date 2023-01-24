---
title: カスタムコンポーネント
description: 作成したコンテンツを表示するカスタム署名コンポーネントのエンドツーエンドの作成について説明します。 ビジネスロジックをカプセル化して署名コンポーネントおよび対応する HTL を入力し、コンポーネントをレンダリングする Sling Model の開発を含みます。
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
source-git-commit: bbdb045edf5f2c68eec5094e55c1688e725378dc
workflow-type: tm+mt
source-wordcount: '4065'
ht-degree: 13%

---

# カスタムコンポーネント {#custom-component}

このチュートリアルでは、カスタムのエンドツーエンドの作成について説明します `Byline` ダイアログで作成されたコンテンツを表示し、Sling モデルの開発を検討して、コンポーネントの HTL を入力するビジネスロジックをカプセル化するAEMコンポーネント。

## 前提条件 {#prerequisites}

設定に必要なツールと手順を確認します。 [ローカル開発環境](overview.md#local-dev-environment).

### スタータープロジェクト

>[!NOTE]
>
> 前の章を正常に完了した場合は、プロジェクトを再利用して、スタータープロジェクトをチェックアウトする手順をスキップできます。

チュートリアルの構築元となるベースラインコードを確認します。

1. 以下を確認します。 `tutorial/custom-component-start` ～から分岐する [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
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

完成したコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) または、ブランチに切り替えて、コードをローカルでチェックアウトします。 `tutorial/custom-component-solution`.

## 目的

1. カスタムAEMコンポーネントの構築方法を説明します
1. Sling モデルを使用してビジネスロジックをカプセル化する方法を説明します
1. HTL スクリプト内から Sling モデルを使用する方法を説明します。

## 作成する内容 {#what-build}

WKND チュートリアルのこの部分では、記事の投稿者に関する作成済み情報を表示するために使用する署名コンポーネントが作成されます。

![署名コンポーネントの例](assets/custom-component/byline-design.png)

*署名コンポーネント*

署名コンポーネントの実装には、署名コンテンツを収集するダイアログと、次のような詳細を取得するカスタム Sling Model が含まれています。

* 名前
* 画像
* 職業

## 署名コンポーネントの作成 {#create-byline-component}

最初に、署名コンポーネントノード構造を作成し、ダイアログを定義します。 これは、AEM のコンポーネントを表し、JCR 内のその場所によってコンポーネントのリソースタイプを暗黙的に定義します。

ダイアログは、コンテンツ作成者が提供できるインターフェイスを表示します。この実装の場合、AEM WCM コアコンポーネントの **画像** コンポーネントは、署名画像のオーサリングとレンダリングを処理するために使用されるので、このコンポーネントの `sling:resourceSuperType`.

### コンポーネント定義を作成 {#create-component-definition}

1. 内 **ui.apps** モジュール、に移動します。 `/apps/wknd/components` 次の名前のフォルダーを作成します。 `byline`.
1. 内部 `byline` フォルダを追加し、 `.content.xml`

   ![ノードを作成するためのダイアログ](assets/custom-component/byline-node-creation.png)

1. 次の項目に `.content.xml` ファイルには以下の情報が含まれます。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   上記の XML ファイルは、タイトル、説明、グループなど、コンポーネントの定義を提供します。 この `sling:resourceSuperType` ポイント `core/wcm/components/image/v2/image`( これは [コア画像コンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html).

### HTL スクリプトの作成 {#create-the-htl-script}

1. 内部 `byline` フォルダー、ファイルを追加 `byline.html`：コンポーネントのHTML表示を担当します。 ファイルにフォルダーと同じ名前を付けることは重要です。Sling がこのリソースタイプをレンダリングする際に使用するデフォルトのスクリプトになるからです。

1. 以下のコードを `byline.html` に追加します。

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

この `byline.html` が [後で再び行う](#byline-htl)Sling モデルが作成されたら、 HTL ファイルの現在の状態を使用すると、コンポーネントをページにドラッグ&amp;ドロップしたときに、AEM Sites のページエディターで空の状態で表示できます。

### ダイアログ定義の作成 {#create-the-dialog-definition}

次に、以下のフィールドを含む、署名コンポーネント用のダイアログを定義します。

* **名前**：寄稿者の名前のテキストフィールド。
* **画像**：寄稿者の自己紹介写真への参照。
* **職業**：寄稿者に起因する職業のリスト。職業は、アルファベットの昇順（a～z）で並べ替えられる必要があります。

1. 内部 `byline` フォルダー、名前を付けたフォルダーを作成します。 `_cq_dialog`.
1. 内部 `byline/_cq_dialog`、という名前のファイルを追加します。 `.content.xml`. これは、ダイアログの XML 定義です。 次の XML を追加します。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="nt:unstructured"
           jcr:title="Byline"
           sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured"
               sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="granite/ui/components/coral/foundation/tabs"
                       maximized="{Boolean}false">
                   <items jcr:primaryType="nt:unstructured">
                       <asset
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}false"/>
                       <metadata
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}true"/>
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
                                               <name
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                       emptyText="Enter the contributor's name to display."
                                                       fieldDescription="The contributor's name to display."
                                                       fieldLabel="Name"
                                                       name="./name"
                                                       required="{Boolean}true"/>
                                               <occupations
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                       fieldDescription="A list of the contributor's occupations."
                                                       fieldLabel="Occupations"
                                                       required="{Boolean}false">
                                                   <field
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           emptyText="Enter an occupation"
                                                           name="./occupations"/>
                                               </occupations>
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

   これらのダイアログノード定義では、 [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) どのダイアログタブが `sling:resourceSuperType` コンポーネント（この場合は） **コアコンポーネントの画像コンポーネント**.

   ![署名用の完了済みダイアログ](assets/custom-component/byline-dialog-created.png)

### ポリシーダイアログの作成 {#create-the-policy-dialog}

ダイアログ作成と同じ方法でポリシーダイアログ（以前のデザインダイアログ）を作成して、コアコンポーネントの画像コンポーネントから継承されたポリシー設定の不要なフィールドを非表示にします。

1. 内部 `byline` フォルダー、名前を付けたフォルダーを作成します。 `_cq_design_dialog`.
1. 内部 `byline/_cq_design_dialog`、という名前のファイルを作成します。 `.content.xml`. ファイルを次のように更新します。を次の XML に置き換えます。 を開くのが最も簡単です。 `.content.xml` をクリックし、その中に以下の XML をコピー&amp;ペーストします。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Byline"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <decorative
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <altValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <titleValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <displayCaptionPopup
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <disableUuidTracking
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                   </items>
                               </content>
                           </items>
                       </properties>
                       <features
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <accordion
                                               jcr:primaryType="nt:unstructured">
                                           <items jcr:primaryType="nt:unstructured">
                                               <orientation
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                               <crop
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                           </items>
                                       </accordion>
                                   </items>
                               </content>
                           </items>
                       </features>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   前の基準 **ポリシーダイアログ** XML は [コアコンポーネントの画像コンポーネント](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   ダイアログ設定のと同様に、 [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) を使用して、無関係なフィールドを非表示にします。これ以外の場合は、 `sling:resourceSuperType`を使用して、 `sling:hideResource="{Boolean}true"` プロパティ。

### コードのデプロイ {#deploy-the-code}

1. 変更を同期 `ui.apps` IDE または Maven スキルを使用して、

   ![AEMサーバーの署名コンポーネントに書き出し](assets/custom-component/export-byline-component-aem.png)

## ページへのコンポーネントの追加 {#add-the-component-to-a-page}

AEMコンポーネントの開発に重点を置いたシンプルな作業を維持するために、署名コンポーネントを現在の状態で記事ページに追加して、 `cq:Component` ノード定義が正しい。 また、AEMが新しいコンポーネント定義を認識し、コンポーネントのダイアログがオーサリングに使用できることを確認する場合にも使用します。

### AEM Assetsへの画像の追加

まず、サンプルのヘッドショットをAEM Assetsにアップロードして、署名コンポーネントに画像を設定する際に使用します。

1. AEM Assetsの LA Skateparks フォルダーに移動します。 [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. のヘッドショットをアップロード  **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** をフォルダーに追加します。

   ![ヘッドショットがAEM Assetsにアップロードされました](assets/custom-component/stacey-roswell-headshot-assets.png)

### コンポーネントの作成 {#author-the-component}

次に、署名コンポーネントをAEMのページに追加します。 署名コンポーネントは **WKND Sites プロジェクト — コンテンツ** コンポーネントグループ（経由） `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` 定義を使用する場合、任意のユーザーが自動的に使用できます **コンテナ** その **ポリシー** 許可 **WKND Sites プロジェクト — コンテンツ** コンポーネントグループ したがって、記事ページのレイアウトコンテナで使用できます。

1. LA Skatepark の記事 ( ) に移動します。 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. 左側のサイドバーから、 **署名コンポーネント** 次に **bottom** をクリックします。

   ![署名コンポーネントをページに追加](assets/custom-component/add-to-page.png)

1. 左側のサイドバーが開いていることを確認します。**そして見え、**&#x200B;アセットファインダー**が選択されている。

1. を選択します。 **署名コンポーネントのプレースホルダー**&#x200B;をクリックし、次にアクションバーを表示して、 **レンチ** アイコンをクリックしてダイアログを開きます。

1. ダイアログが開き、最初のタブ（アセット）がアクティブな状態で、左側のサイドバーを開き、アセットファインダーから画像を画像ドロップゾーンにドラッグします。 WKND ui.content パッケージに含まれる Stacey Roswells のバイオ画像を検索するには、「stacey」を検索します。

   ![ダイアログに画像を追加](assets/custom-component/add-image.png)

1. 画像を追加したら、「**プロパティ**」タブをクリックして「**名前**」および「**職業**」に入力します。

   職業を入力する場合は、次の場所に入力します。 **逆アルファベット** Sling Model に実装されている英文化化ビジネスロジックが検証されるように、順序を変更します。

   次をタップします。 **完了** ボタンをクリックして変更を保存します。

   ![署名コンポーネントのプロパティの設定](assets/custom-component/add-properties.png)

   AEM作成者は、ダイアログを使用してコンポーネントを設定および作成します。 この時点で、署名コンポーネントの開発時に、データを収集するためのダイアログが含まれますが、作成したコンテンツをレンダリングするロジックはまだ追加されていません。 したがって、プレースホルダーのみが表示されます。

1. ダイアログを保存したら、に移動します。 [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) およびは、コンポーネントのコンテンツがAEMページの署名コンポーネントコンテンツノードに格納される方法を確認します。

   LA Skate Parks ページの下に、署名コンポーネントのコンテンツノードを見つけます。 `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   プロパティ名に注意してください `name`, `occupations`、および `fileReference` が **署名ノード**.

   また、 `sling:resourceType` の `wknd/components/content/byline` これは、このコンテンツノードを署名コンポーネントの実装にバインドするものです。

   ![CRXDE の署名プロパティ](assets/custom-component/byline-properties-crxde.png)

## 署名 Sling モデルを作成 {#create-sling-model}

次に、データモデルとして機能し、署名コンポーネントのビジネスロジックを格納する Sling モデルを作成します。

Sling モデルは、JCR から Java™変数へのデータのマッピングを容易にし、AEMコンテキストでの開発時に効率を高める注釈駆動の Java™ POJO(Plain Old Java™ Objects) です。

### Maven の依存関係の確認 {#maven-dependency}

署名 Sling モデルは、AEMが提供する複数の Java™ API に依存しています。 これらの API は、 `dependencies` 次に示す `core` モジュールの POM ファイル。 このチュートリアルに使用するプロジェクトは、AEM as a Cloud Service用に構築されています。 ただし、AEM 6.5/6.4 との下位互換性があるので一意です。そのため、Cloud ServiceとAEM 6.x の両方の依存関係が含まれます。

1. を開きます。 `pom.xml` の下のファイル `<src>/aem-guides-wknd/core/pom.xml`.
1. 依存関係を検索 `aem-sdk-api` - **AEMas a Cloud Serviceのみ**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   この [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=ja) には、AEMで公開されるすべてのパブリック Java™ API が含まれています。 この `aem-sdk-api` は、このプロジェクトを構築する際にデフォルトで使用されます。 バージョンは、次の場所にあるプロジェクトのルートからの親リアクター POM に保持されます。 `aem-guides-wknd/pom.xml`.

1. の依存関係を検索 `uber-jar` - **AEM 6.5/6.4 のみ**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   この `uber-jar` が含まれるのは、 `classic` プロファイルが呼び出されました。 `mvn clean install -PautoInstallSinglePackage -Pclassic`. これもこのプロジェクトに固有のものです。 AEMプロジェクトアーキタイプから生成される、実際のプロジェクトでは、 `uber-jar` がデフォルトです ( 指定したAEMのバージョンが 6.5 または 6.4 の場合 )。

   この [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) には、AEM 6.x で公開されているすべてのパブリック Java™ API が含まれています。バージョンは、プロジェクトのルートから親リアクター POM に保持されます `aem-guides-wknd/pom.xml`.

1. 依存関係を検索 `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   これは、AEMコアコンポーネントによって公開される完全なパブリック Java™ API です。 AEMコアコンポーネントは、AEMの外部で管理されるプロジェクトなので、別のリリースサイクルがあります。 このため、依存関係は別々に含める必要があり、 **not** 次に含まれる `uber-jar` または `aem-sdk-api`.

   uber-jar と同様に、この依存関係のバージョンは、次の親リアクター POM ファイルで保持されます。 `aem-guides-wknd/pom.xml`.

   このチュートリアルの後半では、コアコンポーネントの画像クラスを使用して、署名コンポーネントに画像を表示します。 Sling モデルを構築してコンパイルするには、コアコンポーネントの依存関係が必要です。

### Byline インターフェイス {#byline-interface}

署名用のパブリック Java™インターフェイスを作成します。 この `Byline.java` は、 `byline.html` HTL スクリプト。

1. 内部、 `core` 内のモジュール `core/src/main/java/com/adobe/aem/guides/wknd/core/models` フォルダ作成ファイル名： `Byline.java`

   ![署名インターフェイスを作成](assets/custom-component/create-byline-interface.png)

1. 以下のメソッドで `Byline.java` を更新します。

   ```java
   package com.adobe.aem.guides.wknd.core.models;
   
   import java.util.List;
   
   /**
   * Represents the Byline AEM Component for the WKND Site project.
   **/
   public interface Byline {
       /***
       * @return a string to display as the name.
       */
       String getName();
   
       /***
       * Occupations are to be sorted alphabetically in a descending order.
       *
       * @return a list of occupations.
       */
       List<String> getOccupations();
   
       /***
       * @return a boolean if the component has enough content to display.
       */
       boolean isEmpty();
   }
   ```

   最初の 2 つのメソッドは、 **名前** および **職業** 署名コンポーネント用の

   この `isEmpty()` メソッドは、コンポーネントにレンダリングするコンテンツがあるかどうか、または設定を待っているかどうかを判断するために使用されます。

   画像に対するメソッドがないことに注意してください。 [これは後でレビューされます](#tackling-the-image-problem).

1. パブリック Java™クラスを含む Java™パッケージ（この場合は Sling モデル）は、パッケージの  `package-info.java` ファイル。

   WKND ソースの Java™パッケージ以降 `com.adobe.aem.guides.wknd.core.models` バージョンを宣言する `1.0.0`を追加し、新しい公開インターフェイスおよびメソッドを追加する場合は、バージョンを `1.1.0`. 次の場所にあるファイルを開きます。 `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` および更新 `@Version("1.0.0")` から `@Version("2.1.0")`.

       &quot;&#39;
       @Version(&quot;2.1.0&quot;)
       package com.adobe.aem.guides.wknd.core.models;
       
       import org.osgi.annotation.versioning.Version;
       &quot;&#39;
   
このパッケージ内のファイルに変更が加えられると、 [パッケージのバージョンは、意味的に調整する必要があります](https://semver.org/). そうでない場合、Maven プロジェクトの [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd/tree/master/maven/bnd-baseline-maven-plugin) は無効なパッケージバージョンを検出し、ビルドを中断します。 幸いにも、失敗した場合、Maven プラグインは無効な Java™パッケージのバージョンと、そのバージョンを報告します。 を更新します。 `@Version("...")` Java™パッケージの宣言 `package-info.java` を、プラグインで推奨される修正用のバージョンに追加する。

### 署名実装 {#byline-implementation}

この `BylineImpl.java` は、 `Byline.java` 以前に定義されたインターフェイス。 `BylineImpl.java` の完全なコードは、この節の最後に記載しています。

1. という名前のフォルダーを作成します。 `impl` 下 `core/src/main/java/com/adobe/aem/guides/core/models`.
1. 内 `impl` フォルダ、ファイルの作成 `BylineImpl.java`.

   ![署名 Impl ファイル](assets/custom-component/byline-impl-file.png)

1. `BylineImpl.java`を開きます。これによって `Byline` インターフェイス。 IDE のオートコンプリート機能を使用するか、手動でファイルを更新して、 `Byline` インターフェイス：

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   import java.util.List;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   
   public class BylineImpl implements Byline {
   
       @Override
       public String getName() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public List<String> getOccupations() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public boolean isEmpty() {
           // TODO Auto-generated method stub
           return false;
       }
   }
   ```

1. 以下のクラスレベルの注釈で `BylineImpl.java` を更新して、Sling Model 注釈を追加します。この `@Model(..)`注釈は、クラスを Sling モデルに変換するものです。

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ...
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
       ...
   }
   ```

   この注釈とそのパラメーターを確認してみましょう:

   * この `@Model` annotation は、BylineImpl をAEMにデプロイする際に Sling Model として登録します。
   * この `adaptables` パラメーターは、このモデルがリクエストで適応できることを指定します。
   * この `adapters` パラメーターを使用すると、実装クラスを Byline インターフェイスに登録できます。 これにより、HTL スクリプトは（実装が直接ではなく）インターフェイスを介して Sling モデルを呼び出すことができます。 adapters について詳しくは、[こちら](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110)を参照してください。
   * この `resourceType` は署名コンポーネントのリソースタイプ（以前に作成した）を指し、複数の実装がある場合に正しいモデルの解決に役立ちます。 モデルクラスのリソースタイプとの関連付けについて詳しくは、[こちら](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130)を参照してください。

### Sling Model メソッドの実装 {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

最初に実装されるメソッドは次のとおりです。 `getName()`という名前の場合は、単にプロパティの下の署名の JCR コンテンツノードに保存されている値を返します。 `name`.

この場合、 `@ValueMapValue` Sling Model 注釈は、リクエストのリソースの ValueMap を使用して Java™フィールドに値を挿入するために使用されます。


```java
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private String name;

    ...
    @Override
    public String getName() {
        return name;
    }
    ...
}
```

JCR プロパティは Java™フィールドとして名前を共有するので（両方とも「name」）、 `@ValueMapValue` はこの関連付けを自動的に解決し、プロパティの値を「Java™」フィールドに挿入します。

#### getOccupations() {#implementing-get-occupations}

次に実装する方法は次のとおりです。 `getOccupations()`. このメソッドは、JCR プロパティに保存されている職業を読み込みます `occupations` 並べ替えられた（アルファベット順の）コレクションを返します。

に示したのと同じ方法の使用 `getName()` プロパティ値は、Sling モデルのフィールドに挿入できます。

挿入された Java™フィールドを介して JCR プロパティの値を Sling Model で使用できるようになったら、 `occupations`を指定しない場合、並べ替えビジネスロジックは `getOccupations()` メソッド。


```java
import java.util.ArrayList;
import java.util.Collections;
  ...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
    @Override
    public List<String> getOccupations() {
        if (occupations != null) {
            Collections.sort(occupations);
            return new ArrayList<String>(occupations);
        } else {
            return Collections.emptyList();
        }
    }
    ...
}
  ...
```


#### isEmpty() {#implementing-is-empty}

最後のパブリックメソッドは、 `isEmpty()` これにより、コンポーネントがレンダリングに十分な作成を行ったと見なすタイミングが決まります。

このコンポーネントの場合、ビジネス要件は 3 つのフィールドすべてになります。 `name, image and occupations` 入力する必要があります *前* コンポーネントはレンダリングできます。


```java
import org.apache.commons.lang3.StringUtils;
  ...
public class BylineImpl implements Byline {
    ...
    @Override
    public boolean isEmpty() {
        if (StringUtils.isBlank(name)) {
            // Name is missing, but required
            return true;
        } else if (occupations == null || occupations.isEmpty()) {
            // At least one occupation is required
            return true;
        } else if (/* image is not null, logic to be determined */) {
            // A valid image is required
            return true;
        } else {
            // Everything is populated, so this component is not considered empty
            return false;
        }
    }
    ...
}
```


#### 「画像の問題」への対処 {#tackling-the-image-problem}

名前と職業の条件をチェックすることは簡単で、Apache Commons Lang3 が便利です [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) クラス。 しかし、 **画像の存在** は、コアコンポーネントの画像コンポーネントを画像の表示に使用するので、検証できます。

これに対処するには、2 つの方法があります。

をチェックします。 `fileReference` JCR プロパティがアセットに解決されます。 *または* このリソースをコアコンポーネントの画像 Sling モデルに変換し、 `getSrc()` メソッドが空ではありません。

次を使用します。 **秒** アプローチ。 第 1 のアプローチで十分である可能性が高いですが、このチュートリアルでは、Sling モデルの他の機能を調べるために後者を使用します。

1. 画像を取得するプライベートメソッドを作成します。 このメソッドは、HTL 自体で Image オブジェクトを公開する必要がなく、駆動にのみ使用されるので、非公開のままにします `isEmpty().`

   次のプライベートメソッドを `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   上記のように、 **Image Sling Model**:

   1 つ目は、 `@Self` 注釈 ( 現在のリクエストをコアコンポーネントの `Image.class`

   2 つ目は、 [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) 便利なサービスである OSGi サービス。Java™コード内の他のタイプの Sling モデルを作成するのに役立ちます。

   2 つ目の方法を使用します。

   >[!NOTE]
   >
   >実際の実装では、次を使用して「1」にアプローチします。 `@Self` よりシンプルでエレガントなソリューションなので、をお勧めします。 このチュートリアルでは、2 つ目の方法を使用します。役立つ Sling モデルのより多くのファセットを探索する必要があるので、より複雑なコンポーネントになります。

   Sling モデルは OSGi サービスではなく Java™ POJO なので、通常の OSGi 挿入注釈です `@Reference` **できません** を使用する代わりに、Sling Model は特別な **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 類似の機能を提供する注釈。

1. 更新 `BylineImpl.java` を含めるには `OSGiService` 挿入する注釈 `ModelFactory`:

   ```java
   import org.apache.sling.models.factory.ModelFactory;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   ...
   public class BylineImpl implements Byline {
       ...
       @OSGiService
       private ModelFactory modelFactory;
   }
   ```

   を使用 `ModelFactory` 使用可能なコアコンポーネントの画像 Sling モデルは、次のものを使用して作成できます。

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   ただし、このメソッドにはリクエストとリソースの両方が必要で、Sling モデルではまだ使用できません。 これらを取得するには、より多くの Sling Model 注釈が使用されます。

   現在のリクエストを取得するには、 **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 注釈を使用して、 `adaptable` ( `@Model(..)` as `SlingHttpServletRequest.class`を Java™クラスフィールドに追加します。

1. を **@Self** 注釈を使用して **SlingHttpServletRequest リクエスト**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   注意：を使用 `@Self Image image` コアコンポーネントの画像 Sling モデルを挿入することは、上記のオプションでした。 `@Self` annotation は、適応可能なオブジェクト（この場合は SlingHttpServletRequest）の挿入を試み、注釈フィールドタイプに合わせます。 コアコンポーネントの画像 Sling モデルは SlingHttpServletRequest オブジェクトから適応可能なので、これは動作し、より探索的なコードよりも少ないコードです `modelFactory` アプローチ。

   これで、ModelFactory API を使用して画像モデルをインスタンス化するために必要な変数が挿入されます。 Sling Model のを使用します **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** 注釈を追加し、Sling Model のインスタンス化後にこのオブジェクトを取得します。

   `@PostConstruct` は非常に役に立ち、コンストラクターと同様の能力で動作しますが、クラスがインスタンス化され、注釈が付けられたすべての Java™フィールドが挿入された後に呼び出されます。 他の Sling Model 注釈は Java™クラスフィールド（変数）に注釈を付けます。 `@PostConstruct` は、通常、 `init()` （ただし、任意の名前を付けることができます）。

1. 追加 **@PostConstruct** メソッド：

   ```java
   import javax.annotation.PostConstruct;
   ...
   public class BylineImpl implements Byline {
       ...
       private Image image;
   
       @PostConstruct
       private void init() {
           image = modelFactory.getModelFromWrappedRequest(request,
                                                           request.getResource(),
                                                           Image.class);
       }
       ...
   }
   ```

   Sling Model は OSGi サービス&#x200B;**ではない**&#x200B;ので、クラスの状態を安全に管理できます。頻繁 `@PostConstruct` プレーンコンストラクターと同様に、後で使用するために Sling Model クラスの状態を派生および設定します。

   この `@PostConstruct` メソッドで例外がスローされ、Sling モデルがインスタンス化されず、null になる。

1. **getImage()** を更新して、画像オブジェクトを返すだけで済むようになりました。

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. 次に戻ろう `isEmpty()` 実装を完了します。

   ```java
   @Override
   public boolean isEmpty() {
      final Image componentImage = getImage();
   
       if (StringUtils.isBlank(name)) {
           // Name is missing, but required
           return true;
       } else if (occupations == null || occupations.isEmpty()) {
           // At least one occupation is required
           return true;
       } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
           // A valid image is required
           return true;
       } else {
           // Everything is populated, so this component is not considered empty
           return false;
       }
   }
   ```

   複数の `getImage()` は初期化されたを返すので、問題ありません `image` クラス変数で、を呼び出さない `modelFactory.getModelFromWrappedRequest(...)` それは過度に費用がかかるわけではなく、不必要に呼び出すのを避ける価値がある。

1. 最終 `BylineImpl.java` は次のようになります。


   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import java.util.ArrayList;
   import java.util.Collections;
   import java.util.List;
   import javax.annotation.PostConstruct;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.factory.ModelFactory;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       /**
       * @PostConstruct is immediately called after the class has been initialized
       * but BEFORE any of the other public methods. 
       * It is a good method to initialize variables that is used by methods in the rest of the model
       *
       */
       @PostConstruct
       private void init() {
           // set the image object
           image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
       }
   
       @Override
       public String getName() {
           return name;
       }
   
       @Override
       public List<String> getOccupations() {
           if (occupations != null) {
               Collections.sort(occupations);
               return new ArrayList<String>(occupations);
           } else {
               return Collections.emptyList();
           }
       }
   
       @Override
       public boolean isEmpty() {
           final Image componentImage = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
               // A valid image is required
               return true;
           } else {
               // Everything is populated, so this component is not considered empty
               return false;
           }
       }
   
       /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
       */
       private Image getImage() {
           return image;
       }
   }
   ```


## 署名 HTL {#byline-htl}

内 `ui.apps` モジュール、開く `/apps/wknd/components/byline/byline.html` これは、AEMコンポーネントの以前の設定で作成されたものです。

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

この HTL スクリプトでおこなうことを確認しましょう。

* この `placeholderTemplate` は、コアコンポーネントのプレースホルダーを指しています。このプレースホルダーは、コンポーネントが完全に設定されていない場合に表示されます。 これは、AEM Sitesページエディターで、上で説明したように、コンポーネントタイトルを持つボックスとしてレンダリングされます ( `cq:Component`&#39;s  `jcr:title` プロパティ。

* この `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` は、 `placeholderTemplate` 上で定義され、ブール値で渡されます ( 現在、次のようにハードコードされています： `false`) をプレースホルダーテンプレートに追加します。 条件 `isEmpty` が true の場合、プレースホルダテンプレートはグレーのボックスをレンダリングし、そうでない場合は何もレンダリングされません。

### 署名 HTL を更新

1. 以下のスケルタル HTML 構造で **byline.html** を更新します。

   ```html
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!--/* Include the Core Components Image Component */-->
           </div>
           <h2 class="cmp-byline__name"><!--/* Include the name */--></h2>
           <p class="cmp-byline__occupations"><!--/* Include the occupations */--></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   CSS クラスは [BEM 命名規則](https://getbem.com/naming/)に従うことに注意してください。BEM 規則の使用は必須ではありませんが、コアコンポーネント CSS クラスで使用され、一般的にクリーンで読みやすい CSS ルールになるので、BEM をお勧めします。

### HTL での Sling Model オブジェクトのインスタンス化 {#instantiating-sling-model-objects-in-htl}

この [ブロックステートメントを使用](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) は、Sling モデルオブジェクトを HTL スクリプトでインスタンス化し、HTL 変数に割り当てるために使用されます。

この `data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` は、BylineImpl で実装された署名インターフェイス (com.adobe.aem.guides.wknd.models.Byline) を使用して、現在の SlingHttpServletRequest をそのインターフェイスに適応し、結果を HTL 変数名のバイライン ( `data-sly-use.<variable-name>`) をクリックします。

1. 外部を更新 `div` を参照する **署名** Sling Model by its public interface:

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Sling Model メソッドへのアクセス {#accessing-sling-model-methods}

HTL は、 JSTL およびは、Java™ゲッターメソッド名の同じ短縮形を使用します。

例えば、署名 Sling モデルのの呼び出し `getName()` 方法はに短縮できます `byline.name`同様に、 `byline.isEmpty`の場合は、 `byline.empty`. 完全なメソッド名の使用 `byline.getName` または `byline.isEmpty`でも動作します。 次の点に注意してください。 `()` は、HTL でメソッドを呼び出すためには使用されません（JSTL と同様）。

パラメーターが必要な Java™メソッド **できません** を HTL で使用する。 これは、HTL のロジックをシンプルにするための設計によるものです。

1. 署名名は、 `getName()` メソッドを使用します。 `${byline.name}`.

   を更新します。 `h2` タグ：

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### HTL 式のオプションの使用 {#using-htl-expression-options}

[HTL 式のオプション](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) HTL 内のコンテンツの修飾子として機能し、日付の書式設定から i18n 翻訳まで幅広く機能します。 式は、リストまたは値の配列を結合する場合にも使用できます。これは、職業をコンマ区切り形式で表示するために必要なものです。

式は `@` 演算子を使用して、HTL 式に含めることができます。

1. 職業のリストを「,」で結合するには、以下のコードを使用します。

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### プレースホルダーの条件付き表示 {#conditionally-displaying-the-placeholder}

AEMコンポーネントのほとんどの HTL スクリプトでは、 **プレースホルダーパラダイム** 作成者に視覚的な手がかりを提供する **コンポーネントが正しく作成されず、AEM パブリッシュに表示されないことを示す**. この決定を推進する規則は、コンポーネントのバッキング Sling Model にメソッドを実装することです。この場合、次のようになります。 `Byline.isEmpty()`.

この `isEmpty()` メソッドが署名 Sling モデルで呼び出され、結果（または負の値）が `!` 演算子 ) が `hasContent`:

1. 外部を更新 `div` という名前の HTL 変数を保存するには、次のようにします。 `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   の使用に注意してください。 `data-sly-test`、HTL `test` block がキーの場合は、HTL 変数を設定し、元の要素をレンダリング/レンダリングしません。 HTL 式評価の結果に基づきます。 「true」の場合、HTML要素はレンダリングされ、それ以外の場合はレンダリングされません。

   この HTL 変数 `hasContent` を再利用して、プレースホルダーを条件付きで表示/非表示にできるようになりました。

1. 条件付き呼び出しを `placeholderTemplate` を次のように指定します。

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### コアコンポーネントを使用した画像の表示 {#using-the-core-components-image}

の HTL スクリプト `byline.html` がほぼ完了し、画像しか欠落していなくなりました。

を `sling:resourceSuperType` は、コアコンポーネントの画像コンポーネントを指し、画像のオーサリングには、コアコンポーネントの画像コンポーネントを使用して画像をレンダリングできます。

この場合、現在の署名リソースを含めますが、リソースタイプを使用して、コアコンポーネントの画像コンポーネントのリソースタイプを強制的に指定します `core/wcm/components/image/v2/image`. これは、コンポーネントの再利用のための強力なパターンです。 これに対して、HTL の `data-sly-resource` ブロックが使用されます。

1. を `div` クラスを持つ `cmp-byline__image` を次のように設定します。

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   この `data-sly-resource`には、相対パスを介して現在のリソースが含まれます `'.'`現在のリソース（または署名コンテンツリソース）を、 `core/wcm/components/image/v2/image`.

   コアコンポーネントのリソースタイプは、プロキシ経由ではなく直接使用されます。これはスクリプト内で使用され、コンテンツに保持されないためです。

2. 完了 `byline.html` 以下：

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline" 
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
           data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
       <h2 class="cmp-byline__name">${byline.name}</h2>
       <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. コードベースをローカルの AEM インスタンスにデプロイします。変更が加えられたため `core` および `ui.apps` 両方のモジュールをデプロイする必要があります。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   AEM 6.5/6.4 にデプロイするには、 `classic` プロファイル：

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > また、Maven プロファイルを使用して、ルートからプロジェクト全体を構築することもできます `autoInstallSinglePackage` ただし、これにより、ページ上のコンテンツの変更が上書きされる場合があります。 これは、 `ui.content/src/main/content/META-INF/vault/filter.xml` は、既存のAEMコンテンツをきれいに上書きするように、チュートリアルのスターターコードを変更しました。 実際のシナリオでは、これは問題ではありません。

### スタイルが設定されていない署名コンポーネントの確認 {#reviewing-the-unstyled-byline-component}

1. アップデートをデプロイした後、 [LA スケートパークスの究極ガイド ](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) ページ、またはこの章で既に署名コンポーネントを追加した場所。

1. この **画像**, **名前**、および **職業** 現在は表示され、スタイルが設定されていないが、作業中の署名コンポーネントが存在する。

   ![スタイル設定されていない署名コンポーネント](assets/custom-component/unstyled.png)

### Sling Model 登録の確認 {#reviewing-the-sling-model-registration}

[AEM Web コンソールの Sling Models Status 表示](http://localhost:4502/system/console/status-slingmodels)には、AEM に登録されたすべての Sling Model が表示されます。署名 Sling Model は、このリストを確認することで、インストールされ、認識されていることを検証できます。

この **BylineImpl** がこのリストに表示されない場合、Sling モデルの注釈に関する問題か、モデルが正しいパッケージに追加されなかった (`com.adobe.aem.guides.wknd.core.models`) をコアプロジェクトに追加しました。

![署名 Sling Model が登録されました](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## 署名のスタイル {#byline-styles}

署名コンポーネントを提供されているクリエイティブデザインに合わせるには、スタイルを設定します。 これは、SCSS ファイルを使用し、 **ui.frontend** モジュール。

### デフォルトのスタイルを追加

署名コンポーネントのデフォルトスタイルを追加します。

1. IDE とに戻ります。 **ui.frontend** ～の下に投影する `/src/main/webpack/components`:
1. `_byline.scss` という名前のファイルを作成します。

   ![署名プロジェクトエクスプローラー](assets/custom-component/byline-style-project-explorer.png)

1. 署名実装 CSS（SCSS として書き込む）を `_byline.scss`:

   ```scss
   .cmp-byline {
       $imageSize: 60px;
   
       .cmp-byline__image {
           float: left;
   
       /* This class targets a Core Component Image CSS class */
       .cmp-image__image {
           width: $imageSize;
           height: $imageSize;
           border-radius: $imageSize / 2;
           object-fit: cover;
           }
       }
   
       .cmp-byline__name {
           font-size: $font-size-medium;
           font-family: $font-family-serif;
           padding-top: 0.5rem;
           margin-left: $imageSize + 25px;
           margin-bottom: .25rem;
           margin-top:0rem;
       }
   
       .cmp-byline__occupations {
           margin-left: $imageSize + 25px;
           color: $gray;
           font-size: $font-size-xsmall;
           text-transform: uppercase;
       }
   }
   ```

1. ターミナルを開き、 `ui.frontend` モジュール。
1. を開始します。 `watch` 次の npm コマンドを使用してプロセスを実行します。

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. ブラウザーに戻り、 [LA SkateParks 記事](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). コンポーネントの更新済みのスタイルが表示されます。

   ![署名コンポーネントの完成](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > 古い CSS が提供されないようにブラウザーのキャッシュをクリアし、署名コンポーネントでページを更新して完全なスタイルを取得する必要が生じる場合があります。

## おめでとうございます。 {#congratulations}

これで、Adobe Experience Managerを使用してカスタムコンポーネントを最初から作成しました。

### 次の手順 {#next-steps}

引き続き、署名 Java™コードの JUnit テストを記述して、すべてが正しく開発され、実装されたビジネスロジックが正しく完了していることを確認し、AEMコンポーネントの開発について学びます。

* [単体テストまたはAEMコンポーネントの作成](unit-testing.md)

で完成したコードを表示する [GitHub](https://github.com/adobe/aem-guides-wknd) または、Git ブランチのローカルのにコードを確認してデプロイします。 `tutorial/custom-component-solution`.

1. のクローン [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) リポジトリ。
1. 以下を確認します。 `tutorial/custom-component-solution` 分岐
