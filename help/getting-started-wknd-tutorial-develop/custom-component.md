---
title: カスタムコンポーネント
description: 作成したコンテンツを表示するカスタム署名コンポーネントのエンドツーエンドの作成について扱います。ビジネスロジックをカプセル化して署名コンポーネントおよび対応する HTL を入力し、コンポーネントをレンダリングする Sling Model の開発を含みます。
sub-product: サイト
feature: sling-models
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
translation-type: tm+mt
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '3961'
ht-degree: 20%

---


# カスタムコンポーネント{#custom-component}

このチュートリアルでは、ダイアログで作成されたコンテンツを表示するAEM Bylineコンポーネントのエンドツーエンドの作成について説明し、Slingモデルの開発を進め、コンポーネントのHTLを埋め込むビジネスロジックをカプセル化します。

## 前提条件 {#prerequisites}

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。

### スタータープロジェクト

>[!NOTE]
>
> 前の章を正常に完了した場合は、プロジェクトを再利用し、スタータープロジェクトをチェックアウトする手順をスキップできます。

チュートリアルが構築する基本行コードを調べます。

1. [GitHub](https://github.com/adobe/aem-guides-wknd)の`tutorial/custom-component-start`ブランチを調べます。

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
   ```

1. Mavenのスキルを使用して、ローカルAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5または6.4を使用している場合は、Mavenコマンドに`classic`プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution)に常に表示できます。また、ブランチ`tutorial/custom-component-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。

## 目的

1. カスタムAEMコンポーネントの構築方法を理解する
1. Slingモデルにビジネスロジックをカプセル化する方法を学びます。
1. HTLスクリプト内からSlingモデルを使用する方法を理解します。

## 作成する内容 {#byline-component}

WKNDチュートリアルのこの部分では、Bylineコンポーネントが作成され、記事の寄稿者に関する作成済み情報を表示するのに使用されます。

![bylineコンポーネントの例](assets/custom-component/byline-design.png)

*Bylineコンポーネント*

Bylineコンポーネントの実装には、署名の内容を収集するダイアログと、署名の内容を取得するカスタムSlingモデルが含まれます。

* 名前
* 画像
* 職業

## 署名コンポーネントの作成 {#create-byline-component}

最初に、Bylineコンポーネントのノード構造を作成し、ダイアログを定義します。 これは、AEM のコンポーネントを表し、JCR 内のその場所によってコンポーネントのリソースタイプを暗黙的に定義します。

ダイアログは、コンテンツ作成者が提供できるインターフェイスを表示します。この実装では、AEM WCMコアコンポーネントの&#x200B;**Image**&#x200B;コンポーネントを使用してBylineのイメージのオーサリングとレンダリングを処理するので、コンポーネントの`sling:resourceSuperType`として設定されます。

### コンポーネント定義の作成{#create-component-definition}

1. **ui.apps**&#x200B;モジュールで、`/apps/wknd/components`に移動し、`byline`という名前の新しいフォルダーを作成します。
1. `byline`フォルダーの下に、`.content.xml`という名前の新しいファイルを追加します

   ![ノードを作成するダイアログ](assets/custom-component/byline-node-creation.png)

1. `.content.xml`ファイルに次の内容を入力します。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   上記のXMLファイルは、タイトル、説明、グループなど、コンポーネントの定義を提供します。 `sling:resourceSuperType`は`core/wcm/components/image/v2/image`を指します。これは[コアイメージコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html)です。

### HTL スクリプトの作成 {#create-the-htl-script}

1. `byline`フォルダーの下に、新しいファイル`byline.html`を追加します。このファイルは、コンポーネントのHTML表示を行います。 ファイルにフォルダーと同じ名前を付けることは重要です。これは、Slingがこのリソースタイプをレンダリングする際に使用するデフォルトのスクリプトになるからです。

1. 以下のコードを `byline.html` に追加します。

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

`byline.html` は、後で [、Slingモデルが作成された後で](#byline-htl)再訪問されます。HTLファイルの現在の状態では、コンポーネントをページ上にドラッグ&amp;ドロップしたときに、AEMサイトのページエディターに空の状態で表示できます。

### ダイアログ定義の作成 {#create-the-dialog-definition}

次に、以下のフィールドを含む、署名コンポーネント用のダイアログを定義します。

* **名前**：寄稿者の名前のテキストフィールド。
* **画像**：寄稿者の自己紹介写真への参照。
* **職業**：寄稿者に起因する職業のリスト。職業は、アルファベットの昇順（a～z）で並べ替えられる必要があります。

1. `byline`フォルダーの下に、`_cq_dialog`という名前の新しいフォルダーを作成します。
1. `byline/_cq_dialog`の下に、`.content.xml`という名前の新しいファイルを追加します。 これは、ダイアログのXML定義です。 追加次のXML:

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

   これらのダイアログノード定義は、[Sling Resource Marge](https://sling.apache.org/documentation/bundles/resource-merger.html)を使用して、`sling:resourceSuperType`コンポーネントから継承するダイアログタブを制御します。この場合、**コアコンポーネント&#39;イメージコンポーネント**&#x200B;です。

   ![署名欄の対話を完了](assets/custom-component/byline-dialog-created.png)

### ポリシーダイアログの作成 {#create-the-policy-dialog}

ダイアログ作成と同じ方法でポリシーダイアログ（以前のデザインダイアログ）を作成して、コアコンポーネントの画像コンポーネントから継承されたポリシー設定の不要なフィールドを非表示にします。

1. `byline`フォルダーの下に、`_cq_design_dialog`という名前の新しいフォルダーを作成します。
1. `byline/_cq_design_dialog`の下に、`.content.xml`という名前の新しいファイルを作成します。 次の内容でファイルを更新します。を次のXMLに置き換えます。 `.content.xml`を開き、下のXMLをコピーして貼り付けるのが最も簡単です。

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

   前の&#x200B;**ポリシーダイアログ** XMLの基本は、[コアコンポーネントイメージコンポーネント](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml)から取得されました。

   ダイアログ設定と同様に、[Sling Resource Marge](https://sling.apache.org/documentation/bundles/resource-merger.html)は、`sling:hideResource="{Boolean}true"`プロパティを持つノード定義で見られるように、`sling:resourceSuperType`から継承される無関係なフィールドを非表示にするために使用されます。

### コードのデプロイ {#deploy-the-code}

1. Mavenのスキルを使用して、更新したコードベースをローカルのAEMインスタンスにデプロイします。

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

## ペ追加ージ{#add-the-component-to-a-page}のコンポーネント

AEMコンポーネントの開発に重点を置いたシンプルな作業を維持するために、記事ページに現在の状態のBylineコンポーネントを追加し、`cq:Component`ノード定義がデプロイされて正しいことを確認します。AEMは新しいコンポーネント定義を認識し、コンポーネントのダイアログはオーサリングのために機能します。

### AEM Assets追加のイメージ

まず、サンプルのヘッドショットをAEM Assetsにアップロードし、Bylineコンポーネントの画像の設定に使用します。

1. AEM AssetsのLA Skateparksフォルダーに移動します。[http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)**&#x200B;のヘッドショットをフォルダーにアップロードします。

   ![ヘッドショットがアップロードされました](assets/custom-component/stacey-roswell-headshot-assets.png)

### コンポーネントの作成 {#author-the-component}

次に、AEMのページにBylineコンポーネントを追加します。 Bylineコンポーネントは&#x200B;**WKNDサイトプロジェクト — コンテンツ**&#x200B;コンポーネントグループに追加されたので、`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml`定義を介して、**ポリシー**&#x200B;で&#x200B;**WKNDサイトプロジェクト — コンテンツが自動的に使用できます/> Article Pageのレイアウトコンテナが含まれるコンポーネントグループ。******

1. 次のLA Skatepark記事に移動します。[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. 左側のサイドバーから、開いている記事ページのレイアウトコンテナの&#x200B;**署名コンポーネント**&#x200B;を&#x200B;**下**&#x200B;にドラッグ&amp;ドロップします。

   ![bylineコンポーネントをページに追加](assets/custom-component/add-to-page.png)

1. **左側のサイドバーが開いて**&#x200B;表示されたことと、**アセットファインダー**&#x200B;が選択されていることを確認します。

   ![アセットファインダーを開く](assets/custom-component/open-asset-finder.png)

1. **署名コンポーネントプレースホルダー**&#x200B;を選択します。これにより、アクションバーが表示され、**レンチ**&#x200B;アイコンをタップしてダイアログが開きます。

   ![コンポーネントアクションバー](assets/custom-component/action-bar.png)

1. ダイアログを開き、最初のタブ（アセット）をアクティブにして、左側のサイドバーを開き、アセットファインダーから画像を画像ドロップゾーンにドラッグします。 WKND ui.contentパッケージで提供されるStacey Roswellsのプロフィールの画像を探すには、「stacey」を探します。

   ![ダイアログに画像を追加](assets/custom-component/add-image.png)

1. 画像を追加したら、「**プロパティ**」タブをクリックして「**名前**」および「**職業**」に入力します。

   職業を入力する際は、**逆アルファベット**&#x200B;の順に入力すると、Slingモデルで実装する業務ロジックがすぐに明らかになります。

   右下の「**完了**」ボタンをタップして、変更を保存します。

   ![bylineコンポーネントのプロパティを入力](assets/custom-component/add-properties.png)

   AEM作成者は、ダイアログを使用してコンポーネントを設定およびオーサリングします。 Bylineコンポーネントの開発のこの時点では、データ収集用のダイアログが含まれていますが、オーサリングされたコンテンツをレンダリングするロジックはまだ追加されていません。 したがって、プレースホルダーのみが表示されます。

1. ダイアログを保存した後、[CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline)に移動し、コンポーネントのコンテンツがAEMページの下のbylineコンポーネントのコンテンツノードにどのように保存されるかを確認します。

   「LA Skate Parks」ページの下のBylineコンポーネントのコンテンツノード(`/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`)を探します。

   `name`、`occupations`、`fileReference`の各プロパティ名は、**バイラインノード**&#x200B;に保存されています。

   また、ノードの`sling:resourceType`が`wknd/components/content/byline`に設定されていることに注意してください。これは、このコンテンツノードをBylineコンポーネントの実装に連結するものです。

   ![CRXDEのbylineプロパティ](assets/custom-component/byline-properties-crxde.png)

## 署名スリングモデルの作成{#create-sling-model}

次に、データモデルとして機能し、署名コンポーネントのビジネスロジックを格納する Sling Model を作成します。

Slingモデルは、JCRからJava変数へのデータのマッピングを容易にし、AEMのコンテキストで開発する際に、様々な点を提供する注釈駆動のJava 「POJO」(Plain Old Java Objects)です。

### Mavenの依存関係の確認{#maven-dependency}

Byline Slingモデルは、AEMが提供する複数のJava APIに依存します。 これらのAPIは、`core`モジュールのPOMファイルに記載されている`dependencies`を通じて入手できます。 このチュートリアルで使用するプロジェクトは、AEM用にCloud Serviceとして構築されています。 ただし、AEM 6.5/6.4との下位互換性がある点で一意です。したがって、Cloud ServiceとAEM 6.xの両方の依存関係が含まれます。

1. `<src>/aem-guides-wknd/core/pom.xml`の下の`pom.xml`ファイルを開きます。
1. `aem-sdk-api` - **AEMの依存関係をCloud Serviceとして探す**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#building-for-the-sdk)には、AEMで公開されているパブリックJava APIがすべて含まれています。 `aem-sdk-api`は、このプロジェクトの構築時にデフォルトで使用されます。 バージョンは、`aem-guides-wknd/pom.xml`にあるプロジェクトのルートにある親リアクタのpomに保持されます。

1. `uber-jar` - **AEM 6.5/6.4のみ**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   `uber-jar`は、`classic`プロファイルが呼び出された場合（`mvn clean install -PautoInstallSinglePackage -Pclassic`など）にのみ含まれます。 これはこのプロジェクトに固有のものです。 AEMプロジェクトのアーキタイプから生成された現実世界のプロジェクトでは、AEMのバージョンが6.5または6.4の場合、`uber-jar`がデフォルトになります。

   [uber-jar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies)には、AEM 6.xで公開されているパブリックJava APIがすべて含まれています。バージョンは、プロジェクト`aem-guides-wknd/pom.xml`のルートにある親リアクタpomに保持されます。

1. `core.wcm.components.core`の依存関係を探します。

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   これは、AEMコアコンポーネントで公開されているすべてのパブリックJava APIです。 AEMコアコンポーネントは、AEMの外部で管理されるプロジェクトなので、別のリリースサイクルを持ちます。 このため、依存関係は別々に含める必要があり、`uber-jar`や`aem-sdk-api`には含めない&#x200B;**です。**

   uber-jarと同様、この依存関係のバージョンは`aem-guides-wknd/pom.xml`にある親リアクタpomファイルに保持されます。

   このチュートリアルの後半では、Core Component Imageクラスを使用して、Bylineコンポーネントに画像を表示します。 Slingモデルを構築してコンパイルするには、コアコンポーネントの依存関係が必要です。

### Byline インターフェイス {#byline-interface}

署名用の公開Javaインターフェイスを作成します。 `Byline.java` は、 `byline.html` HTLスクリプトを駆動するのに必要なパブリックメソッドを定義します。

1. `aem-guides-wknd.core`モジュール内の`core/src/main/java/com/adobe/aem/guides/wknd/core/models`の下に、`Byline.java`という名前の新しいファイルを作成します

   ![バイラインインターフェイスの作成](assets/custom-component/create-byline-interface.png)

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

   最初の2つのメソッドは、Bylineコンポーネントの&#x200B;**name**&#x200B;および&#x200B;**occumentations**&#x200B;の値を公開します。

   `isEmpty()`メソッドは、コンポーネントにレンダリングするコンテンツがあるか、設定待ちかを判断するために使用します。

   イメージに対するメソッドがないことに注意してください。[なぜ後で](#tackling-the-image-problem)なるのかを見てみよう。

### 署名実装 {#byline-implementation}

`BylineImpl.java` は、前に定義したインター `Byline.java` フェイスを実装するSlingモデルの実装です。`BylineImpl.java` の完全なコードは、この節の最後に記載しています。

1. `core/src/main/java/com/adobe/aem/guides/core/models`の下に`impl`という名前の新しいフォルダーを作成します。
1. `impl`フォルダーに新しいファイル`BylineImpl.java`を作成します。

   ![Byline Impl File](assets/custom-component/byline-impl-file.png)

1. 開く `BylineImpl.java`. `Byline`インターフェイスが実装されていることを指定します。 IDEのオートコンプリート機能を使用するか、または手動でファイルを更新し、`Byline`インタフェースの実装に必要なメソッドを含めます。

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

1. 以下のクラスレベルの注釈で `BylineImpl.java` を更新して、Sling Model 注釈を追加します。この`@Model(..)`注釈は、クラスをSlingモデルに変換します。

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
       protected static final String RESOURCE_TYPE = "wknd/components/content/byline";
       ...
   }
   ```

   この注釈とそのパラメーターを確認してみましょう:

   * `@Model`注釈は、BylineImplをAEMに展開するときにSlingモデルとして登録します。
   * `adaptables`パラメーターは、このモデルが要求に応じて適応できることを指定します。
   * `adapters`パラメーターを使用すると、実装クラスをBylineインターフェイスに登録できます。 これにより、HTLスクリプトは、（implを直接呼び出すのではなく）インターフェイスを介してSlingモデルを呼び出すことができます。 adapters について詳しくは、[こちら](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110)を参照してください。
   * `resourceType`は、Bylineコンポーネントのリソースタイプ（以前に作成した）を指し、複数の実装がある場合は、正しいモデルの解決に役立ちます。 モデルクラスのリソースタイプとの関連付けについて詳しくは、[こちら](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130)を参照してください。

### Sling Model メソッドの実装  {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

最初に取り組む方法は`getName()`です。これは`name`プロパティの下にあるバイラインのJCRコンテンツノードに格納された値を返すだけです。

この場合、`@ValueMapValue` Sling Model注釈を使用して、要求のリソースのValueMapを使用してJavaフィールドに値を挿入します。


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

JCRプロパティはJavaフィールドと同じ名前（両方とも「name」です）を共有するので、`@ValueMapValue`は自動的にこの関連付けを解決し、プロパティの値をJavaフィールドに挿入します。

#### getOccuptions() {#implementing-get-occupations}

次に実装する方法は`getOccupations()`です。 このメソッドは、JCRプロパティ`occupations`に格納されたすべての職業を収集し、ソートされた（アルファベット順）コレクションを返します。

`getName()`で説明したのと同じ方法を使用して、プロパティ値をSlingモデルのフィールドに挿入できます。

挿入されたJavaフィールド`occupations`を介してJCRプロパティ値がSlingモデルで使用可能になったら、`getOccupations()`メソッドで並べ替えビジネスロジックを適用できます。


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

最後のパブリックメソッドは`isEmpty()`です。これは、コンポーネントがレンダリングに十分に「オーサリングされた」と見なすタイミングを決定します。

このコンポーネントに関しては、*コンポーネントをレンダリングする前に、名前、画像、職業の3つのフィールドをすべて*&#x200B;入力する必要があるというビジネス要件があります。


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


#### 「画像の問題」に取り組む{#tackling-the-image-problem}

名前と占有条件のチェックは簡単です（Apache Commons Lang3は常に便利な[StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html)クラスを提供します）が、イメージの表示にコアコンポーネントイメージコンポーネントが使用されているので、Image **の**&#x200B;の存在を検証する方法は不明です。

これに対処するには、2 つの方法があります。

`fileReference` JCRプロパティがアセットに解決されるかどうかを確認します。 *ORC* このリソースをコアコンポーネントの画像スリングモデルに変換し、 `getSrc()` メソッドが空でないことを確認します。

**2番目の**&#x200B;アプローチを選択します。 最初のアプローチは十分と思われますが、このチュートリアルでは、Slingモデルの他の機能を調べるのに後者を使用します。

1. 画像を取得するプライベートメソッドを作成します。 このメソッドをプライベートにしておくのは、HTL 自体の画像オブジェクトを公開する必要がなく、`isEmpty().` () を実行するためにのみ使用するからです。

   `getImage()`の次のプライベートメソッド：

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   上記のように、**Image Sling Model**&#x200B;を取得する方法がさらに2つあります。

   最初のは`@Self`注釈を使用し、現在のリクエストをコアコンポーネントの`Image.class`に自動的に適応させます。

   ```java
   @Self
   private Image image;
   ```

   2つ目は[Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGiサービスを使用しています。これは非常に便利なサービスで、Javaコードで他のタイプのSlingモデルを作成するのに役立ちます。

   第2のアプローチを選択します。

   >[!NOTE]
   >
   >実際の実装では、`@Self`を使用する方が「One」というアプローチが望まれます。これは、よりシンプルでエレガントなソリューションです。 このチュートリアルでは、2つ目のアプローチを使用します。非常に役立つSlingモデルのファセットをさらに検討する必要があるので、より複雑なコンポーネントを使用します。

   SlingモデルはOSGiサービスではなくJava POJOなので、通常のOSGi挿入注釈`@Reference` **は使用できません**。代わりに、Slingモデルは、類似した機能を提供する特別な&#x200B;**[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)**&#x200B;注釈を提供します。

1. `BylineImpl.java`を更新し、`ModelFactory`を挿入する`OSGiService`注釈を含めます。

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

   `ModelFactory`を使用して、次を使用してコアコンポーネントの画像スリングモデルを作成できます。

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   ただし、このメソッドには要求とリソースの両方が必要で、Slingモデルではまだ使用できません。 これらを取得するには、より多くのSlingモデル注釈が使用されます。

   現在のリクエストを取得するには、**[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)**&#x200B;注釈を使用して、`adaptable`（`@Model(..)`で`SlingHttpServletRequest.class`として定義されている）をJavaクラスフィールドに挿入します。

1. 追加&#x200B;**@Self**&#x200B;注釈。**SlingHttpServletRequest**&#x200B;を取得します。

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   `@Self Image image`を使用してコアコンポーネント画像Slingモデルを挿入する方法は、上記のオプションでした。`@Self`注釈は、順応性のあるオブジェクト（この例ではSlingHttpServletRequest）を挿入し、注釈フィールドのタイプに合わせて調整します。 コアコンポーネントの画像 Sling Model は SlingHttpServletRequest オブジェクトから適応可能なので、これなら機能したはずです。また、説明的な 2 番目の方法よりも少ないコードで済みます。

   これで、ModelFactory API で画像モデルをインスタンス化するために必要な変数を挿入できました。次に、Sling Model の **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods) 注釈を使用して、Sling Model をインスタンス化した後、このオブジェクトを取得します。**

   `@PostConstruct` は非常に役に立ち、コンストラクターと同様の機能を持ちますが、クラスをインスタンス化し、注釈を付けたすべてのJavaフィールドを挿入した後に呼び出されます。他のSlingモデル注釈はJavaクラスフィールド（変数）に注釈を付けるのに対し、`@PostConstruct`はvoidでゼロのパラメータメソッドを注釈付けします。通常は`init()`という名前です（ただし、名前は任意に指定できます）。

1. 追加&#x200B;**@PostConstruct**&#x200B;メソッド：

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

   Sling Model は OSGi サービス&#x200B;**ではない**&#x200B;ので、クラスの状態を安全に管理できます。`@PostConstruct`は、通常のコンストラクタと同じように、後で使用するためにSling Modelクラスの状態を導き出し、設定します。

   `@PostConstruct`メソッドが例外をスローした場合、Slingモデルはインスタンス化されません（nullになります）。

1. **getImage()** を更新して、単にイメージオブジェクトを返すことができるようになりました。

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. `isEmpty()`に戻り、実装を終了します。

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

   注意：`getImage()`への複数の呼び出しは、初期化された`image`クラス変数を返すため問題はありません。また、`modelFactory.getModelFromWrappedRequest(...)`を呼び出すのはあまり高くはありませんが、不必要に呼び出すのを避ける価値があります。

1. 最終的な`BylineImpl.java`は次のようになります。


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
       protected static final String RESOURCE_TYPE = "wknd/components/content/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       @PostConstruct
       private void init() {
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

`ui.apps`モジュールで、AEMコンポーネントの以前のセットアップで作成した`/apps/wknd/components/byline/byline.html`を開きます。

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

この HTL スクリプトでおこなうことを確認しましょう。

* `placeholderTemplate`は、コアコンポーネントのプレースホルダーを指し、コンポーネントが完全に設定されていない場合に表示されます。 これは、AEM Sitesページエディターで、`cq:Component`の`jcr:title`プロパティで上記の定義に従って、コンポーネントのタイトルを持つボックスとしてレンダリングされます。

* `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}`は、上記に定義した`placeholderTemplate`を読み込み、ブール値（現在は`false`にハードコードされています）をプレースホルダーテンプレートに渡します。 `isEmpty`がtrueの場合、プレースホルダテンプレートはグレーのボックスをレンダリングし、trueでない場合は何もレンダリングしません。

### 署名HTLの更新

1. 以下のスケルタル HTML 構造で **byline.html** を更新します。

   ```html
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!-- Include the Core Components Image Component -->
           </div>
           <h2 class="cmp-byline__name"><!-- Include the name --></h2>
           <p class="cmp-byline__occupations"><!-- Include the occupations --></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   CSS クラスは [BEM 命名規則](https://getbem.com/naming/)に従うことに注意してください。BEM 規則の使用は必須ではありませんが、コアコンポーネント CSS クラスで使用され、一般的にクリーンで読みやすい CSS ルールになるので、BEM をお勧めします。

### HTL での Sling Model オブジェクトのインスタンス化  {#instantiating-sling-model-objects-in-htl}

[Use block statement](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use)は、HTLスクリプト内のSlingモデルオブジェクトをインスタンス化し、HTL変数に割り当てるために使用します。

`data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` は、BylineImplによって実装されるBylineインターフェイス(com.adobe.aem.guides.wknd.models.Byline)を使用し、現在のSlingHttpServletRequestを調整し、結果をbyline (  `data-sly-use.<variable-name>`)というHTL変数名に格納します。

1. 外側の`div`を更新して、**Byline** Slingモデルをパブリックインターフェイスで参照します。

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Sling Model メソッドへのアクセス {#accessing-sling-model-methods}

HTLはJSTLから参照し、Java getterメソッド名と同じ短縮形を使用します。

例えば、Byline Slingモデルの`getName()`メソッドを呼び出す場合は、`byline.isEmpty`と同様に`byline.name`に短縮できます。同様に、`byline.empty`に短縮できます。 完全なメソッド名`byline.getName`または`byline.isEmpty`を使用することもできます。 `()`は、HTLでのメソッドの呼び出しには使用されないことに注意してください（JSTLと同様）。

パラメーター&#x200B;**を必要とするJavaメソッドは、HTLでは使用できません。**&#x200B;これは、HTL のロジックをシンプルにするための設計によるものです。

1. 署名名は、Byline Slingモデルの`getName()`メソッドを呼び出すか、HTLで追加することで、コンポーネントに追加できます。`${byline.name}`.

   `h2`タグの更新：

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### HTL 式のオプションの使用 {#using-htl-expression-options}

[HTL式](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) オプションは、HTL内のコンテンツに対する修飾子として機能し、日付の書式設定からi18nへの変換の範囲を持ちます。また、式は、リストの結合や値の配列に使用でき、職業をコンマ区切り形式で表示するのに必要です。

式は、HTL式ーの`@`演算子を介して追加されます。

1. 職業のリストを「,」で結合するには、以下のコードを使用します。

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### プレースホルダーの条件付き表示  {#conditionally-displaying-the-placeholder}

AEM Components用のほとんどのHTLスクリプトは、**プレースホルダーパラダイム**&#x200B;を利用して、コンポーネントが誤って作成され、AEM Publish **に表示されないことを示す視覚的なキューを作成者に提供します。**&#x200B;この判断を推進するには、コンポーネントの背後の Sling Model のメソッド（この場合 `Byline.isEmpty()`()）を実装する必要があります。

`isEmpty()` がByline Slingモデルで呼び出され、結果(または `!` 演算子を介した負の値)が次のHTL変数に保存され `hasContent`ます。

1. 外側の`div`を更新して、`hasContent`という名前のHTL変数を保存します。

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   `data-sly-test`を使うと、HTL `test`ブロックは、HTL変数を設定し、HTL式の結果が真かどうかに基づいて、HTML要素をレンダリング/レンダリングしないという点でおもしろいです。 「truthy」の場合、HTML要素はレンダリングされ、そうでない場合はレンダリングされません。

   このHTL変数`hasContent`は、プレースホルダーを条件付きで表示/非表示にするために再利用できるようになりました。

1. ファイルの下部にある`placeholderTemplate`への条件付き呼び出しを、次の内容で更新します。

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### コアコンポーネントを使用した画像の表示{#using-the-core-components-image}

`byline.html`のHTLスクリプトは現在、ほとんど完了しており、画像が見つかりません。

コアコンポーネントの画像コンポーネント `sling:resourceSuperType` を使用して画像のオーサリングを提供しているので、コアコンポーネントの画像コンポーネントを使用して画像をレンダリングすることもできます。

この場合、現在の署名リソースを含める必要がありますが、リソースタイプ `core/wcm/components/image/v2/image`. これは、コンポーネントを再利用するための強力なパターンです。 この場合、HTLの`data-sly-resource`ブロックが使用されます。

1. `div`を`cmp-byline__image`のクラスに置き換えます。

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   この`data-sly-resource`は、相対パス`'.'`を介して現在のリソースを含め、現在のリソース（またはバイラインコンテンツリソース）を強制的に`core/wcm/components/image/v2/image`のリソースタイプに含めます。

   コアコンポーネントのリソースタイプは、スクリプト内で使用され、コンテンツに対して保持されないので、プロキシ経由ではなく直接使用されます。

2. `byline.html`を完了：

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

3. コードベースをローカルの AEM インスタンスにデプロイします。POMファイルに大幅な変更が加えられたので、プロジェクトのルートディレクトリから完全なMavenビルドを実行します。

   ```shell
   $ cd aem-guides-wknd/
   $ mvn clean install -PautoInstallSinglePackage
   ```

   AEM 6.5/6.4にデプロイする場合は、`classic`プロファイルを起動します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

### スタイル設定を解除したBylineコンポーネントの確認{#reviewing-the-unstyled-byline-component}

1. アップデートを展開した後、『Ultimate Guide to LA Skateparks ](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)』ページに移動するか、この章の前半でBylineコンポーネントを追加した任意の場所に移動します。[

1. **画像**、**名前**、**職業**&#x200B;が現れ、スタイルは設定されていませんが、Bylineコンポーネントが動作しています。

   ![非スタイル化バイラインコンポーネント](assets/custom-component/unstyled.png)

### Sling Model 登録の確認 {#reviewing-the-sling-model-registration}

[AEM Web コンソールの Sling Models Status 表示](http://localhost:4502/system/console/status-slingmodels)には、AEM に登録されたすべての Sling Model が表示されます。署名 Sling Model は、このリストを確認することで、インストールされ、認識されていることを検証できます。

**BylineImpl**&#x200B;がこのリストに表示されない場合、Slingモデルの注釈に問題があったか、Slingモデルがコアプロジェクトの登録済みSlingモデルパッケージ(com.adobe.aem.guides.wknd.core.models)に追加されなかった可能性があります。

![Byline Slingモデルが登録されました](assets/custom-component/osgi-sling-models.png)

*http://localhost:4502/system/console/status-slingmodels*

## 署名のスタイル {#byline-styles}

署名コンポーネントは、署名コンポーネントのクリエイティブデザインに沿ってスタイルを設定する必要があります。これは、SCSSを使用して行います。AEMは、**ui.frontend** Mavenサブプロジェクトを介してをサポートします。

### 追加デフォルトのスタイル

署名コンポーネントのデフォルトスタイルを追加します。`/src/main/webpack/components`の下の&#x200B;**ui.frontend**&#x200B;プロジェクト：

1. `_byline.scss`という名前の新しいファイルを作成します。

   ![byline project explorer](assets/custom-component/byline-style-project-explorer.png)

1. Byline実装追加CSS（SCSSとして書き込まれる）を`default.scss`に渡します。

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

1. `main.scss`を`ui.frontend/src/main/webpack/site/main.scss`で確認：

   ```scss
   @import 'variables';
   @import 'wkndicons';
   @import 'base';
   @import '../components/**/*.scss';
   @import './styles/*.scss';
   ```

   `main.scss` は、 `ui.frontend` モジュールによって含まれるスタイルのメインエントリポイントです。正規式`'../components/**/*.scss'`には、`components/`フォルダーの下にあるすべてのファイルが含まれます。

1. 完全なプロジェクトを構築してAEMにデプロイします。

   ```shell
   $ cd aem-guides-wknd/
   $ mvn clean install -PautoInstallSinglePackage
   ```

   AEM 6.4/6.5を使用している場合は、`-Pclassic`プロファイルを追加します。

   >[!TIP]
   >
   >古いCSSが提供されていないことを確認するためにブラウザーのキャッシュをクリアし、Bylineコンポーネントでページを更新してフルスタイルを取得する必要がある場合があります。

## まとめ {#putting-it-together}

以下に、完全に作成され、スタイル設定された署名コンポーネントが AEM ページでどのように見えるかを示します。

![完成したバイラインコンポーネント](assets/custom-component/final-byline-component.png)

## バリデーターが{#congratulations}

Adobe Experience Managerを使用してカスタムコンポーネントをゼロから作成しました。

### 次の手順 {#next-steps}

引き続きAEMコンポーネントの開発について学習します。Byline JavaコードのJUnitテストを作成して、すべてが正しく開発され、実装されたビジネスロジックが正しく完了していることを確認します。

* [ユニットテストまたはAEMコンポーネントの作成](unit-testing.md)

[GitHub](https://github.com/adobe/aem-guides-wknd)上の完了したコードを表示するか、Gitブラック`tutorial/custom-component-solution`上のローカルにコードを確認して展開します。

1. [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd)リポジトリをコピーします。
1. `tutorial/custom-component-solution`ブランチをチェックアウト
