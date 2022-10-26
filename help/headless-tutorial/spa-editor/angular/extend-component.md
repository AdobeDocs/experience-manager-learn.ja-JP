---
title: コンポーネントの拡張 | AEM SPA Editor とAngularの概要
description: 既存のコアコンポーネントを拡張して、AEM SPA Editor で使用する方法を説明します。 既存のコンポーネントにプロパティとコンテンツを追加する方法を理解することは、AEM SPA Editor の実装の機能を拡張するための強力な手法です。 Sling Resource Merger の Sling モデルおよび機能の拡張に、委任パターンを使用する方法を説明します。
feature: SPA Editor, Core Components
doc-type: tutorial
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 5871
thumbnail: 5871-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 0265d3df-3de8-4a25-9611-ddf73d725f6e
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1935'
ht-degree: 3%

---

# コアコンポーネントの拡張 {#extend-component}

既存のコアコンポーネントを拡張して、AEM SPA Editor で使用する方法を説明します。 既存のコンポーネントの拡張方法を理解することは、AEM SPA Editor 実装の機能をカスタマイズおよび拡張するための強力な手法です。

## 目的

1. 追加のプロパティやコンテンツを使用して、既存のコアコンポーネントを拡張します。
2. を使用したコンポーネントの継承の基本を理解する `sling:resourceSuperType`.
3. 使用方法 [委任パターン](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) を使用して、既存のロジックと機能を再利用できます。

## 作成する内容

この章では、新しい `Card` コンポーネントが作成されます。 この `Card` コンポーネントは [画像コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=ja) タイトルやコールトゥアクションボタンなどのコンテンツフィールドを追加して、SPA内の他のコンテンツに対するティーザーの役割を実行します。

![カードコンポーネントの最終オーサリング](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> 実際の実装では、単に [ティーザーコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/teaser.html) 拡張よりも [画像コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) 作る `Card` コンポーネントを作成できます。 常に、 [コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja) 可能な場合は直接。

## 前提条件

設定に必要なツールと手順を確認します。 [ローカル開発環境](overview.md#local-dev-environment).

### コードの取得

1. このチュートリアルの開始点を Git からダウンロードします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
   ```

2. Maven を使用して、ローカルのAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   を使用する場合 [AEM 6.x](overview.md#compatibility) 追加 `classic` プロファイル：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 従来の [WKND リファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0). が提供する画像 [WKND リファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/latest) が WKND SPAで再利用されます。 パッケージは、 [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![パッケージマネージャーによる wknd.all のインストール](./assets/map-components/package-manager-wknd-all.png)

完成したコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) または、ブランチに切り替えて、コードをローカルでチェックアウトします。 `Angular/extend-component-solution`.

## Inspectの初期カード実装

チャプターのスターターターコードによって、初期のカードコンポーネントが提供されました。 Inspectカード実装の出発点。

1. 任意の IDE で、 `ui.apps` モジュール。
2. に移動します。 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` と `.content.xml` ファイル。

   ![カードコンポーネントのAEM定義の開始](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   プロパティ `sling:resourceSuperType` ポイント `wknd-spa-angular/components/image` それは `Card` コンポーネントは、 WKND SPA画像コンポーネントから機能を継承します。

3. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml` ファイルを検査します。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   この `sling:resourceSuperType` ポイント `core/wcm/components/image/v2/image`. これは、WKND SPA画像コンポーネントがコアコンポーネントの画像から機能を継承していることを示しています。

   別名 [プロキシパターン](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) Sling リソースの継承は、子コンポーネントが必要に応じて機能を継承し、動作を拡張/上書きできるようにするための強力なデザインパターンです。 Sling 継承は複数のレベルの継承をサポートするので、最終的に `Card` コンポーネントは、コアコンポーネントの画像の機能を継承します。

   多くの開発チームは、DRY になろうと努力しています（自分で繰り返さないでください）。 Sling の継承により、AEMでこれを実現できます。

4. の `card` フォルダーを開き、ファイルを開きます。 `_cq_dialog/.content.xml`.

   このファイルは、 `Card` コンポーネント。 Sling 継承を使用している場合、 [Sling Resource Merger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html?lang=ja) ダイアログの一部を上書きまたは拡張する場合。 このサンプルでは、作成者から追加データをキャプチャしてカードコンポーネントに入力するための新しいタブがダイアログに追加されました。

   次のようなプロパティ `sling:orderBefore` 開発者が新しいタブやフォームフィールドを挿入する場所を選択できるようにします。 この場合、 `Text` タブが `asset` タブをクリックします。 Sling Resource Merger を最大限に活用するには、 [画像コンポーネントダイアログ](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml).

5. の `card` フォルダーを開き、ファイルを開きます。 `_cq_editConfig.xml`. このファイルは、AEMオーサリング UI でのドラッグ&amp;ドロップ動作を示します。 画像コンポーネントを拡張する場合、リソースタイプがコンポーネント自体に一致することが重要です。 以下を確認します。 `<parameters>` ノード：

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   ほとんどのコンポーネントでは、 `cq:editConfig`画像コンポーネントの子孫と子子孫は例外です。

6. IDE で、 `ui.frontend` モジュール、移動する `ui.frontend/src/app/components/card`:

   ![Angularコンポーネントの開始](assets/extend-component/angular-card-component-start.png)

7. `card.component.ts` ファイルを検査します。

   コンポーネントは、既にAEMにマッピングするためにスタブ化されています `Card` 標準を使用するコンポーネント `MapTo` 関数に置き換えます。

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   3 つの `@Input` クラス内のパラメーター `src`, `alt`、および `title`. これらは、AngularコンポーネントにマッピングされるAEMコンポーネントからの期待される JSON 値です。

8. ファイルを開きます。 `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   この例では、既存の画像画像コンポーネントを再利用することをAngularしました。 `app-image` 単に `@Input` パラメーター `card.component.ts`. チュートリアルの後半で、追加のプロパティが追加されて表示されます。

## テンプレートポリシーの更新

この最初の `Card` 実装は、AEM SPAエディターで機能を確認します。 最初の `Card` コンポーネントを作成し、テンプレートポリシーを更新する必要があります。

1. スターターコードをAEMのローカルインスタンスにデプロイします（まだデプロイしていない場合）。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. SPA Page Template( ) に移動します。 [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. レイアウトコンテナのポリシーを更新して、新しい `Card` 許可されたコンポーネントとしてのコンポーネント：

   ![レイアウトコンテナポリシーの更新](assets/extend-component/card-component-allowed.png)

   変更をポリシーに保存し、 `Card` 許可されたコンポーネントとしてのコンポーネント：

   ![許可されたコンポーネントとしてのカードコンポーネント](assets/extend-component/card-component-allowed-layout-container.png)

## 初期カードコンポーネントの作成

次に、 `Card` コンポーネントを使用して、AEM SPA Editor を使用します。

1. に移動します。 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. In `Edit` モード、 `Card` コンポーネント `Layout Container`:

   ![新規コンポーネントを挿入](assets/extend-component/insert-custom-component.png)

3. アセットファインダーから `Card` コンポーネント：

   ![画像を追加](assets/extend-component/card-add-image.png)

4. を開きます。 `Card` コンポーネントダイアログと **テキスト** タブ。
5. 次の値を **テキスト** タブ：

   ![「テキストコンポーネント」タブ](assets/extend-component/card-component-text.png)

   **カードのパス** - SPAホームページの下のページを選択します。

   **CTA テキスト** - &quot;続きを読む&quot;

   **カードタイトル**  — 空白のまま

   **リンクされたページからタイトルを取得する** - true を示すチェックボックスをオンにします。

6. を更新します。 **アセットメタデータ** タブで値を追加 **代替テキスト** および **キャプション**.

   現在は、ダイアログの更新後に追加の変更は表示されません。 新しいフィールドをAngularコンポーネントに公開するには、 `Card` コンポーネント。

7. 新しいタブを開き、に移動します。 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card). Inspectの下のコンテンツノード `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` 見つける `Card` コンポーネントのコンテンツ。

   ![CRXDE-Lite コンポーネントのプロパティ](assets/extend-component/crxde-lite-properties.png)

   次のプロパティを観察します。 `cardPath`, `ctaText`, `titleFromPage` は、ダイアログで保持されます。

## カード Sling モデルを更新

最終的に、コンポーネントダイアログの値をAngularコンポーネントに公開するには、の JSON を入力する Sling モデルを更新する必要があります。 `Card` コンポーネント。 また、次の 2 つのビジネスロジックを実装することもできます。

* If `titleFromPage` から **true**&#x200B;を返し、 `cardPath` それ以外の場合は、 `cardTitle` textfield.
* 指定したページの最終変更日を返す `cardPath`.

任意の IDE に戻り、を開きます。 `core` モジュール。

1. `Card.java`（`core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`）ファイルを開きます。

   次の点に注意してください。 `Card` インターフェイスは現在、 `com.adobe.cq.wcm.core.components.models.Image` したがって、は `Image` インターフェイス。 この `Image` インターフェイスは既に `ComponentExporter` Sling モデルを JSON 形式で書き出し、SPAエディターでマッピングするインターフェイス。 したがって、を明示的に拡張する必要はありません。 `ComponentExporter` 我々がやったようなインターフェース [カスタムコンポーネントの章](custom-component.md).

2. インターフェイスに次のメソッドを追加します。

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

   これらのメソッドは、JSON モデル API を介して公開され、Angularコンポーネントに渡されます。

3. `CardImpl.java`を開きます。これは、 `Card.java` インターフェイス。 この実装は、チュートリアルを高速化するために部分的にスタブ化されました。  次の `@Model` および `@Exporter` Sling Model Exporter を介して Sling Model を JSON としてシリアル化できるようにする注釈。

   `CardImpl.java` また、 [Sling モデルの委任パターン](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 画像コアコンポーネントからロジックを書き換えるのを避けるために使用します。

4. 次の行を確認します。

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   上記の注釈は、という名前の Image オブジェクトをインスタンス化します。 `image` 基準 `sling:resourceSuperType` 継承 `Card` コンポーネント。

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   その場合、 `image` オブジェクトを使用して、 `Image` インターフェイスを作る必要がありません。 この手法は `getSrc()`, `getAlt()`、および `getTitle()`.

5. 次に、 `initModel()` プライベート変数を開始するメソッド `cardPage` ～の価値に基づく `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   この `@PostConstruct initModel()` Sling モデルが初期化されるときにが呼び出されるので、モデル内の他のメソッドで使用できるオブジェクトを初期化する場合に役立ちます。 この `pageManager` 次のいずれかの [Java™ベースのグローバルオブジェクト](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html) は、 `@ScriptVariable` 注釈。 この [getPage](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) メソッドは、パスを取り、AEMを返します。 [ページ](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) オブジェクトまたは null （パスが有効なページを指していない場合）。

   これにより、 `cardPage` 変数。基になるリンクされたページに関するデータを返すために、他の新しいメソッドで使用されます。

6. オーサーダイアログを保存した JCR プロパティに既にマッピングされているグローバル変数を確認します。 この `@ValueMapValue` 注釈は、マッピングを自動的に実行するために使用されます。

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

   これらの変数は、 `Card.java` インターフェイス。

7. 次に示すように、 `Card.java` インターフェイス：

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
   > 次の項目を表示すると、 [ここで CardImpl.java が完了しました](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java).

8. ターミナルウィンドウを開き、 `core` Maven を使用するモジュール `autoInstallBundle` プロファイル `core` ディレクトリ。

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   を使用する場合 [AEM 6.x](overview.md#compatibility) 追加 `classic` プロファイル。

9. JSON モデルの応答を次の場所に表示します。 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) を検索し、 `wknd-spa-angular/components/card`:

   ```json
   "card": {
       "ctaText": "Read More",
       "cardTitle": "Page 1",
       "title": "Woman chillaxing with river views in Australian bushland",
       "src": "/content/wknd-spa-angular/us/en/home/_jcr_content/root/responsivegrid/card.coreimg.jpeg/1595190732886/adobestock-216674449.jpeg",
       "alt": "Female sitting on a large rock relaxing in afternoon dappled light the Australian bushland with views over the river",
       "cardLastModified": 1591360492414,
       "ctaLinkURL": "/content/wknd-spa-angular/us/en/home/page-1.html",
       ":type": "wknd-spa-angular/components/card"
   }
   ```

   JSON モデルは、 `CardImpl` Sling モデル。

## 更新Angularコンポーネント

これで、JSON モデルに、 `ctaLinkURL`, `ctaText`, `cardTitle`、および `cardLastModified` これらを表示するAngularコンポーネントを更新できます。

1. IDE に戻り、を開きます。 `ui.frontend` モジュール。 必要に応じて、新しいターミナルウィンドウから webpack 開発サーバーを起動し、変更をリアルタイムで確認します。

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. 開く `card.component.ts` 時刻 `ui.frontend/src/app/components/card/card.component.ts`. 追加の `@Input` 新しいモデルをキャプチャするための注釈：

   ```diff
   export class CardComponent implements OnInit {
   
        @Input() src: string;
        @Input() alt: string;
        @Input() title: string;
   +    @Input() cardTitle: string;
   +    @Input() cardLastModified: number;
   +    @Input() ctaLinkURL: string;
   +    @Input() ctaText: string;
   ```

3. コールトゥアクションの準備ができているかどうかを確認し、 `cardLastModified` 入力：

   ```js
   export class CardComponent implements OnInit {
       ...
       get hasCTA(): boolean {
           return this.ctaLinkURL && this.ctaLinkURL.trim().length > 0 && this.ctaText && this.ctaText.trim().length > 0;
       }
   
       get lastModifiedDate(): string {
           const lastModifiedDate = this.cardLastModified ? new Date(this.cardLastModified) : null;
   
           if (lastModifiedDate) {
           return lastModifiedDate.toLocaleDateString();
           }
           return null;
       }
       ...
   }
   ```

4. 開く `card.component.html` タイトル、行動喚起、最終変更日を表示するには、次のマークアップを追加します。

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
       <div class="card__content">
           <h2 class="card__title">
               {{cardTitle}}
               <span class="card__lastmod" *ngIf="lastModifiedDate">{{lastModifiedDate}}</span>
           </h2>
           <div class="card__action-container" *ngIf="hasCTA">
               <a [routerLink]="ctaLinkURL" class="card__action-link" [title]="ctaText">
                   {{ctaText}}
               </a>
           </div>
       </div>
   </div>
   ```

   Sass ルールは既に次の場所に追加されています： `card.component.scss` タイトルのスタイルを設定するには、コールトゥアクションと最終変更日を使用します。

   >[!NOTE]
   >
   > 完了した [Angularカードのコンポーネントコードをここに入力](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card).

5. Maven を使用して、プロジェクトのルートからAEMに完全な変更をデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. に移動します。 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) 更新されたコンポーネントを確認するには：

   ![AEMのカードコンポーネントの更新](assets/extend-component/updated-card-in-aem.png)

7. 次のようなページを作成するには、既存のコンテンツを再オーサリングできるはずです。

   ![カードコンポーネントの最終オーサリング](assets/extend-component/final-authoring-card.png)

## おめでとうございます。 {#congratulations}

これで、AEMコンポーネントの拡張方法と、Sling モデルとダイアログが JSON モデルと連携する仕組みを学びました。

完成したコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) または、ブランチに切り替えて、コードをローカルでチェックアウトします。 `Angular/extend-component-solution`.
