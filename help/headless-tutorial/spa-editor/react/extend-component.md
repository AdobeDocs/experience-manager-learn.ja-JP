---
title: コアコンポーネントの拡張 | AEM SPA Editor と React の使用の手引き
description: 既存のコアコンポーネントの JSON モデルを拡張し、AEM SPA Editor で使用する方法を説明します。 既存のコンポーネントにプロパティとコンテンツを追加する方法を理解すると、AEM SPA エディター実装の機能を拡張するための強力な手法になります。 Sling Resource Merger の Sling モデルおよび機能の拡張に委任パターンを使用する方法を説明します。
feature: SPA Editor, Core Components
version: Cloud Service
jira: KT-5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 44433595-08bc-4a82-9232-49d46c31b07b
duration: 350
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1058'
ht-degree: 16%

---

# コアコンポーネントの拡張 {#extend-component}

既存のコアコンポーネントを拡張して、AEM SPA エディターで使用する方法を説明します。 既存のコンポーネントの拡張方法を理解することは、AEM SPA エディター実装の機能をカスタマイズおよび拡張するための強力な手法です。

## 目的

1. 追加のプロパティやコンテンツを使用して、既存のコアコンポーネントを拡張する
2. `sling:resourceSuperType` を使用したコンポーネントの継承の基本を理解する
3. 以下を行うために、 [委任パターン](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) Sling モデルが既存のロジックと機能を再利用するための条件です。

## 作成する内容

この章では、標準に追加のプロパティを追加するために必要な追加のコードを示します。 `Image` 新しい要求を満たすための要素 `Banner` コンポーネント。 The `Banner` コンポーネントには、標準と同じプロパティがすべて含まれます `Image` コンポーネントに含まれますが、ユーザーが **バナーテキスト**.

![最終的に作成されたバナーコンポーネント](assets/extend-component/final-author-banner-component.png)

## 前提条件

[ローカル開発環境](overview.md#local-dev-environment)の設定に必要なツールと手順を確認してください。この時点で、チュートリアルのユーザーがAEM SPAエディターの機能を確実に理解していることを前提としています。

## Sling リソーススーパータイプでの継承 {#sling-resource-super-type}

既存のコンポーネントを拡張するには、という名前のプロパティを設定します。 `sling:resourceSuperType` を設定します。  `sling:resourceSuperType`は [プロパティ](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties) これは、別のコンポーネントを指すAEMコンポーネントの定義に対して設定できます。 これにより、 `sling:resourceSuperType`.

を拡張する場合、 `Image` コンポーネント： `wknd-spa-react/components/image` コードを更新する必要があります ( `ui.apps` モジュール。

1. の下に新しいフォルダーを作成します。 `ui.apps` ～のモジュール `banner` 時刻 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner`.
1. の下 `banner` コンポーネント定義を作成する (`.content.xml`) は次のようになります。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Banner"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   これは設定 `wknd-spa-react/components/banner` すべての機能を継承する `wknd-spa-react/components/image`.

## cq:editConfig {#cq-edit-config}

The `_cq_editConfig.xml` ファイルは、AEMオーサリング UI でのドラッグ&amp;ドロップ動作を示します。 画像コンポーネントを拡張する場合、リソースタイプがコンポーネント自体に一致することが重要です。

1. Adobe Analytics の `ui.apps` モジュールは以下に別のファイルを作成します。 `banner` 名前付き `_cq_editConfig.xml`.
1. 入力 `_cq_editConfig.xml` を次の XML に置き換えます。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="cq:EditConfig">
       <cq:dropTargets jcr:primaryType="nt:unstructured">
           <image
               jcr:primaryType="cq:DropTargetConfig"
               accept="[image/gif,image/jpeg,image/png,image/webp,image/tiff,image/svg\\+xml]"
               groups="[media]"
               propertyName="./fileReference">
               <parameters
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wknd-spa-react/components/banner"
                   imageCrop=""
                   imageMap=""
                   imageRotate=""/>
           </image>
       </cq:dropTargets>
       <cq:inplaceEditing
           jcr:primaryType="cq:InplaceEditingConfig"
           active="{Boolean}true"
           editorType="image">
           <inplaceEditingConfig jcr:primaryType="nt:unstructured">
               <plugins jcr:primaryType="nt:unstructured">
                   <crop
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*">
                       <aspectRatios jcr:primaryType="nt:unstructured">
                           <wideLandscape
                               jcr:primaryType="nt:unstructured"
                               name="Wide Landscape"
                               ratio="0.6180"/>
                           <landscape
                               jcr:primaryType="nt:unstructured"
                               name="Landscape"
                               ratio="0.8284"/>
                           <square
                               jcr:primaryType="nt:unstructured"
                               name="Square"
                               ratio="1"/>
                           <portrait
                               jcr:primaryType="nt:unstructured"
                               name="Portrait"
                               ratio="1.6180"/>
                       </aspectRatios>
                   </crop>
                   <flip
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="-"/>
                   <map
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff,image/svg+xml]"
                       features="*"/>
                   <rotate
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
                   <zoom
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
               </plugins>
               <ui jcr:primaryType="nt:unstructured">
                   <inline
                       jcr:primaryType="nt:unstructured"
                       toolbar="[crop#launch,rotate#right,history#undo,history#redo,fullscreen#fullscreen,control#close,control#finish]">
                       <replacementToolbars
                           jcr:primaryType="nt:unstructured"
                           crop="[crop#identifier,crop#unlaunch,crop#confirm]"/>
                   </inline>
                   <fullscreen jcr:primaryType="nt:unstructured">
                       <toolbar
                           jcr:primaryType="nt:unstructured"
                           left="[crop#launchwithratio,rotate#right,flip#horizontal,flip#vertical,zoom#reset100,zoom#popupslider]"
                           right="[history#undo,history#redo,fullscreen#fullscreenexit]"/>
                       <replacementToolbars jcr:primaryType="nt:unstructured">
                           <crop
                               jcr:primaryType="nt:unstructured"
                               left="[crop#identifier]"
                               right="[crop#unlaunch,crop#confirm]"/>
                           <map
                               jcr:primaryType="nt:unstructured"
                               left="[map#rectangle,map#circle,map#polygon]"
                               right="[map#unlaunch,map#confirm]"/>
                       </replacementToolbars>
                   </fullscreen>
               </ui>
           </inplaceEditingConfig>
       </cq:inplaceEditing>
   </jcr:root>
   ```

1. ファイルの固有の側面は、 `<parameters>` resourceType をに設定するノード `wknd-spa-react/components/banner`.

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/banner"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   ほとんどのコンポーネントでは、 `_cq_editConfig`. 画像のコンポーネントと子孫は例外です。

## ダイアログの拡張 {#extend-dialog}

アドビの `Banner` コンポーネントでは、 `bannerText`. Sling 継承を使用しているので、 [Sling Resource Merger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html?lang=ja) ダイアログの一部を上書きまたは拡張する場合。 このサンプルでは、作成者から追加データをキャプチャしてカードコンポーネントに入力するための新しいタブがダイアログに追加されています。

1. Adobe Analytics の `ui.apps` モジュール、 `banner` フォルダー、名前を付けたフォルダーを作成します。 `_cq_dialog`.
1. の下 `_cq_dialog` ダイアログ定義ファイルの作成 `.content.xml`. 次のように入力します。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Banner"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <text
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Text"
                           sling:orderBefore="asset"
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
                                               <textGroup
                                                   granite:hide="${cqDesign.titleHidden}"
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/well">
                                                   <items jcr:primaryType="nt:unstructured">
                                                       <bannerText
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           fieldDescription="Text to display on top of the banner."
                                                           fieldLabel="Banner Text"
                                                           name="./bannerText"/>
                                                   </items>
                                               </textGroup>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </text>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   上記の XML 定義により、という名前の新しいタブが作成されます。 **テキスト** そして注文して *前* 既存の **アセット** タブをクリックします。 1 つのフィールドが含まれます **バナーテキスト**.

1. ダイアログは次のようになります。

   ![バナーの最終ダイアログ](assets/extend-component/banner-dialog.png)

   タブを定義する必要がなかったことを確認します。 **アセット** または **メタデータ**. これらは、 `sling:resourceSuperType` プロパティ。

   ダイアログをプレビューする前に、SPAコンポーネントと `MapTo` 関数に置き換えます。

## SPAコンポーネントの実装 {#implement-spa-component}

SPAエディターでバナーコンポーネントを使用するには、次にマッピングする新しいSPAコンポーネントを作成する必要があります。 `wknd-spa-react/components/banner`. これは、 `ui.frontend` モジュール。

1. Adobe Analytics の `ui.frontend` モジュール新しいフォルダーの作成 `Banner` 時刻 `ui.frontend/src/components/Banner`.
1. `Banner` フォルダーの下に `Banner.js` という名前の新規ファイルを作成します。次のように入力します。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   export const BannerEditConfig = {
       emptyLabel: 'Banner',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   
   export default class Banner extends Component {
   
       get content() {
           return <img     
                   className="Image-src"
                   src={this.props.src}
                   alt={this.props.alt}
                   title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       // display our custom bannerText property!
       get bannerText() {
           if(this.props.bannerText) {
               return <h4>{this.props.bannerText}</h4>;
           }
   
           return null;
       }
   
       render() {
           if (BannerEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
               <div className="Banner">
                   {this.bannerText}
                   <div className="BannerImage">{this.content}</div>
               </div>
           );
       }
   }
   
   MapTo('wknd-spa-react/components/banner')(Banner, BannerEditConfig);
   ```

   このSPAコンポーネントは、AEMコンポーネントにマッピングされます `wknd-spa-react/components/banner` 作成済み。

1. 更新 `import-components.js` 時刻 `ui.frontend/src/components/import-components.js` 新しい `Banner` SPAコンポーネント：

   ```diff
     import './ExperienceFragment/ExperienceFragment';
     import './OpenWeather/OpenWeather';
   + import './Banner/Banner';
   ```

1. この時点で、プロジェクトをAEMにデプロイして、ダイアログをテストできます。 Maven のスキルを使用してプロジェクトをデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. SPAテンプレートのポリシーを更新して、 `Banner` コンポーネントを **許可されたコンポーネント**.

1. SPAページに移動し、 `Banner` コンポーネントをSPAページの 1 つに配置します。

   ![バナーコンポーネントを追加](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > このダイアログでは、次の値を保存できます。 **バナーテキスト** ただし、この値はSPAコンポーネントには反映されません。 有効にするには、コンポーネントの Sling モデルを拡張する必要があります。

## Java インターフェイスを追加 {#java-interface}

最終的に、コンポーネントダイアログの値を React コンポーネントに公開するには、の JSON を入力する Sling Model を更新する必要があります。 `Banner` コンポーネント。 これは、 `core` SPAプロジェクトのすべての Java コードを含むモジュール。

まず、用の新しい Java インターフェイスを作成します。 `Banner` これは `Image` Java インターフェイス。

1. Adobe Analytics の `core` モジュール作成新しいファイル名： `BannerModel.java` 時刻 `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. `BannerModel.java` に以下を入力します。

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.osgi.annotation.versioning.ProviderType;
   
   @ProviderType
   public interface BannerModel extends Image {
   
       public String getBannerText();
   
   }
   ```

   これにより、すべてのメソッドがコアコンポーネントから継承されます `Image` インターフェイスと新しいメソッドを 1 つ追加します。 `getBannerText()`.

## Sling モデルの実装 {#sling-model}

次に、の Sling モデルを実装します。 `BannerModel` インターフェイス。

1. Adobe Analytics の `core` モジュール作成新しいファイル名： `BannerModelImpl.java` 時刻 `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`.

1. `BannerModelImpl.java` に以下を入力します。

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import com.adobe.aem.guides.wkndspa.react.core.models.BannerModel;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.via.ResourceSuperType;
   
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { BannerModel.class,ComponentExporter.class}, 
       resourceType = BannerModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   @Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)
   public class BannerModelImpl implements BannerModel {
   
       // points to the the component resource path in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/banner";
   
       @Self
       private SlingHttpServletRequest request;
   
       // With sling inheritance (sling:resourceSuperType) we can adapt the current resource to the Image class
       // this allows us to re-use all of the functionality of the Image class, without having to implement it ourself
       // see https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models
       @Self
       @Via(type = ResourceSuperType.class)
       private Image image;
   
       // map the property saved by the dialog to a variable named `bannerText`
       @ValueMapValue
       private String bannerText;
   
       // public getter to expose the value of `bannerText` via the Sling Model and JSON output
       @Override
       public String getBannerText() {
           return bannerText;
       }
   
       // Re-use the Image class for all other methods:
   
       @Override
       public String getSrc() {
           return null != image ? image.getSrc() : null;
       }
   
       @Override
       public String getAlt() {
           return null != image ? image.getAlt() : null;
       }
   
       @Override
       public String getTitle() {
           return null != image ? image.getTitle() : null;
       }
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/banner`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return BannerModelImpl.RESOURCE_TYPE;
       }
   }
   ```

   次の点に注意してください。 `@Model` および `@Exporter` Sling Model Exporter を介して Sling Model を JSON としてシリアル化できるようにする注釈。

   `BannerModelImpl.java` は [Sling モデルの委任パターン](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 画像コアコンポーネントからすべてのロジックが書き換えられるのを防ぎます。

1. 次の行を確認します。

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   上記の注釈は、という名前の画像オブジェクトをインスタンス化します。 `image` 基準： `sling:resourceSuperType` 継承 `Banner` コンポーネント。

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   その場合、`image` オブジェクトを使用して `Image` インターフェイスで定義されたメソッドを実装することができるので、自分でロジックを書く必要はありません。この手法は、 `getSrc()`, `getAlt()` および `getTitle()`.

1. ターミナルウィンドウを開き、`core` ディレクトリの Maven `autoInstallBundle` プロファイルを使用して `core` モジュールへの更新のみをデプロイします。

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## まとめ {#put-together}

1. AEMに戻り、 `Banner` コンポーネント。
1. を更新します。 `Banner` 含めるコンポーネント **バナーテキスト**:

   ![バナーテキスト](assets/extend-component/banner-text-dialog.png)

1. コンポーネントに画像を入力します。

   ![バナーダイアログに画像を追加](assets/extend-component/banner-dialog-image.png)

   ダイアログの更新を保存します。

1. これで、のレンダリング値が表示されます。 **バナーテキスト**:

![バナーテキストが表示されました](assets/extend-component/banner-text-displayed.png)

1. JSON モデルの応答を次の場所に表示します。 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) を検索し、 `wknd-spa-react/components/card`:

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   JSON モデルは、 `BannerModelImpl.java`.

## おめでとうございます。 {#congratulations}

これで、を使用してAEMコンポーネントを拡張する方法と、Sling のモデルとダイアログが JSON モデルと連携する方法を学びました。
