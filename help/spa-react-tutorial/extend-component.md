---
title: コンポーネントの拡張 | AEM SPA EditorとReactの使い始めに
description: AEM SPAエディタで使用する既存のコアコンポーネントを拡張する方法を説明します。 既存のコンポーネントにプロパティやコンテンツを追加する方法を理解することは、AEM SPA Editorの実装機能を拡張する強力なテクニックです。 Sling Resource MangerのSlingモデルと機能を拡張するための委任パターの使用方法を説明します。
sub-product: サイト
feature: SPA Editor, Core Components
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1974'
ht-degree: 4%

---


# コアコンポーネントの拡張{#extend-component}

AEM SPAエディタで使用する既存のコアコンポーネントを拡張する方法を説明します。 既存のコンポーネントを拡張する方法を理解することは、AEM SPAエディタの実装機能をカスタマイズおよび拡張する強力な手法です。

## 目的

1. 追加のプロパティとコンテンツを使用して、既存のコアコンポーネントを拡張します。
2. `sling:resourceSuperType`を使用して、コンポーネントの継承の基本を理解します。
3. Slingモデルの[委任パターン](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models)を活用して、既存のロジックと機能を再利用する方法を説明します。

## 作成する内容

この章では、新しい`Card`コンポーネントが作成されます。 `Card`コンポーネントは、[Image Core Component](https://docs.adobe.com/content/help/ja/experience-manager-core-components/using/components/image.html)を拡張して、「タイトル」や「行動喚起」ボタンなどの追加のコンテンツフィールドを追加し、SPA内の他のコンテンツに対してティーザーの役割を実行します。

![カードコンポーネントの最終オーサリング](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> 実際の実装では、単に[Teaserコンポーネント](https://docs.adobe.com/content/help/ja/experience-manager-core-components/using/components/teaser.html)を使用し、次に[Image Core Component](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html)を拡張して、プロジェクトの要件に応じて`Card`コンポーネントを作成する方が適している場合があります。 可能な場合は、[コアコンポーネント](https://docs.adobe.com/content/help/ja/experience-manager-core-components/using/introduction.html)を直接使用することをお勧めします。

## 前提条件

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認します。

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

   [AEM 6.x](overview.md#compatibility)を使用している場合は、`classic`プロファイルを追加します。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 従来の[WKNDリファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/latest)用に完成したパッケージをインストールします。 [WKNDリファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/latest)から提供された画像は、WKND SPAで再利用されます。 パッケージは、[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)を使用してインストールできます。

   ![Package Managerのインストールwknd.all](./assets/map-components/package-manager-wknd-all.png)

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/extend-component-solution)に常に表示できます。また、ブランチ`React/extend-component-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。

## Inspectの最初のカードの実装

チャプタースターターコードによって、最初のカードコンポーネントが提供されました。 Inspectは、カードの実装の起点です。

1. 選択したIDEで`ui.apps`モジュールを開きます。
2. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/card`に移動し、`.content.xml`ファイルを表示します。

   ![カードコンポーネントのAEM定義開始](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   プロパティ`sling:resourceSuperType`は`wknd-spa-react/components/image`を指し、`Card`コンポーネントがWKND SPA Imageコンポーネントからすべての機能を継承することを示します。

3. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image/.content.xml` ファイルを検査します。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   `sling:resourceSuperType`が`core/wcm/components/image/v2/image`を指していることに注意してください。 これは、WKND SPAイメージコンポーネントが、コアコンポーネントイメージからすべての機能を継承することを示しています。

   [プロキシパターン](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) Slingリソース継承とも呼ばれます。子コンポーネントに機能を継承させ、必要に応じて動作を拡張/オーバーライドさせるための強力な設計パターンです。 Sling継承は複数レベルの継承をサポートするので、最終的には新しい`Card`コンポーネントはコアコンポーネントイメージの機能を継承します。

   多くの開発チームはDRYになろうと努力しています（繰り返さないでください）。 AEMでは、Slingの継承が可能です。

4. `card`フォルダーの下で、`_cq_dialog/.content.xml`ファイルを開きます。

   このファイルは、`Card`コンポーネントのコンポーネントダイアログ定義です。 Sling継承を使用する場合、[Sling Resource Marger](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html)の機能を使用して、ダイアログの一部を上書きまたは拡張できます。 このサンプルでは、作成者から追加のデータを取り込み、カードコンポーネントに入力するための新しいタブがダイアログに追加されています。

   `sling:orderBefore`のようなプロパティを使用すると、開発者は新しいタブやフォームフィールドを挿入する場所を選択できます。 この場合、`Text`タブは`asset`タブの前に挿入されます。 Sling Resource Margeをフルに活用するには、[イメージコンポーネントダイアログ](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml)の元のダイアログノード構造を知ることが重要です。

5. `card`フォルダーの下で、`_cq_editConfig.xml`ファイルを開きます。 このファイルは、AEMオーサリングUIでのドラッグ&amp;ドロップの動作を指示します。 Imageコンポーネントを拡張する場合、リソースタイプがコンポーネント自体と一致することが重要です。 `<parameters>`ノードを確認します。

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   ほとんどのコンポーネントには`cq:editConfig`は必要ありません。ImageコンポーネントとImageコンポーネントの子孫は例外です。

6. IDEスイッチで`ui.frontend`モジュールに移動し、`ui.frontend/src/components/Card`に移動します。

   ![反応成分開始](assets/extend-component/react-card-component-start.png)

7. `Card.js` ファイルを検査します。

   標準の`MapTo`関数を使用してAEM `Card`コンポーネントにマップするために、コンポーネントは既にスタブアウトされています。

   ```js
   MapTo('wknd-spa-react/components/card')(Card, CardEditConfig);
   ```

8. メソッド`get imageContent()`をInspect:

   ```js
    get imageContent() {
       return (
           <div className="Card__image">
               <Image {...this.props} />
           </div>)
   }
   ```

   この例では、`Card`コンポーネントから`this.props`を渡すだけで、既存のReact Imageコンポーネント`Image`を再利用することを選択しました。 チュートリアルの後半で、`get bodyContent()`メソッドを導入して、タイトル、日付、アクションの呼び物のボタンを表示します。

## テンプレートポリシーの更新

この最初の`Card`導入で、AEM SPAエディタで機能を確認します。 最初の`Card`コンポーネントを確認するには、テンプレートポリシーの更新が必要です。

1. スターターコードをAEMのローカルインスタンスにデプロイします（まだデプロイしていない場合）。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html)にあるSPAページテンプレートに移動します。
3. レイアウトコンテナのポリシーを更新し、新しい`Card`コンポーネントを許可されたコンポーネントとして追加します。

   ![レイアウトコンテナポリシーの更新](assets/extend-component/card-component-allowed.png)

   ポリシーに対する変更を保存し、`Card`コンポーネントを許可されたコンポーネントとして確認します。

   ![許可されたコンポーネントとしてのカードコンポーネント](assets/extend-component/card-component-allowed-layout-container.png)

## 作成者の初期カードコンポーネント

次に、AEM SPAエディタを使用して`Card`コンポーネントを作成します。

1. [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)に移動します。
2. `Edit`モードで、`Card`コンポーネントを`Layout Container`に追加します。

   ![新しいコンポーネントの挿入](assets/extend-component/insert-card-component.png)

3. アセットファインダーから`Card`コンポーネントに画像をドラッグ&amp;ドロップします。

   ![追加画像](assets/extend-component/card-add-image.png)

4. `Card`コンポーネントダイアログを開き、**テキスト**&#x200B;タブが追加されていることを確認します。
5. 「**テキスト**」タブに次の値を入力します。

   ![「テキストコンポーネント」タブ](assets/extend-component/card-component-text.png)

   **カードパス** - SPAホームページの下のページを選択します。

   **CTA Text** :「Read More」

   **カードタイトル**  — 空白のままにします

   **リンクされたページからタイトルを取得**  — 真を示すチェックボックスをオンにします。

6. 「**アセットメタデータ**」タブを更新し、**代替テキスト**&#x200B;と&#x200B;**キャプション**&#x200B;の値を追加します。

   現在、ダイアログを更新した後は、追加の変更は表示されません。 新しいフィールドをReact Componentに公開するには、`Card`コンポーネントのSlingモデルを更新する必要があります。

7. 新しいタブを開き、[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-react/us/en/home/jcr%3Acontent/root/responsivegrid/card)に移動します。 `/content/wknd-spa-react/us/en/home/jcr:content/root/responsivegrid`の下のコンテンツノードをInspectして、`Card`コンポーネントの内容を見つけます。

   ![CRXDE-Liteコンポーネントのプロパティ](assets/extend-component/crxde-lite-properties.png)

   `cardPath`、`ctaText`、`titleFromPage`のプロパティがダイアログに保持されることを確認します。

## カードスリングモデルの更新

最終的にコンポーネントダイアログの値をReactコンポーネントに公開するには、`Card`コンポーネントのJSONを入力するSlingモデルを更新する必要があります。 また、次の2つのビジネスロジックを導入する機会もあります。

* `titleFromPage`から&#x200B;**true**&#x200B;の場合、`cardPath`で指定されたページのタイトルを返します。それ以外の場合は、`cardTitle`テキストフィールドの値を返します。
* `cardPath`で指定されたページの最終変更日を返します。

選択したIDEに戻り、`core`モジュールを開きます。

1. `Card.java`（`core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/Card.java`）ファイルを開きます。

   `Card`インターフェイスは現在`com.adobe.cq.wcm.core.components.models.Image`を拡張しているので、`Image`インターフェイスのすべてのメソッドを継承していることを確認してください。 `Image`インターフェイスは既に`ComponentExporter`インターフェイスを拡張しており、これによりSlingモデルをJSONとして書き出し、SPAエディターでマッピングできます。 したがって、[カスタムコンポーネントの章](custom-component.md)のように、`ComponentExporter`インターフェイスを明示的に拡張する必要はありません。

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

3. 開く `CardImpl.java`. これは`Card.java`インターフェイスの実装です。 この実装は、チュートリアルの速度を上げるために既に部分的に骨格化されています。  `@Model`および`@Exporter`注釈を使用して、SlingモデルがSlingモデルエクスポーターを介してJSONとしてシリアル化できることを確認してください。

   `CardImpl.java` また、Sling  [Modelの](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 委任パターンを使用して、Imageコアコンポーネントのすべてのロジックが書き換えられないようにします。

4. 次の行を確認します。

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   上記の注釈は、`Card`コンポーネントの`sling:resourceSuperType`継承に基づいて、`image`という名前のImageオブジェクトをインスタンス化します。

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   その後、`image`オブジェクトを単純に使用して、`Image`インターフェイスで定義されたメソッドを実装できます。ロジックを自分で記述する必要はありません。 この手法は`getSrc()`、`getAlt()`、`getTitle()`に使用されます。

5. 次に、`initModel()`メソッドを実装し、`cardPath`の値に基づいてプライベート変数`cardPage`を開始します

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   `@PostConstruct initModel()`は、Slingモデルの初期化時に常に呼び出されるので、モデル内の他のメソッドで使用されるオブジェクトを初期化する場合に便利です。 `pageManager`は、`@ScriptVariable`注釈を介してSlingモデルで使用可能にされた、多数の[Javaバックされたグローバルオブジェクト](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects)の1つです。 [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-)メソッドはパスを取り込み、AEM [Page](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html)オブジェクトを返します。パスが有効なページを参照していない場合はnullを返します。

   これにより`cardPage`変数が初期化されます。この変数は、基になるリンクページに関するデータを返すために他の新しいメソッドで使用されます。

6. 作成者ダイアログを保存したJCRプロパティに既にマッピングされているグローバル変数を確認します。 `@ValueMapValue`注釈は、マッピングの自動実行に使用されます。

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

   これらの変数は、`Card.java`インターフェイスの追加メソッドの実装に使用されます。

7. `Card.java`インターフェイスで定義された追加のメソッドを実装します。

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
   > [完了したCardImpl.javaを表示するには、](https://github.com/adobe/aem-guides-wknd-spa/blob/React/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CardImpl.java)ここをクリックします。

8. ターミナルウィンドウを開き、`core`ディレクトリのMaven `autoInstallBundle`プロファイルを使用して`core`モジュールの更新だけを展開します。

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   [AEM 6.x](overview.md#compatibility)を使用する場合は、`classic`プロファイルを追加します。

9. JSONモデルの応答の表示:[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)を検索して`wknd-spa-react/components/card`を探します。

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

   `CardImpl` Slingモデルのメソッドを更新すると、JSONモデルがキーと値のペアを追加して更新されます。

## 反応コンポーネントを更新

JSONモデルに`ctaLinkURL`、`ctaText`、`cardTitle`および`cardLastModified`の新しいプロパティが入力されたので、Reactコンポーネントを更新してこれらを表示できます。

1. IDEに戻り、`ui.frontend`モジュールを開きます。 必要に応じて、新しいターミナルウィンドウからwebpack開発サーバを開始し、変更をリアルタイムで確認します。

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. `Card.js`を`ui.frontend/src/components/Card/Card.js`で開きます。
3. 誘追加い文句（CTA：コールトゥアクション）をレンダリングするメソッド`get ctaButton()`:

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

4. 追加`get lastModifiedDisplayDate()`が`this.props.cardLastModified`を日付を表すローカライズされたStringに変換する方法。

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

5. `get bodyContent()`を更新して`this.props.cardTitle`を表示し、前の手順で作成したメソッドを使用します。

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

6. `Card.scss`には、タイトルのスタイル設定、誘い文句（CTA：コールトゥアクション）、最終変更日の設定を行うためのサスルールが既に追加されています。 ファイルの先頭の`Card.js`に次の行を追加して、これらのスタイルを含めます。

   ```diff
     import {MapTo} from '@adobe/aem-react-editable-components';
   
   + require('./Card.scss');
   
     export const CardEditConfig = {
   ```

   >[!NOTE]
   >
   > [リアクトカードコンポーネントコードをここ](https://github.com/adobe/aem-guides-wknd-spa/blob/React/extend-component-solution/ui.frontend/src/components/Card/Card.js)で表示できます。

7. Mavenを使用して、プロジェクトのルートからAEMにすべての変更をデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

8. [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)に移動して、更新されたコンポーネントを確認します。

   ![AEMのカードコンポーネントの更新](assets/extend-component/updated-card-in-aem.png)

9. 既存のコンテンツを再オーサリングして、次のようなページを作成できるはずです。

   ![カードコンポーネントの最終オーサリング](assets/extend-component/final-authoring-card.png)

## これで完了です! {#congratulations}

これまで、を使用してAEMコンポーネントを拡張する方法と、SlingモデルとダイアログがJSONモデルと連携する方法を学びました。

終了したコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/extend-component-solution)に常に表示できます。また、ブランチ`React/extend-component-solution`に切り替えて、コードをローカルでチェックアウトすることもできます。
