---
title: AEMコンポーネントでのAdobeクライアントデータレイヤーのカスタマイズ
description: カスタムのAEMコンポーネントのコンテンツを使用してAdobeクライアントデータレイヤーをカスタマイズする方法について説明します。 AEMコアコンポーネントが提供するAPIを使用して、データレイヤーを拡張およびカスタマイズする方法について説明します。
feature: Adobeクライアントデータレイヤー、コアコンポーネント
topics: integrations
audience: developer
doc-type: tutorial
activity: use
version: cloud-service
kt: 6265
thumbnail: KT-6265.jpg
topic: 統合
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2034'
ht-degree: 4%

---


# AdobeクライアントデータレイヤーをAEMコンポーネントでカスタマイズ{#customize-data-layer}

カスタムのAEMコンポーネントのコンテンツを使用してAdobeクライアントデータレイヤーをカスタマイズする方法について説明します。 [AEMコアコンポーネントが提供するAPIを使用して](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)を拡張し、データレイヤーをカスタマイズする方法について説明します。

## 作成する内容

![署名データレイヤー](assets/adobe-client-data-layer/byline-data-layer-html.png)

このチュートリアルでは、WKND [署名コンポーネント](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html)を更新して、Adobeクライアントデータレイヤーを拡張するための様々なオプションを調べます。 これはカスタムコンポーネントで、このチュートリアルで学習したレッスンは、他のカスタムコンポーネントに適用できます。

### 目的 {#objective}

1. SlingモデルとコンポーネントのHTLを拡張して、データレイヤーにコンポーネントデータを挿入する
1. コアコンポーネントのデータレイヤーユーティリティを使用して作業を軽減
1. コアコンポーネントのデータ属性を使用して、既存のデータレイヤーイベントに関連付ける

## 前提条件 {#prerequisites}

このチュートリアルを完了するには、**ローカル開発環境**&#x200B;が必要です。 スクリーンショットとビデオは、macOS上で動作するAEM as aCloud ServiceSDKを使用してキャプチャされます。 特に断りのない限り、コマンドとコードはローカルのオペレーティングシステムとは独立しています。

**AEM as a Cloud Service は初めてですか？** AEM as a  [Cloud ServiceSDKを使用したローカル開発環境のセットアップについては、次のガイドを参照してください](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。

**AEM 6.5を初めて使用する場合** 次のガイドを参照し [て、ローカル開発環境を設定します](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## WKNDリファレンスサイト{#set-up-wknd-site}をダウンロードしてデプロイします。

このチュートリアルでは、WKNDリファレンスサイトの署名コンポーネントを拡張します。 WKNDコードベースのクローンを作成し、ローカル環境にインストールします。

1. [http://localhost:4502](http://localhost:4502)で実行されているAEMのローカルクイックスタート&#x200B;**オーサー**&#x200B;インスタンスを起動します。
1. ターミナルウィンドウを開き、Gitを使用してWKNDコードベースのクローンを作成します。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. WKNDコードベースをAEMのローカルインスタンスにデプロイします。

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5および最新のサービスパックを使用している場合は、Mavenコマンドに`classic`プロファイルを追加します。
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. 新しいブラウザーウィンドウを開き、AEMにログインします。 次のような&#x200B;**Magazine**&#x200B;ページを開きます。[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![ページ上の署名コンポーネント](assets/adobe-client-data-layer/byline-component-onpage.png)

   エクスペリエンスフラグメントの一部としてページに追加された署名コンポーネントの例が表示されます。 エクスペリエンスフラグメントは、[http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)で確認できます。
1. 開発者ツールを開き、**コンソール**&#x200B;に次のコマンドを入力します。

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspectへの応答で、AEMサイト上のデータレイヤーの現在の状態を確認します。 ページと個々のコンポーネントに関する情報が表示されます。

   ![Adobeデータレイヤーの応答](assets/data-layer-state-response.png)

   署名コンポーネントがデータレイヤーにリストされていないことを確認します。

## 署名Slingモデル{#sling-model}の更新

データレイヤーにコンポーネントに関するデータを挿入するには、まずコンポーネントのSlingモデルを更新する必要があります。 次に、署名のJavaインターフェイスとSling Model実装を更新して、新しいメソッド`getData()`を追加します。 このメソッドには、データレイヤーに挿入するプロパティが含まれます。

1. 任意のIDEで、`aem-guides-wknd`プロジェクトを開きます。 `core`モジュールに移動します。
1. `Byline.java`（`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`）ファイルを開きます。

   ![署名Javaインターフェイス](assets/adobe-client-data-layer/byline-java-interface.png)

1. 新しいメソッドをインターフェイスに追加します。

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

   これは`Byline`インターフェイスの実装で、Slingモデルとして実装されます。

1. ファイルの先頭に次のimport文を追加します。

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   `fasterxml.jackson` APIを使用して、JSONとして公開するデータをシリアル化します。 AEMコアコンポーネントの`ComponentUtils`は、データレイヤーが有効かどうかの確認に使用されます。

1. 未実装のメソッド`getData()`を`BylineImple.java`に追加します。

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

   上記のメソッドでは、JSONとして公開するプロパティを取り込むために新しい`HashMap`を使用します。 `getName()`や`getOccupations()`などの既存のメソッドが使用されます。 `@type` コンポーネントの一意のリソースタイプを表します。これにより、クライアントは、コンポーネントのタイプに基づいてイベントやトリガーを簡単に識別できます。

   `ObjectMapper`は、プロパティをシリアル化してJSON文字列を返すために使用します。 その後、このJSON文字列をデータレイヤーに挿入できます。

1. ターミナルウィンドウを開きます。Mavenのスキルを使用して、`core`モジュールのみを構築してデプロイします。

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## 署名HTLの更新 {#htl}

次に、`Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl)を更新します。 HTL(HTML Template Language)は、コンポーネントのHTMLをレンダリングする際に使用するテンプレートです。

各AEMコンポーネントの特別なデータ属性`data-cmp-data-layer`を使用して、データレイヤーを公開します。  AEMコアコンポーネントが提供するJavaScriptは、このデータ属性を探し、その値に署名Slingモデルの`getData()`メソッドから返されたJSON文字列が入力され、Adobeクライアントデータレイヤーに値を挿入します。

1. IDEで`aem-guides-wknd`プロジェクトを開きます。 `ui.apps`モジュールに移動します。
1. `byline.html`（`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`）ファイルを開きます。

   ![署名HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. `byline.html`を更新して`data-cmp-data-layer`属性を含めます。

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   `data-cmp-data-layer`の値は`"${byline.data}"`に設定されています。`byline`は、前に更新されたSling Modelです。 `.data` は、前の演習で実装したのHTLでJava Getterメソッドを呼び出すた `getData()` めの標準的な表記です。

1. ターミナルウィンドウを開きます。Mavenのスキルを使用して、`ui.apps`モジュールのみを構築してデプロイします。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. ブラウザーに戻り、署名コンポーネントを使用してページを再度開きます。[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. 開発者ツールを開き、ページのHTMLソースに署名コンポーネントがないか調べます。

   ![署名データレイヤー](assets/adobe-client-data-layer/byline-data-layer-html.png)

   `data-cmp-data-layer`にSlingモデルからJSON文字列が入力されていることがわかります。

1. ブラウザーの開発者ツールを開き、**コンソール**&#x200B;に次のコマンドを入力します。

   ```js
   window.adobeDataLayer.getState();
   ```

1. `component`の下の応答の下に移動して、`byline`コンポーネントのインスタンスがデータレイヤーに追加されていることを確認します。

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

   公開されるプロパティが、Sling Modelの`HashMap`に追加されたものと同じであることを確認します。

## クリックイベント{#click-event}の追加

Adobeクライアントデータレイヤーはイベント駆動型で、アクションをトリガーする最も一般的なイベントの1つは`cmp:click`イベントです。 AEMコアコンポーネントを使用すると、データ要素を利用してコンポーネントを簡単に登録できます。`data-cmp-clickable`.

クリック可能な要素は、通常、CTAボタンまたはナビゲーションリンクです。 署名コンポーネントにはこれらのものはありませんが、他のカスタムコンポーネントでは一般的な場合があるので、任意の方法で登録します。

1. IDEで`ui.apps`モジュールを開きます。
1. `byline.html`（`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`）ファイルを開きます。

1. `byline.html`を更新して、署名の&#x200B;**name**&#x200B;要素に`data-cmp-clickable`属性を含めます。

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. 新しいターミナルを開きます。 Mavenのスキルを使用して、`ui.apps`モジュールのみを構築してデプロイします。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. ブラウザーに戻り、署名コンポーネントを追加してページを再度開きます。[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   イベントをテストするには、開発者コンソールを使用してJavaScriptを手動で追加します。 この方法に関するビデオについては、[AEMコアコンポーネントでのAdobeクライアントデータレイヤーの使用](data-layer-overview.md)を参照してください。

1. ブラウザーのデベロッパーツールを開き、**コンソール**&#x200B;に次のメソッドを入力します。

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

1. **コンソール**&#x200B;に次のメソッドを入力します。

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   上記のメソッドは、イベントリスナーをデータレイヤーにプッシュして`cmp:click`イベントをリッスンし、`bylineClickHandler`を呼び出します。

   >[!CAUTION]
   >
   > この演習全体でブラウザーを更新する際には、****&#x200B;ではなくが重要です。そうしないと、コンソールのJavaScriptが失われます。

1. ブラウザーの&#x200B;**コンソール**&#x200B;を開き、署名コンポーネントで作成者の名前をクリックします。

   ![署名コンポーネントをクリック](assets/adobe-client-data-layer/byline-component-clicked.png)

   コンソールメッセージ`Byline Clicked!`と署名名が表示されます。

   `cmp:click`イベントは、最も簡単に接続できます。 より複雑なコンポーネントの場合や、その他の動作を追跡する場合は、カスタムJavaScriptを追加して新しいイベントを追加し、登録することができます。 好例はカルーセルコンポーネントです。このコンポーネントは、スライドが切り替えられるたびに`cmp:show`イベントをトリガーします。 詳しくは、[ソースコード](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219)を参照してください。

## DataLayerBuilderユーティリティ{#data-layer-builder}を使用する

この章の前述のSlingモデルが[更新された](#sling-model)の場合、`HashMap`を使用して各プロパティを手動で設定し、JSON文字列を作成することを選択しました。 この方法は、1回限りの小さなコンポーネントでは正常に機能しますが、AEMコアコンポーネントを拡張するコンポーネントでは、多くの追加コードが生じる可能性があります。

重いリフティングのほとんどを実行するユーティリティクラス`DataLayerBuilder`が存在します。 これにより、実装は必要なプロパティのみを拡張できます。 `DataLayerBuilder`を使用するようにSlingモデルを更新します。

1. IDEに戻り、 `core`モジュールに移動します。
1. `Byline.java`（`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`）ファイルを開きます。
1. `getData()`メソッドを変更して、`ComponentData`型を返します。

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

   `ComponentData` は、AEMコアコンポーネントが提供するオブジェクトです。前の例と同様にJSON文字列が生成されますが、多くの追加作業も実行されます。

1. `BylineImpl.java`（`core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`）ファイルを開きます。

1. 次のimport文を追加します。

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. `getData()`メソッドを次のように置き換えます。

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

   署名コンポーネントは、画像コアコンポーネントの一部を再利用して、作成者を表す画像を表示します。 上記のスニペットでは、[DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)を使用して`Image`コンポーネントのデータレイヤーを拡張します。 これにより、使用する画像に関するすべてのデータがJSONオブジェクトに事前設定されます。 また、`@type`やコンポーネントの一意の識別子の設定など、ルーチンの機能の一部も実行します。 メソッドが小さいことに注意してください。

   唯一のプロパティは`withTitle`を拡張し、`getName()`の値に置き換えられます。

1. ターミナルウィンドウを開きます。Mavenのスキルを使用して、`core`モジュールのみを構築してデプロイします。

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. IDEに戻り、`ui.apps`の下の`byline.html`ファイルを開きます。
1. `byline.data.json`を使用するようにHTLを更新し、`data-cmp-data-layer`属性を設定します。

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   現在、型`ComponentData`のオブジェクトが返されていることを覚えておいてください。 このオブジェクトには、ゲッターメソッド`getJson()`が含まれ、これは`data-cmp-data-layer`属性を設定するために使用されます。

1. ターミナルウィンドウを開きます。Mavenのスキルを使用して、`ui.apps`モジュールのみを構築してデプロイします。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. ブラウザーに戻り、署名コンポーネントを追加してページを再度開きます。[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. ブラウザーの開発者ツールを開き、**コンソール**&#x200B;に次のコマンドを入力します。

   ```js
   window.adobeDataLayer.getState();
   ```

1. `component`の下の応答に移動して、`byline`コンポーネントのインスタンスを探します。

   ![署名データレイヤーの更新](assets/adobe-client-data-layer/byline-data-layer-builder.png)

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

   `byline`コンポーネントエントリ内に`image`オブジェクトが存在することを確認します。 DAM内のアセットに関する情報が多く含まれます。 また、`@type`と一意のID（この場合は`byline-136073cfcb`）が自動的に入力され、コンポーネントが変更された日時を示す`repo:modifyDate`も確認します。

## その他の例{#additional-examples}

1. データレイヤーを拡張するもう1つの例は、WKNDコードベース内の`ImageList`コンポーネントを調べることで確認できます。
   * `ImageList.java`  — モジュール内のJavaインターフ `core` ェイス。
   * `ImageListImpl.java`  — モジュール内のSlingモ `core` デル。
   * `image-list.html` - HTLテンプレート(モジュ `ui.apps` ール内)

   >[!NOTE]
   >
   > [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)を使用する場合、`occupation`のようなカスタムプロパティを含めるのは、もう少し難しくなります。 ただし、画像またはページを含むコアコンポーネントを拡張する場合は、このユーティリティによって多くの時間を節約できます。

   >[!NOTE]
   >
   > 実装全体で再利用されるオブジェクト用の高度なデータレイヤーを構築する場合は、データレイヤー要素を独自のデータレイヤー固有のJavaオブジェクトに抽出することをお勧めします。 例えば、コマースコアコンポーネントには、コマース実装内の多くのコンポーネントで使用できる`ProductData`と`CategoryData`用のインターフェイスが追加されています。 詳しくは、 aem-cif-core-componentsリポジトリの[コードを参照してください。](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer)

## バリデーターが {#congratulations}

AEMコンポーネントを使用してクライアントデータレイヤーを拡張およびカスタマイズする方法をいくつか試しました。

## その他のリソース {#additional-resources}

* [Adobeクライアントデータレイヤーのドキュメント](https://github.com/adobe/adobe-client-data-layer/wiki)
* [コアコンポーネントとのデータレイヤーの統合](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [Adobeクライアントデータレイヤーとコアコンポーネントのドキュメントの使用](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/data-layer/overview.html)
