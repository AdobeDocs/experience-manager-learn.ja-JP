---
title: コンポーネントの拡張 | AEM SPAエディターとReactの使い始めに
description: AEM SPAエディターで使用する既存のコアコンポーネントを拡張する方法を説明します。 既存のコンポーネントにプロパティやコンテンツを追加する方法を理解することは、AEM SPA Editorの実装機能を拡張する強力なテクニックです。 Sling Resource MangerのSlingモデルと機能を拡張するための委任パターの使用方法を説明します。
sub-product: サイト
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5879
thumbnail: 5879-spa-react.jpg
translation-type: tm+mt
source-git-commit: ff75a9d10e9d00510e4c49dea0dcc36e68ca46c4
workflow-type: tm+mt
source-wordcount: '1969'
ht-degree: 2%

---


# コアコンポーネントの拡張 {#extend-component}

AEM SPAエディターで使用する既存のコアコンポーネントを拡張する方法を説明します。 既存のコンポーネントの拡張方法を理解することは、AEM SPAエディタの実装機能をカスタマイズおよび拡張する強力な手法です。

## 目的

1. 追加のプロパティとコンテンツを使用して、既存のコアコンポーネントを拡張します。
2. コンポーネント継承の基本を、を使用して理解し `sling:resourceSuperType`ます。
3. Slingモデルの [委任パターン](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) (Delegation Pattern for Sling Models)を活用して、既存のロジックと機能を再利用する方法を説明します。

## 作成する内容

この章では、新しい `Card` コンポーネントが作成されます。 この `Card` コンポーネントは、 [](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/components/image.html) Image Coreコンポーネントを拡張して、「タイトル」や「アクションの呼び物」ボタンなどの追加のコンテンツフィールドを追加し、SPA内の他のコンテンツに対するティーザーの役割を実行します。

![カードコンポーネントの最終オーサリング](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> 実際の実装では、Teaserコンポーネントを単に使用し [、プロジェクトの要件に応じて](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/teaser.html) Image Core Component [(](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/components/image.html)`Card` Image Core Component)を拡張してコンポーネントを作成する方が適切な場合があります。 可能な場合は、常に [コアコンポーネントを直接使用することをお勧めします](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html) 。

## 前提条件

Review the required tooling and instructions for setting up a [local development environment](overview.md#local-dev-environment).

### コードの取得

1. Gitを介して、このチュートリアルのスタートポイントをダウンロードします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/extend-component-start
   ```

2. Mavenを使用して、ローカルのAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   AEM 6.x [を使用している場合](overview.md#compatibility) 、 `classic` プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 従来の [WKNDリファレンスサイト用に完成したパッケージをインストールし](https://github.com/adobe/aem-guides-wknd/releases/latest)ます。 WKNDリファレンスサイトから提供される [画像は](https://github.com/adobe/aem-guides-wknd/releases/latest) 、WKND SPAで再利用されます。 パッケージは、 [AEM Package Managerを使用してインストールできます](http://localhost:4502/crx/packmgr/index.jsp)。

   ![Package Managerのインストールwknd.all](./assets/map-components/package-manager-wknd-all.png)

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/React/extend-component-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `React/extend-component-solution`ます。

## Inspectの最初のカードの実装

チャプタースターターコードによって、最初のカードコンポーネントが提供されました。 Inspectは、カードの実装の起点です。

1. In the IDE of your choice open the `ui.apps` module.
2. ファイルに移動し `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/card` て表示し `.content.xml` ます。

   ![カードコンポーネントのAEM定義開始](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   このプロパティ `sling:resourceSuperType` は、WKND SPA Imageコンポーネントからすべての機能を `wknd-spa-react/components/image``Card` コンポーネントが継承することを示します。

3. Inspectファイル `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   が指す点に注意し `sling:resourceSuperType` てくだ `core/wcm/components/image/v2/image`さい。 これは、WKND SPAイメージコンポーネントが、コアコンポーネントイメージからすべての機能を継承していることを示しています。

   プロ [キシパターン](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) Slingリソース継承とも呼ばれるのは、子コンポーネントに機能を継承させ、必要に応じて動作を拡張/オーバーライドできる強力な設計パターンです。 Sling継承は複数レベルの継承をサポートするので、最終的には新しい `Card` コンポーネントはコアコンポーネントイメージの機能を継承します。

   多くの開発チームはDRYになろうと努力しています（繰り返さないでください）。 AEMでは、Slingの継承が可能です。

4. フォルダーの下で `card` 、ファイルを開き `_cq_dialog/.content.xml`ます。

   このファイルは、コンポーネントのコンポーネントダイアログ定義で `Card` す。 Slingの継承を使用する場合は、Sling Resource Margeの機能を使用して、ダイアログの一部を上書きまたは拡張する [ことができます](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html) 。 このサンプルでは、作成者から追加のデータを取り込み、カードコンポーネントに入力するための新しいタブがダイアログに追加されています。

   新しいタブやフォームフィールドを挿入する場所を開発者が選択でき `sling:orderBefore` るなどのプロパティ。 この場合、 `Text` タブはタブの前に挿入され `asset` ます。 Sling Resource Margeをフルに活用するには、 [イメージコンポーネントダイアログの元のダイアログノード構造を知ることが重要です](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml)。

5. フォルダーの下で `card` 、ファイルを開き `_cq_editConfig.xml`ます。 このファイルは、AEMオーサリングUIでのドラッグ&amp;ドロップの動作を指示します。 Imageコンポーネントを拡張する場合、リソースタイプがコンポーネント自体と一致することが重要です。 ノードを確認し `<parameters>` ます。

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   ほとんどのコンポーネントでは、を必要としません。Imageコンポーネント `cq:editConfig`とImageコンポーネントの子孫は例外です。

6. IDEスイッチで `ui.frontend` モジュールに移動し、次の場所に移動し `ui.frontend/src/components/Card`ます。

   ![反応成分開始](assets/extend-component/react-card-component-start.png)

7. ファイルをInspect `Card.js`。

   標準の `Card``MapTo` 関数を使用してAEMコンポーネントにマップするために、コンポーネントは既にスタブアウトされています。

   ```js
   MapTo('wknd-spa-react/components/card')(Card, CardEditConfig);
   ```

8. Inspect: `get imageContent()`

   ```js
    get imageContent() {
       return (
           <div className="Card__image">
               <Image {...this.props} />
           </div>)
   }
   ```

   この例では、コンポーネントからを渡すだけで、既存のReact Imageコンポーネント `Image` を再利用す `this.props``Card` ることを選択しました。 チュートリアルの後半で、タイトル、日付、コールトゥアクションボタンを表示する `get bodyContent()` メソッドが導入されます。

## テンプレートポリシーの更新

この最初の `Card` 導入で、AEM SPAエディタの機能を確認します。 初期コンポーネントを確認するには、テンプレートポリシーを更新する必要があります。 `Card`

1. スターターコードをAEMのローカルインスタンスにデプロイします（まだデプロイしていない場合）。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigate to the SPA Page Template at [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
3. レイアウトコンテナのポリシーを更新し、新しいコンポー `Card` ネントを許可されたコンポーネントとして追加します。

   ![レイアウトコンテナポリシーの更新](assets/extend-component/card-component-allowed.png)

   ポリシーに対する変更を保存し、そのコンポーネントを許可されたコンポーネントとして確認し `Card` ます。

   ![許可されたコンポーネントとしてのカードコンポーネント](assets/extend-component/card-component-allowed-layout-container.png)

## 作成者の初期カードコンポーネント

次に、AEM SPAエディタを使用して `Card` コンポーネントをオーサリングします。

1. http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.htmlに移動し [ます](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。
2. モ `Edit` ードで、コンポー `Card` ネントを次の場所に追加し `Layout Container`ます。

   ![新しいコンポーネントの挿入](assets/extend-component/insert-card-component.png)

3. Drag and drop an image from the Asset finder onto the `Card` component:

   ![追加画像](assets/extend-component/card-add-image.png)

4. コンポーネントダイアログを開き、「 `Card` テキスト **** 」タブが追加されていることを確認します。
5. 「 **テキスト** 」タブに次の値を入力します。

   ![「テキストコンポーネント」タブ](assets/extend-component/card-component-text.png)

   **カードパス** - SPAホームページの下のページを選択します。

   **CTA Text** - &quot;Read More&quot;

   **カードタイトル** — 空白のままにします

   **リンクされたページからタイトルを取得** — 真を示すチェックボックスをオンにします。

6. 「 **アセットメタデータ** 」タブを更新し、 **代替テキスト** と ****&#x200B;キャプションの値を追加します。

   現在、ダイアログを更新した後は、追加の変更は表示されません。 新しいフィールドをReact Componentに公開するには、コンポーネントのSlingモデルを更新する必要があり `Card` ます。

7. 新しいタブを開き、 [CRXDE-Liteに移動します](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-react/us/en/home/jcr%3Acontent/root/responsivegrid/card)。 下のコンテンツノードをInspect `/content/wknd-spa-react/us/en/home/jcr:content/root/responsivegrid` して、コンポー `Card` ネントのコンテンツを見つけます。

   ![CRXDE-Liteコンポーネントのプロパティ](assets/extend-component/crxde-lite-properties.png)

   プロパティ `cardPath`、 `ctaText`、がダイアログで持続するこ `titleFromPage` とを確認します。

## カードスリングモデルの更新

最終的にコンポーネントダイアログの値をReactコンポーネントに公開するには、コンポーネントのJSONを入力するSlingモデルを更新する必要があり `Card` ます。 また、次の2つのビジネスロジックを導入する機会もあります。

* true `titleFromPage` の場合、で指定されたページのタイトルを返します **。それ以外の場合は、**`cardPath``cardTitle` textfieldの値を返します。
* で指定されたページの最終変更日を返し `cardPath`ます。

選択したIDEに戻り、 `core` モジュールを開きます。

1. Open the file `Card.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/Card.java`.

   インター `Card` フェイスが現在拡張され `com.adobe.cq.wcm.core.components.models.Image` ているので、インター `Image` フェイスのすべてのメソッドを継承していることを確認してください。 この `Image``ComponentExporter` インターフェイスは既にインターフェイスを拡張しており、SlingモデルをJSONとして書き出し、SPAエディターでマッピングできます。 したがって、「 `ComponentExporter` カスタムコンポーネント」の章と同様に、明示的にインター [フェイスを拡張する必要はありません](custom-component.md)。

2. イン追加ターフェイスに対する次のメソッド：

   ```java
   @ProviderType
   public interface Card extends Image {
   
       /***
       * The URL to populate the CTA button as part of the card.
       * The link should be based on the cardPath property that points to a page.
       * @return String URL
       */
       public String getCtaLinkURL();
   
       /***
       * The text to display on the CTA button of the card.
       * @return String CTA text
       */
       public String getCtaText();
   
   
   
       /***
       * The date to be displayed as part of the card.
       * This is based on the last modified date of the page specified by the cardPath
       * @return
       */
       public Calendar getCardLastModified();
   
   
       /**
       * Return the title of the page specified by cardPath if `titleFromPage` is set to true.
       * Otherwise return the value of `cardTitle`
       * @return
       */
       public String getCardTitle();
   }
   ```

   これらのメソッドは、JSONモデルAPIを介して公開され、Reactコンポーネントに渡されます。

3. 開く `CardImpl.java`. これは、インター `Card.java` フェイスの実装です。 この実装は、チュートリアルの速度を上げるために既に部分的に骨格化されています。  SlingモデルがSlingモデルエクスポーターを介してJSONとしてシリアル化できるようにするために、 `@Model``@Exporter` および注釈が使用されています。

   `CardImpl.java` また、Imageコアコンポーネントからすべてのロジックが書き換えられないように、Slingモデルの [委任パターンも使用します](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 。

4. 次の行を確認します。

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   上記の注釈は、コンポー `image` ネントの `sling:resourceSuperType``Card` 継承に基づいて名前が付けられたImageオブジェクトをインスタンス化します。

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   その後、単にこの `image` オブジェクトを使用して、自分でロジックを記述することなく、インター `Image` フェイスで定義されたメソッドを実装できます。 この手法は、および `getSrc()`に使用 `getAlt()` し `getTitle()`ます。

5. 次に、 `initModel()``cardPage` `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   Slingモデル `@PostConstruct initModel()` の初期化時には常にが呼び出されるので、モデル内の他のメソッドで使用されるオブジェクトを初期化する場合に適しています。 は、Slingモデル `pageManager` が [注釈を介して使用できるようにした、多数の](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects)`@ScriptVariable` Java対応グローバルオブジェクトの1つです。 getPage [メソッドはパスを取得し、AEM](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-) Page [](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html) オブジェクトを返します。パスが有効なページを参照しない場合はnullを返します。

   これにより、変数が初期化されます。この変数は、基になるリンクページに関するデータを返すために他の新しいメソッドで使用されます。 `cardPage`

6. 作成者ダイアログを保存したJCRプロパティに既にマッピングされているグローバル変数を確認します。 この `@ValueMapValue` 注釈は、マッピングを自動的に実行するために使用されます。

   ```java
   @ValueMapValue
   private String cardPath;
   
   @ValueMapValue
   private String ctaText;
   
   @ValueMapValue
   private boolean titleFromPage;
   
   @ValueMapValue
   private String cardTitle;
   ```

   これらの変数は、インター `Card.java` フェイスの追加メソッドの実装に使用されます。

7. インター `Card.java` フェイスで定義された追加のメソッドを実装します。

   ```java
   @Override
   public String getCtaLinkURL() {
       if(cardPage != null) {
           return cardPage.getPath() + ".html";
       }
       return null;
   }
   
   @Override
   public String getCtaText() {
       return ctaText;
   }
   
   @Override
   public Calendar getCardLastModified() {
      if(cardPage != null) {
          return cardPage.getLastModified();
      }
      return null;
   }
   
   @Override
   public String getCardTitle() {
       if(titleFromPage) {
           return cardPage != null ? cardPage.getTitle() : null;
       }
       return cardTitle;
   }
   ```

   >[!NOTE]
   >
   > ここで、 [完成したCardImpl.javaを表示できます](https://github.com/adobe/aem-guides-wknd-spa/blob/React/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CardImpl.java)。

8. ターミナルウィンドウを開き、ディレクトリのMaven `core` プロファイルを使用して、モジュールの更新だけを展開 `autoInstallBundle``core` します。

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   [AEM 6.xを使用している場合は](overview.md#compatibility) 、 `classic` プロファイルを追加します。

9. JSONモデルの応答の表示: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)`wknd-spa-react/components/card`and search for the:

   ```json
   "card": {
       "ctaText": "Read More",
       "cardTitle": "Page 1",
       "title": "Woman chillaxing with river views in Australian bushland",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/card.coreimg.jpeg/1595190732886/adobestock-216674449.jpeg",
       "alt": "Female sitting on a large rock relaxing in afternoon dappled light the Australian bushland with views over the river",
       "cardLastModified": 1591360492414,
       "ctaLinkURL": "/content/wknd-spa-react/us/en/home/page-1.html",
       ":type": "wknd-spa-react/components/card"
   }
   ```

   JSONモデルは、Slingモデルのメソッドを更新した後、追加のキー/値のペアで更新され `CardImpl` ます。

## 反応コンポーネントを更新

JSONモデルに、 `ctaLinkURL`、 `ctaText`、の新しいプロパティが入力されたので、Reactコンポーネント `cardTitle``cardLastModified` を更新してこれらのプロパティを表示できます。

1. IDEに戻り、 `ui.frontend` モジュールを開きます。 必要に応じて、新しいターミナルウィンドウからwebpack開発サーバを開始し、変更をリアルタイムで確認します。

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. を開 `Card.js` き `ui.frontend/src/components/Card/Card.js`ます。
3. 誘追加い文句（CTA：コールトゥアクション）をレンダリングするメソッド。 `get ctaButton()`

   ```js
   import {Link} from "react-router-dom";
   ...
   
   export default class Card extends Component {
   
       get ctaButton() {
           if(this.props && this.props.ctaLinkURL && this.props.ctaText) {
               return (
                   <div className="Card__action-container">
                       <Link to={this.props.ctaLinkURL} title={this.props.title}
                           className="Card__action-link">
                           {this.props.ctaText}
                       </Link>
                   </div>
               );
           }
   
           return null;
       }
       ...
   }
   ```

4. 日付を表すローカライズされた追加Stringに `get lastModifiedDisplayDate()` 変換 `this.props.cardLastModified` する方法。

   ```js
   export default class Card extends Component {
       ...
       get lastModifiedDisplayDate() {
           const lastModifiedDate = this.props.cardLastModified ? new Date(this.props.cardLastModified) : null;
   
           if (lastModifiedDate) {
               return lastModifiedDate.toLocaleDateString();
           }
           return null;
       }
       ...
   }
   ```

5. を更新 `get bodyContent()` して、前の手順で作成したメソッド `this.props.cardTitle` を表示および使用します。

   ```js
   export default class Card extends Component {
       ...
       get bodyContent() {
          return (<div class="Card__content">
                       <h2 class="Card__title"> {this.props.cardTitle}
                           <span class="Card__lastmod">
                               {this.lastModifiedDisplayDate}
                           </span>
                       </h2>
                       {this.ctaButton}
               </div>);
       }
       ...
   }
   ```

6. タイトルのスタイル設定、誘い文句（CTA：コールトゥアクション）、最終変更日の設定 `Card.scss` を行うために、に既にSassルールが追加されています。 ファイルの先頭に次の行を追加して、これらのスタイル `Card.js` を含めます。

   ```diff
     import {MapTo} from '@adobe/aem-react-editable-components';
   
   + require('./Card.scss');
   
     export const CardEditConfig = {
   ```

   >[!NOTE]
   >
   > 完成した [Reactカードコンポーネントコードをここで表示できます](https://github.com/adobe/aem-guides-wknd-spa/blob/React/extend-component-solution/ui.frontend/src/components/Card/Card.js)。

7. Mavenを使用して、プロジェクトのルートからAEMにすべての変更をデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

8. http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.htmlに移動して、更新されたコンポ [ーネントを確認します](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html) 。

   ![AEMのカードコンポーネントの更新](assets/extend-component/updated-card-in-aem.png)

9. 既存のコンテンツを再オーサリングして、次のようなページを作成できるはずです。

   ![カードコンポーネントの最終オーサリング](assets/extend-component/final-authoring-card.png)

## バリデーターが{#congratulations}

これまで、を使用してAEMコンポーネントを拡張する方法と、SlingモデルとダイアログがJSONモデルと連携する方法を学びました。

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/React/extend-component-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `React/extend-component-solution`ます。
