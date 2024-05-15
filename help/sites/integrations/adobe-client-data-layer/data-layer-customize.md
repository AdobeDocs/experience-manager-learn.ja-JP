---
title: AEM コンポーネントを使用したアドビクライアントデータレイヤーのカスタマイズ
description: カスタム AEM コンポーネントのコンテンツを使用してアドビクライアントデータレイヤーをカスタマイズする方法について説明します。AEM コアコンポーネントから提供される API を使用してデータレイヤーを拡張しカスタマイズする方法について説明します。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
jira: KT-6265
thumbnail: KT-6265.jpg
last-substantial-update: 2022-09-20T00:00:00Z
doc-type: Tutorial
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
duration: 452
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1813'
ht-degree: 100%

---

# AEM コンポーネントを使用したアドビクライアントデータレイヤーのカスタマイズ {#customize-data-layer}

カスタム AEM コンポーネントのコンテンツを使用してアドビクライアントデータレイヤーをカスタマイズする方法について説明します。[AEM コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html?lang=ja)から提供される API を使用してデータレイヤーを拡張しカスタマイズする方法について説明します。

## 作ろうとしているもの

![Byline データレイヤー](assets/adobe-client-data-layer/byline-data-layer-html.png)

このチュートリアルでは、WKND [署名コンポーネント](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html?lang=ja)を更新して、Adobe Client Data Layer を拡張するための様々なオプションを調べます。_署名_&#x200B;コンポーネントは&#x200B;**カスタムコンポーネント**&#x200B;であり、このチュートリアルで学んだ内容は他のカスタムコンポーネントにも適用できます。

### 目的 {#objective}

1. Sling モデルとコンポーネント HTL を拡張してデータレイヤーにコンポーネントデータを挿入する
1. コアコンポーネントのデータレイヤーユーティリティを使用して手間を減らす
1. コアコンポーネントのデータ属性を使用して既存のデータレイヤーイベントに関連付ける

## 前提条件 {#prerequisites}

このチュートリアルを完了するには、**ローカル開発環境**&#x200B;が必要です。スクリーンショットとビデオは、macOS で動作する AEM as a Cloud Service SDK を使用してキャプチャされています。特に断りのない限り、コマンドとコードはローカルのオペレーティングシステムに依存しません。

**AEM as a Cloud Service を初めて使用する場合は、**[AEM as a Cloud Service SDK を使用してローカル開発環境をセットアップするためのガイド](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja)を確認してください。

**AEM 6.5 を初めて使用する場合は、**[ローカル開発環境を設定するためのガイド](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja)を確認してください。

## WKND リファレンスサイトのダウンロードとデプロイ {#set-up-wknd-site}

このチュートリアルでは、WKND リファレンスサイトの署名コンポーネントを拡張します。WKND コードベースのクローンを作成して、ローカル環境にインストールします。

1. [http://localhost:4502](http://localhost:4502) で動作する、AEM のローカルのクイックスタート&#x200B;**オーサー**&#x200B;インスタンスを起動します。
1. ターミナルウィンドウを開き、Git を使用して WKND コードベースのクローンを作成します。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. WKND コードベースを AEM のローカルインスタンスにデプロイします。

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 および最新のサービスパックを使用している場合は、`classic` プロファイルを Maven コマンドに追加します。
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. 新しいブラウザーウィンドウを開き、AEM にログインします。**Magazine** ページ（[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)）を開きます。

   ![ページ上の署名コンポーネント](assets/adobe-client-data-layer/byline-component-onpage.png)

   エクスペリエンスフラグメントの一部としてページに追加された署名コンポーネントの例が表示されます。このエクスペリエンスフラグメントは、[http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html) で表示されます。
1. 開発者ツールを開き、**コンソール**&#x200B;で次のコマンドを入力します。

   ```js
   window.adobeDataLayer.getState();
   ```

   AEM サイト上のデータレイヤーの現在の状態を確認するには、応答を調べます。ページとそれぞれのコンポーネントに関する情報が表示されます。

   ![アドビデータレイヤーの応答](assets/data-layer-state-response.png)

   Byline コンポーネントがデータレイヤーに一覧表示されていないことを確認します。

## Byline Sling モデルの更新 {#sling-model}

データレイヤーにコンポーネントに関するデータを挿入するには、まずコンポーネントの Sling モデルを更新する必要があります。次に、署名の Java™ インターフェイスと Sling モデルの実装を更新して、新しいメソッド `getData()` を使用します。このメソッドには、データレイヤーに挿入されるプロパティが含まれます。

1. 選択した IDE 内の `aem-guides-wknd` プロジェクトを開きます。`core` モジュールに移動します。
1. `Byline.java`（`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`）ファイルを開きます。

   ![署名 Java インターフェイス](assets/adobe-client-data-layer/byline-java-interface.png)

1. 以下のメソッドをインターフェイスに追加します。

   ```java
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return String
        */
       String getData();
   }
   ```

1. `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java` の `BylineImpl.java` ファイルを開きます。これは `Byline` インターフェイスの実装であり、Sling モデルとして実装されます。

1. ファイルの先頭に次の import 文を追加します。

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   `fasterxml.jackson` API を使用して、JSON として公開するデータをシリアル化します。AEM コアコンポーネントの `ComponentUtils` を使用して、データレイヤーが有効かどうかを確認します。

1. 未実装のメソッド `getData()` を `BylineImple.java` に追加します。

   ```java
   public class BylineImpl implements Byline {
       ...
       @Override
       public String getData() {
           Resource bylineResource = this.request.getResource();
           // Use ComponentUtils to verify if the DataLayer is enabled
           if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
               //Create a map of properties we want to expose
               Map<String, Object> bylineProperties = new HashMap<String,Object>();
               bylineProperties.put("@type", bylineResource.getResourceType());
               bylineProperties.put("name", this.getName());
               bylineProperties.put("occupation", this.getOccupations());
               bylineProperties.put("fileReference", image.getFileReference());
   
               //Use AEM Core Component utils to get a unique identifier for the Byline component (in case multiple are on the page)
               String bylineComponentID = ComponentUtils.getId(bylineResource, this.currentPage, this.componentContext);
   
               // Return the bylineProperties as a JSON String with a key of the bylineResource's ID
               try {
                   return String.format("{\"%s\":%s}",
                       bylineComponentID,
                       // Use the ObjectMapper to serialize the bylineProperties to a JSON string
                       new ObjectMapper().writeValueAsString(bylineProperties));
               } catch (JsonProcessingException e) {
   
                   LOGGER.error("Unable to generate dataLayer JSON string", e);
               }
   
           }
           // return null if the Data Layer is not enabled
           return null;
       }
   }
   ```

   上記の方法では、新しい `HashMap` を使用して、JSON として公開するプロパティを取り込みます。`getName()` や `getOccupations()` のような既存のメソッドを使用しています。`@type` は、コンポーネントの一意のリソースタイプを表します。これにより、クライアントは、コンポーネントのタイプに基づいてイベントやトリガーを簡単に識別できます。

   `ObjectMapper` を使用してプロパティをシリアル化し、JSON 文字列を返します。その後、この JSON 文字列をデータレイヤーに挿入できます。

1. ターミナルウィンドウを開きます。Maven スキルを使用して、`core` モジュールだけをビルドしてデプロイします。

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## 署名 HTL の更新 {#htl}

次に、`Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/specification.html?lang=ja) を更新します。HTL（HTML テンプレート言語）は、コンポーネントの HTML をレンダリングするために使用されるテンプレートです。

各 AEM コンポーネントの特別なデータ属性 `data-cmp-data-layer` を使用して、そのデータレイヤーを公開します。AEM コアコンポーネントが提供する JavaScript が、このデータ属性を見つけます。このデータ属性の値には、署名 Sling モデルの `getData()` メソッドが返した JSON 文字列が入力され、その値が Adobe Client Data Layer に挿入されます。

1. `aem-guides-wknd` プロジェクトを IDE で開きます。`ui.apps` モジュールに移動します。
1. `byline.html`（`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`）ファイルを開きます。

   ![署名 HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. `byline.html` を更新して `data-cmp-data-layer` 属性を含めます。

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   `data-cmp-data-layer` の値は `"${byline.data}"` に設定されています。ここで、`byline` は以前に更新された Sling モデルです。`.data` は、前の演習で実装した `getData()` の HTL で Java™ Getter メソッドを呼び出すための標準的な表記法です。

1. ターミナルウィンドウを開きます。Maven スキルを使用して、`ui.apps` モジュールだけをビルドしてデプロイします。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. ブラウザーに戻り、署名コンポーネント [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html) でページを再度開きます。

1. デベロッパーツールを開き、署名コンポーネントのページの HTML ソースを調べます。

   ![署名データレイヤー](assets/adobe-client-data-layer/byline-data-layer-html.png)

   `data-cmp-data-layer` に Sling モデルの JSON 文字列が入力されていることがわかります。

1. ブラウザーのデベロッパーツールを開き、**コンソール**&#x200B;で次のコマンドを入力します。

   ```js
   window.adobeDataLayer.getState();
   ```

1. `component` の応答の下に移動して、データレイヤーに追加された `byline` コンポーネントのインスタンスを見つけます。

   ![データレイヤーの署名部分](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   次のようなエントリが表示されます。

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   公開されているプロパティは、Sling モデルの `HashMap` に追加されたものと同じであることを確認してください。

## クリックイベントの追加 {#click-event}

Adobe Client Data Layer はイベント主導型であり、アクションをトリガーする最も一般的なイベントの 1 つは `cmp:click` イベントです。AEM コアコンポーネントを使用すると、データ要素 `data-cmp-clickable` を利用してコンポーネントを簡単に登録できます。

クリック可能な要素は、通常、CTA ボタンまたはナビゲーションリンクです。残念ながら署名コンポーネントにはこれらの機能はありませんが、他のカスタムコンポーネントでは一般的である可能性があるため、登録しておきます。

1. IDE で `ui.apps` モジュールを開きます。
1. `byline.html`（`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`）ファイルを開きます。

1. `byline.html` を更新して、署名の **name** 要素に `data-cmp-clickable` 属性を含めます。

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. 新しいターミナルを開きます。Maven スキルを使用して `ui.apps` モジュールだけをビルドしてデプロイします。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. ブラウザーに戻り、追加した署名コンポーネント [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html) でページを再度開きます。

   イベントをテストするには、developer console を使用して JavaScript を手動で追加します。この方法に関する動画について詳しくは、[AEM コアコンポーネントでの Adobe Client Data Layer の使用](data-layer-overview.md)を参照してください。

1. ブラウザーの開発者ツールを開き、**コンソール**&#x200B;で次のメソッドを入力します。

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   この単純なメソッドで、署名コンポーネントの名前のクリックを処理します。

1. **コンソール**&#x200B;で次のメソッドを入力します。

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   上記のメソッドは、イベントリスナーをデータレイヤーにプッシュして `cmp:click` イベントをリッスンし、`bylineClickHandler` を呼び出します。

   >[!CAUTION]
   >
   > この演習では、ブラウザーを&#x200B;**更新しない**&#x200B;でください。更新すると、コンソールの JavaScript が失われます。

1. ブラウザーで&#x200B;**コンソール**&#x200B;を開き、署名コンポーネントで作成者の名前をクリックします。

   ![署名コンポーネントをクリック](assets/adobe-client-data-layer/byline-component-clicked.png)

   コンソールメッセージ `Byline Clicked!` と署名の名前が表示されます。

   `cmp:click` イベントは、最も接続しやすいイベントです。より複雑なコンポーネントの場合や、その他の行動をトラッキングする場合は、カスタム JavaScript を追加して新しいイベントを登録できます。良い例は、スライドが切り替えられるたびに `cmp:show` イベントをトリガーするカルーセルコンポーネントです。詳しくは、[ソースコード](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js)を参照してください。

## DataLayerBuilder ユーティリティの使用 {#data-layer-builder}

この章の前半で Sling モデルを[更新](#sling-model)したとき、`HashMap` を使用して各プロパティを手動で設定することにより、JSON 文字列を作成することを選択しました。この方法は、1 回限りの小さなコンポーネントには有効ですが、AEM コアコンポーネントを拡張するようなコンポーネントでは、多くの余分なコードが生じる可能性があります。

ユーティリティクラス `DataLayerBuilder` を使用すると、ほとんどの重作業を実行することができます。これにより、必要なプロパティだけを拡張して実装できます。Sling モデルを更新して、`DataLayerBuilder` を使用するようにしましょう。

1. IDE に戻り、`core` モジュールに移動します。
1. `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java` の `Byline.java` ファイルを開きます。
1. `getData()` メソッドを変更して、`ComponentData` の型を返すようにします。

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   ...
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return ComponentData
        */
       ComponentData getData();
   }
   ```

   `ComponentData` は、AEM コアコンポーネントが提供するオブジェクトです。前の例と同様に JSON 文字列が生成されますが、多くの追加作業も実行します。

1. `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java` の `BylineImpl.java` ファイルを開きます。

1. 次の import 文を追加します。

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. `getData()` メソッドを次のように置き換えます。

   ```java
   @Override
   public ComponentData getData() {
       Resource bylineResource = this.request.getResource();
       // Use ComponentUtils to verify if the DataLayer is enabled
       if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
           return DataLayerBuilder.extending(getImage().getData()).asImageComponent()
               .withTitle(this::getName)
               .build();
   
       }
       // return null if the Data Layer is not enabled
       return null;
   }
   ```

   署名コンポーネントは、画像コアコンポーネントの一部を再利用して、作成者を表す画像を表示します。上記のスニペットでは、[DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) を使用して `Image` コンポーネントのデータレイヤーを拡張しています。これにより、使用する画像に関するすべてのデータが JSON オブジェクトに事前設定されます。また、`@type` やコンポーネントの一意の識別子の設定など、いくつかのルーチン機能も実行します。メソッドが小さいことがわかります。

   唯一の拡張プロパティである `withTitle` は、`getName()` の値に置き換えられます。

1. ターミナルウィンドウを開きます。Maven スキルを使用して `core` モジュールだけをビルドしてデプロイします。

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. IDE に戻り、`ui.apps` にある `byline.html` ファイルを開きます。
1. HTL を更新し、`byline.data.json` を使用して `data-cmp-data-layer` 属性を設定します。

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   `ComponentData` 型のオブジェクトを返すようになりました。このオブジェクトには getter メソッド `getJson()` が含まれており、これは `data-cmp-data-layer` 属性を設定するために使用されます。

1. ターミナルウィンドウを開きます。Maven スキルを使用して、`ui.apps` モジュールだけをビルドしてデプロイします。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. ブラウザーに戻り、追加した署名コンポーネント [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html) でページを再度開きます。
1. ブラウザーのデベロッパーツールを開き、**コンソール**&#x200B;で次のコマンドを入力します。

   ```js
   window.adobeDataLayer.getState();
   ```

1. `component` の応答の下に移動して、`byline` コンポーネントのインスタンスを見つけます。

   ![更新された署名データレイヤー](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   次のようなエントリが表示されます。

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       dc:title: "Stacey Roswells"
       image:
           @type: "image/jpeg"
           repo:id: "142658f8-4029-4299-9cd6-51afd52345c0"
           repo:modifyDate: "2019-10-25T23:49:51Z"
           repo:path: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
           xdm:tags: []
       parentId: "page-30d989b3f8"
       repo:modifyDate: "2019-10-18T20:17:24Z"
   ```

   `byline` コンポーネントのエントリ内に `image` オブジェクトがあることを確認します。DAM 内のアセットに関する詳細情報が多く含まれています。`@type` と一意の ID（この場合は `byline-136073cfcb`）が自動的に入力されていることと、コンポーネントがいつ変更されたかを示す `repo:modifyDate` を確認します。

## その他の例 {#additional-examples}

1. データ層を拡張するその他の例は、WKND コードベースの `ImageList` コンポーネントを調べることで確認できます。
   * `ImageList.java` - `core` モジュールの Java インターフェイス
   * `ImageListImpl.java` - `core` モジュールの Sling モデル
   * `image-list.html` - `ui.apps` モジュールの HTL テンプレート

   >[!NOTE]
   >
   > [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) を使用する場合、`occupation` のようなカスタムプロパティを含めるのは少し難しくなります。ただし、画像またはページを含むコアコンポーネントを拡張する場合、このユーティリティにより多くの時間を節約できます。

   >[!NOTE]
   >
   > 実装全体で再利用されるオブジェクトに対して詳細なデータレイヤーを構築する場合は、データレイヤー要素をデータレイヤー固有の Java™ オブジェクトに抽出することをお勧めします。たとえば、Commerce コア コンポーネントには `ProductData` と `CategoryData` のインターフェイスが追加されています。これは、Commerce の実装内の多くのコンポーネントで使用できるようにするためです。詳しくは、[aem-cif-core-components リポジトリのコード](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer)を確認してください。

## これで完了です。 {#congratulations}

AEM コンポーネントを使用して Adobe Client Data Layer を拡張およびカスタマイズする方法をいくつか確認しました。

## その他のリソース {#additional-resources}

* [Adobe Client Data Layer のドキュメント](https://github.com/adobe/adobe-client-data-layer/wiki)
* [データレイヤーとコアコンポーネントの統合](https://github.com/adobe/aem-core-wcm-components/blob/main/DATA_LAYER_INTEGRATION.md)
* [Adobe Client Data Layer とコアコンポーネントのドキュメントの使用](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ja)
