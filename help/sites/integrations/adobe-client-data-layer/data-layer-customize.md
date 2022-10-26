---
title: AEMコンポーネントでのAdobeクライアントデータレイヤーのカスタマイズ
description: カスタムAEMコンポーネントのコンテンツを使用してAdobeクライアントデータレイヤーをカスタマイズする方法について説明します。 AEMコアコンポーネントが提供する API を使用して、データレイヤーを拡張およびカスタマイズする方法について説明します。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
kt: 6265
thumbnail: KT-6265.jpg
last-substantial-update: 2022-09-20T00:00:00Z
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '2016'
ht-degree: 4%

---

# AEMコンポーネントでのAdobeクライアントデータレイヤーのカスタマイズ {#customize-data-layer}

カスタムAEMコンポーネントのコンテンツを使用してAdobeクライアントデータレイヤーをカスタマイズする方法について説明します。 が提供する API の使用方法を説明します。 [AEM Core Components to extend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html) をクリックし、データレイヤーをカスタマイズします。

## 作成する内容

![署名データレイヤー](assets/adobe-client-data-layer/byline-data-layer-html.png)

このチュートリアルでは、WKND を更新してAdobeクライアントデータレイヤーを拡張する様々なオプションを確認します [署名コンポーネント](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html). これはカスタムコンポーネントで、このチュートリアルで学習したレッスンは他のカスタムコンポーネントに適用できます。

### 目的 {#objective}

1. Sling モデルとコンポーネント HTL を拡張して、データレイヤーにコンポーネントデータを挿入する
1. コアコンポーネントのデータレイヤーユーティリティを使用して労力を削減
1. コアコンポーネントのデータ属性を使用して、既存のデータレイヤーイベントに関連付ける

## 前提条件 {#prerequisites}

A **ローカル開発環境** このチュートリアルを完了するには、が必要です。 スクリーンショットとビデオは、macOSで実行するAEMas a Cloud ServiceSDK を使用してキャプチャされます。 特に断りのない限り、コマンドとコードはローカルのオペレーティングシステムから独立しています。

**AEM as a Cloud Service は初めてですか？** 以下を確認します。 [AEM as a Cloud Service SDK を使用したローカル開発環境の設定に関する以下のガイド](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja).

**AEM 6.5 を初めて使用する場合** 以下を確認します。 [次のガイドでは、ローカル開発環境を設定します](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja).

## WKND リファレンスサイトをダウンロードしてデプロイします。 {#set-up-wknd-site}

このチュートリアルでは、WKND リファレンスサイトの署名コンポーネントを拡張します。 WKND コードベースのクローンを作成し、ローカル環境にインストールします。

1. ローカルクイックスタートの起動 **作成者** 次の場所で実行されるAEMのインスタンス： [http://localhost:4502](http://localhost:4502).
1. ターミナルウィンドウを開き、Git を使用して WKND コードベースのクローンを作成します。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. WKND コードベースをAEMのローカルインスタンスにデプロイします。

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 および最新のサービスパックを使用している場合は、 `classic` profile to to Maven コマンド：
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. 新しいブラウザーウィンドウを開き、AEMにログインします。 を開きます。 **雑誌** 次のようなページ： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![ページ上の署名コンポーネント](assets/adobe-client-data-layer/byline-component-onpage.png)

   エクスペリエンスフラグメントの一部としてページに追加された署名コンポーネントの例が表示されます。 エクスペリエンスフラグメントは、次の場所で表示できます。 [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)
1. 開発者ツールを開き、 **コンソール**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspectは、AEMサイト上のデータレイヤーの現在の状態を確認する応答を返します。 ページと個々のコンポーネントに関する情報が表示されます。

   ![Adobeデータレイヤーの応答](assets/data-layer-state-response.png)

   署名コンポーネントがデータレイヤーに表示されないことを確認します。

## 署名 Sling モデルの更新 {#sling-model}

データレイヤーにコンポーネントに関するデータを挿入するには、まずコンポーネントの Sling モデルを更新する必要があります。 次に、署名の Java インターフェイスと Sling Model 実装を更新して、新しいメソッドを追加します。 `getData()`. このメソッドには、データレイヤーに挿入するプロパティが含まれます。

1. 任意の IDE で、 `aem-guides-wknd` プロジェクト。 次に移動： `core` モジュール。
1. `Byline.java`（`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`）ファイルを開きます。

   ![署名 Java インターフェイス](assets/adobe-client-data-layer/byline-java-interface.png)

1. インターフェイスに新しいメソッドを追加します。

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

1. `BylineImpl.java`（`core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`）ファイルを開きます。

   これは、 `Byline` インターフェイスが作成され、Sling Model として実装されます。

1. ファイルの先頭に次の import 文を追加します。

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   この `fasterxml.jackson` API を使用して、JSON として公開するデータをシリアル化します。 この `ComponentUtils` を使用して、データレイヤーが有効かどうかを確認できます。

1. 未実装のメソッドを追加 `getData()` から `BylineImple.java`:

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

   上記の方法では、新しい `HashMap` は、JSON として公開するプロパティを取り込むために使用されます。 既存のメソッドは、 `getName()` および `getOccupations()` が使用されます。 `@type` コンポーネントの一意のリソースタイプを表します。これにより、クライアントは、コンポーネントのタイプに基づいてイベントやトリガーを簡単に識別できます。

   この `ObjectMapper` を使用してプロパティをシリアル化し、JSON 文字列を返します。 その後、この JSON 文字列をデータレイヤーに挿入できます。

1. ターミナルウィンドウを開きます。のみをビルドしてデプロイする `core` モジュールで Maven のスキルを使用します。

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## 署名 HTL の更新 {#htl}

次に、 `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl). HTL(HTMLテンプレート言語 ) は、コンポーネントのHTMLをレンダリングするために使用されるテンプレートです。

特別なデータ属性 `data-cmp-data-layer` 各AEMコンポーネントで、データレイヤーを公開するためにを使用します。  AEMコアコンポーネントから提供される JavaScript は、このデータ属性を探します。このデータ属性の値は、署名 Sling モデルの `getData()` メソッドを使用して、値をAdobeクライアントデータレイヤーに挿入します。

1. IDE で、 `aem-guides-wknd` プロジェクト。 次に移動： `ui.apps` モジュール。
1. `byline.html`（`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`）ファイルを開きます。

   ![署名HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. 更新 `byline.html` を含めるには `data-cmp-data-layer` 属性：

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   の値 `data-cmp-data-layer` が `"${byline.data}"` 場所 `byline` は、以前に更新された Sling モデルです。 `.data` は、の HTL で Java Getter メソッドを呼び出すための標準的な表記です。 `getData()` 前の演習で実装済み。

1. ターミナルウィンドウを開きます。のみをビルドしてデプロイする `ui.apps` モジュールで Maven のスキルを使用します。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. ブラウザーに戻り、署名コンポーネントでページを再度開きます。 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. 開発者ツールを開き、ページのHTMLソースに署名コンポーネントがないか調べます。

   ![署名データレイヤー](assets/adobe-client-data-layer/byline-data-layer-html.png)

   次のように表示されます。 `data-cmp-data-layer` は、Sling モデルから JSON 文字列を使用して設定されています。

1. ブラウザーの開発者ツールを開き、 **コンソール**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. の下の応答の下に移動します。 `component` のインスタンスを見つけるには `byline` コンポーネントがデータレイヤーに追加されました。

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

   公開されるプロパティが、 `HashMap` を Sling Model に追加します。

## クリックイベントの追加 {#click-event}

Adobeクライアントデータレイヤーはイベント主導型で、アクションをトリガーする最も一般的なイベントの 1 つは、 `cmp:click` イベント。 AEMコアコンポーネントを使用すれば、データ要素を利用して、コンポーネントを簡単に登録できます。 `data-cmp-clickable`.

クリック可能な要素は、通常、CTA ボタンまたはナビゲーションリンクです。 残念ながら、署名コンポーネントにはこれらのものはありませんが、他のカスタムコンポーネントでは一般的な場合があるので、どのような場合でも登録します。

1. を開きます。 `ui.apps` IDE のモジュール
1. `byline.html`（`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`）ファイルを開きます。

1. 更新 `byline.html` を含めるには `data-cmp-clickable` 署名の属性 **名前** 要素：

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. 新しいターミナルを開きます。 のみをビルドしてデプロイする `ui.apps` モジュールで Maven のスキルを使用します。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. ブラウザーに戻り、署名コンポーネントを追加してページを再度開きます。 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   イベントをテストするには、開発者コンソールを使用して JavaScript を手動で追加します。 詳しくは、 [AEMコアコンポーネントでのAdobeクライアントデータレイヤーの使用](data-layer-overview.md) この方法に関するビデオについては、を参照してください。

1. ブラウザーの開発者ツールを開き、 **コンソール**:

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   この単純なメソッドは、署名コンポーネントの名前のクリックを処理する必要があります。

1. 次のメソッドを **コンソール**:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   上記のメソッドは、イベントリスナーをデータレイヤーにプッシュし、 `cmp:click` イベントと呼び出し `bylineClickHandler`.

   >[!CAUTION]
   >
   > それは重要です **not** この演習全体でブラウザーを更新する場合、コンソール JavaScript は失われます。

1. ブラウザーで、 **コンソール** を開き、署名コンポーネントで作成者の名前をクリックします。

   ![署名コンポーネントのクリック](assets/adobe-client-data-layer/byline-component-clicked.png)

   コンソールメッセージが表示されます。 `Byline Clicked!` 署名名

   この `cmp:click` イベントは、最も簡単に接続できます。 より複雑なコンポーネントの場合や、他の動作を追跡する場合は、カスタム JavaScript を追加して新しいイベントを追加し、登録することができます。 その良い例は、カルーセルコンポーネントです。このトリガーは、 `cmp:show` イベントを設定します。 詳しくは、 [ソースコードの詳細](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219).

## DataLayerBuilder ユーティリティの使用 {#data-layer-builder}

Sling モデルが [更新済み](#sling-model) この章で先ほど、を使用して JSON 文字列を作成することを選択しました。 `HashMap` をクリックし、各プロパティを手動で設定します。 この方法は、小さな 1 回限りのコンポーネントでは正常に機能しますが、AEMコアコンポーネントを拡張するコンポーネントでは、多くの追加コードが生じる可能性があります。

ユーティリティ・クラス `DataLayerBuilder`は、重いリフティングのほとんどを実行するために存在します。 これにより、実装は必要なプロパティのみを拡張できます。 Sling モデルを更新し、 `DataLayerBuilder`.

1. IDE に戻り、 `core` モジュール。
1. `Byline.java`（`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`）ファイルを開きます。
1. を変更します。 `getData()` の型を返すメソッド `ComponentData`

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

   `ComponentData` は、AEMコアコンポーネントが提供するオブジェクトです。 前の例と同様に JSON 文字列が生成されますが、多くの追加作業も実行します。

1. `BylineImpl.java`（`core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`）ファイルを開きます。

1. 次の import 文を追加します。

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. を `getData()` メソッドに次の情報を含めます。

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

   署名コンポーネントは、画像コアコンポーネントの一部を再利用して、作成者を表す画像を表示します。 上記のスニペットでは、 [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) は、 `Image` コンポーネント。 これにより、使用する画像に関するすべてのデータが JSON オブジェクトに事前設定されます。 また、 `@type` およびコンポーネントの一意の識別子。 メソッドが非常に小さいことに注意してください。

   唯一のプロパティは、 `withTitle` これは、 `getName()`.

1. ターミナルウィンドウを開きます。のみをビルドしてデプロイする `core` モジュールで Maven のスキルを使用します。

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. IDE に戻り、を開きます。 `byline.html` ～の下に立ち入る `ui.apps`
1. 使用する HTL を更新します。 `byline.data.json` を設定する `data-cmp-data-layer` 属性：

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   現在、型のオブジェクトを返していることを覚えておいてください。 `ComponentData`. このオブジェクトには、ゲッターメソッドが含まれます `getJson()` これは、 `data-cmp-data-layer` 属性。

1. ターミナルウィンドウを開きます。のみをビルドしてデプロイする `ui.apps` モジュールで Maven のスキルを使用します。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. ブラウザーに戻り、署名コンポーネントを追加してページを再度開きます。 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. ブラウザーの開発者ツールを開き、 **コンソール**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. の下の応答の下に移動します。 `component` のインスタンスを見つけるには `byline` コンポーネント：

   ![署名データレイヤーが更新されました](assets/adobe-client-data-layer/byline-data-layer-builder.png)

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

   今、 `image` オブジェクトを `byline` コンポーネントエントリ。 DAM 内のアセットに関する詳細情報が多く含まれます。 また、 `@type` 一意の ID( この場合は `byline-136073cfcb`) は自動的に設定され、 `repo:modifyDate` コンポーネントがいつ変更されたかを示します。

## その他の例 {#additional-examples}

1. データレイヤーを拡張する別の例は、 `ImageList` WKND コードベースのコンポーネント：
   * `ImageList.java` - Java インターフェイス ( `core` モジュール。
   * `ImageListImpl.java`  — 内の Sling モデル `core` モジュール。
   * `image-list.html`  — 内の HTL テンプレート `ui.apps` モジュール。

   >[!NOTE]
   >
   > のようなカスタムプロパティを含めるのは、もう少し難しくなります。 `occupation` 使用時 [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). ただし、画像またはページを含むコアコンポーネントを拡張する場合、ユーティリティは多くの時間を節約できます。

   >[!NOTE]
   >
   > 実装全体で再利用されるオブジェクトの高度なデータレイヤーを構築する場合は、データレイヤー要素を独自のデータレイヤー固有の Java オブジェクトに抽出することをお勧めします。 例えば、Commerce コアコンポーネントには、 `ProductData` および `CategoryData` これらは、コマース実装内の多くのコンポーネントで使用できるためです。 レビュー [aem-cif-core-components リポジトリのコード](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer) を参照してください。

## おめでとうございます。 {#congratulations}

AEMコンポーネントを使用してAdobeクライアントデータレイヤーを拡張およびカスタマイズする方法をいくつか確認しました。

## その他のリソース {#additional-resources}

* [Adobeクライアントデータレイヤードキュメント](https://github.com/adobe/adobe-client-data-layer/wiki)
* [データレイヤーとコアコンポーネントの統合](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [Adobeクライアントデータレイヤーとコアコンポーネントのドキュメントの使用](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
