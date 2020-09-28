---
title: カスタムコンポーネントの作成 | AEM SPAエディターとAngularの使用の手引き
description: AEM SPAエディターで使用するカスタムコンポーネントを作成する方法を説明します。 JSONモデルを拡張してカスタムコンポーネントに入力するための作成者ダイアログとSlingモデルの開発方法を学びます。
sub-product: サイト
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5831
thumbnail: 5831-spa-angular.jpg
translation-type: tm+mt
source-git-commit: 1fd4d31770a4eac37a88a7c6960fd51845601bee
workflow-type: tm+mt
source-wordcount: '1505'
ht-degree: 3%

---


# カスタムコンポーネントの作成 {#custom-component}

AEM SPAエディターで使用するカスタムコンポーネントを作成する方法を説明します。 JSONモデルを拡張してカスタムコンポーネントに入力するための作成者ダイアログとSlingモデルの開発方法を学びます。

## 目的

1. AEMが提供するJSONモデルAPIを操作する際のSlingモデルの役割を理解します。
2. 新しいAEMコンポーネントダイアログを作成する方法を理解します。
3. SPAエディターフレームワークと互換性のある **カスタム** AEMコンポーネントの作成方法を説明します。

## 作成する内容

前の章では、SPAコンポーネントの開発と、 *既存のAEMコアコンポーネントへのマッピングに重点を置いていました* 。 この章では、AEMが提供する ** 新しいAEMコンポーネントの作成および拡張方法、およびJSONモデルの操作方法に焦点を当てます。

簡単な例は、新しいAEMコンポーネントを作成するために必要な手順を `Custom Component` 示しています。

![すべて大文字で表示されるメッセージ](assets/custom-component/message-displayed.png)

## 前提条件

Review the required tooling and instructions for setting up a [local development environment](overview.md#local-dev-environment).

### コードの取得

1. Gitを介して、このチュートリアルのスタートポイントをダウンロードします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
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

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `Angular/custom-component-solution`ます。

## AEMコンポーネントの定義

AEMコンポーネントは、ノードとプロパティとして定義されます。 プロジェクトでは、これらのノードとプロパティは、 `ui.apps` モジュール内でXMLファイルとして表されます。 次に、 `ui.apps` モジュールでAEMコンポーネントを作成します。

>[!NOTE]
>
> AEMコンポーネントの [基本に関する簡単なリフレッシャが役立つ場合があります](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html)。

1. In the IDE of your choice open the `ui.apps` folder.
2. に移動し、という名前 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` の新しいフォルダーを作成し `custom-component`ます。
3. Create a new file named `.content.xml` beneath the `custom-component` folder. Populate the `custom-component/.content.xml` with the following:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![カスタムコンポーネント定義の作成](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"`  — このノードがAEMコンポーネントであることを示します。

   `jcr:title` は、コンテンツ作成者に表示される値です。 `componentGroup` は、オーサリングUIでのコンポーネントのグループを決定します。

4. フォルダーの下に、 `custom-component` という名前の別のフォルダーを作成し `_cq_dialog`ます。
5. フォルダーの下に、という名前の新しいファイル `_cq_dialog``.content.xml` を作成し、次のように入力します。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Custom Component"
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
                                                   fieldDescription="The text to display on the component."
                                                   fieldLabel="Message"
                                                   name="./message"/>
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

   ![カスタムコンポーネント定義](assets/custom-component/dialog-custom-component-defintion.png)

   上記のXMLファイルは、の非常に単純なダイアログを生成し `Custom Component`ます。 ファイルの重要な部分は、内側の `<message>` ノードです。 このダイアログには単純な名前が含ま `textfield` れ、テキストフィールドの値はという名前のプロパティに保持され `Message``message`ます。

   JSONモデルを介してプロパティの値を公開するために、次にSlingモデルが作成され `message` ます。

   >[!NOTE]
   >
   > コアコンポーネントの定義を表示すると、さらに多くの [ダイアログの例を表示できます](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components)。 CRXDE-Liteの下にある、 `select`、などの追加のフォームフィールドを表示す `textarea`るこ `pathfield`ともで `/libs/granite/ui/components/coral/foundation/form` き [](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form)ます。

   従来のAEMコンポーネントでは、通常、 [HTL](https://docs.adobe.com/content/help/ja-JP/experience-manager-htl/using/overview.html) スクリプトが必要です。 SPAはコンポーネントをレンダリングするので、HTLスクリプトは必要ありません。

## Slingモデルの作成

Slingモデルは、JCRからJava変数へのデータのマッピングを容易にする注釈駆動のJava 「POJO」(Plain Old Java Objects)です。 [Slingモデル](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html#sling-models) は、通常、AEMコンポーネント用の複雑なサーバー側のビジネスロジックをカプセル化するために機能します。

SPAエディターのコンテキストでは、Slingモデルは、Slingモデルエクスポーターを使用する機能を使用して、JSONモデルを介してコンポ [ーネントのコンテンツを公開します](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/develop-sling-model-exporter.html)。

1. In the IDE of your choice open the `core` module. `CustomComponent.java` を作成 `CustomComponentImpl.java` し、チャプタースターターコードの一部としてスタブアウトしている。

   >[!NOTE]
   >
   > Visual StudioコードIDEを使用する場合は、Java用の [拡張機能をインストールすると便利です](https://code.visualstudio.com/docs/java/extensions)。

2. 次の場所でJavaインターフェイス `CustomComponent.java` を開き `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`ます。

   ![CustomComponent.javaインターフェイス](assets/custom-component/custom-component-interface.png)

   これは、Slingモデルによって実装されるJavaインターフェイスです。

3. インターフェイス `CustomComponent.java` が拡張されるように更新し `ComponentExporter` ます。

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   インター `ComponentExporter` フェイスの実装は、SlingモデルがJSONモデルAPIによって自動的に取得される必要があります。

   この `CustomComponent` インターフェースは、単一のgetterメソッドを含む `getMessage()`。 これは、JSONモデルを介して作成者ダイアログの値を公開するメソッドです。 空のパラメーターを持つgetterメソッドのみ `()` が、JSONモデルにエクスポートされます。

4. を開 `CustomComponentImpl.java` き `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`ます。

   This is the implementation of the `CustomComponent` interface. 注 `@Model` 釈は、JavaクラスをSlingモデルとして識別します。 この `@Exporter` 注釈を使用すると、Javaクラスをシリアライズし、Slingモデルエクスポーターを使用してエクスポートできます。

5. 静的変数を更新 `RESOURCE_TYPE` して、前の練習で作成したAEMコンポーネント `wknd-spa-angular/components/custom-component` を指すようにします。

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   コンポーネントのリソースタイプは、SlingモデルをAEMコンポーネントにバインドし、最終的にAngularコンポーネントにマップされます。

6. コンポー追加ネントリソースタイプを返す `getExportedType()``CustomComponentImpl` メソッドを、クラスに対して次のように指定します。

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   このメソッドは、インター `ComponentExporter` フェイスを実装する場合に必要で、Angularコンポーネントへのマッピングを可能にするリソースタイプを公開します。

7. 作成者ダイアログで保持される `getMessage()``message` プロパティの値を返すようにメソッドを更新します。 注釈の使用：JCR値をJava変数 `@ValueMap``message` にマップします。

   ```java
   import org.apache.commons.lang3.StringUtils;
   ...
   
   @ValueMapValue
   private String message;
   
   @Override
   public String getMessage() {
       return StringUtils.isNotBlank(message) ? message.toUpperCase() : null;
   }
   ```

   メッセージの値を大文字で返すために、「ビジネスロジック」がいくつか追加されました。 これにより、作成者ダイアログに保存された生の値とSlingモデルによって公開された値との違いを確認できます。

   >[!NOTE]
   >
   > ここで、 [完成したCustomComponentImpl.javaを表示できます](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java)。

## 角度コンポーネントを更新する

カスタムコンポーネントの角度コードは既に作成されています。 次に、AngularコンポーネントをAEMコンポーネントにマップするために、いくつかの更新を行います。

1. モジュールで、ファイルを `ui.frontend` 開きます。 `ui.frontend/src/app/components/custom/custom.component.ts`
2. 線を確認し `@Input() message: string;` ます。 変換後の大文字の値は、この変数にマップされることが期待されます。
3. AEM SPA Editor JS SDKから `MapTo` オブジェクトを読み込み、それを使用してAEMコンポーネントにマッピングします。

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. を開 `cutom.component.html` き、の値がタグの横に表示さ `{{message}}` れることを確認し `<h2>` ます。
5. を開 `custom.component.css` き、次のルールを追加します。

   ```css
   :host-context {
       display: block;
   }
   ```

   コンポーネントが空の場合にAEMエディタのプレースホルダーが正しく表示されるようにするには、または他のをに設定する `:host-context` 必要 `<div>` があり `display: block;`ます。

6. Mavenのスキルを使用して、すべての更新をプロジェクト環境のルートからローカルAEMディレクトリに展開します。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## テンプレートポリシーの更新

次に、AEMに移動して更新を検証し、SPAへの更新 `Custom Component` を許可します。

1. http://localhost:4502/system/console/status-slingmodelsに移動して、新しいSlingモデルの登録を確認し [ます](http://localhost:4502/system/console/status-slingmodels)。

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   上記の2行は、がコンポーネントに関連付けられ `CustomComponentImpl``wknd-spa-angular/components/custom-component` ていて、Slingモデルエクスポータを介して登録されていることを示しています。

2. Navigate to the SPA Page Template at [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. レイアウトコンテナのポリシーを更新し、新しいコンポーネントを許可コンポーネント `Custom Component` として追加します。

   ![レイアウトコンテナポリシーの更新](assets/custom-component/custom-component-allowed.png)

   ポリシーに対する変更を保存し、許可されているコンポーネント `Custom Component` として確認します。

   ![許可されるコンポーネントとしてのカスタムコンポーネント](assets/custom-component/custom-component-allowed-layout-container.png)

## カスタムコンポーネントの作成

次に、AEM SPAエディタ `Custom Component` ーを使用して作成します。

1. http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.htmlに移動し [ます](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。
2. モ `Edit` ードで、を次 `Custom Component` に追加し `Layout Container`ます。

   ![新規コンポーネントを挿入](assets/custom-component/insert-custom-component.png)

3. コンポーネントのダイアログを開き、一部の小文字を含むメッセージを入力します。

   ![カスタムコンポーネントの設定](assets/custom-component/enter-dialog-message.png)

   これは、前の章でXMLファイルに基づいて作成されたダイアログです。

4. 変更内容を保存します。表示されるメッセージは、すべて大文字で表示されていることを確認してください。

   ![すべて大文字で表示されるメッセージ](assets/custom-component/message-displayed.png)

5. http://localhost:4502/content/wknd-spa-angular/us/en.model.jsonに移動してJSONモデルを表示 [します](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。 Search for `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   JSON値は、Slingモデルに追加されたロジックに基づいて、すべての大文字に設定されます。

## バリデーターが{#congratulations}

おめでとうございます。カスタムAEMコンポーネントの作成方法と、SlingモデルとダイアログがJSONモデルと連携する方法を学びました。

GitHubでの完了したコードの表示を常に行う [か](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) 、ブランチに切り替えてコードをローカルでチェックアウトすることができ `Angular/custom-component-solution`ます。

### 次の手順 {#next-steps}

[コアコンポーネントの拡張](extend-component.md) - AEM SPAエディターで使用する既存のコアコンポーネントを拡張する方法を学びます。 既存のコンポーネントにプロパティやコンテンツを追加する方法を理解することは、AEM SPA Editorの実装機能を拡張する強力なテクニックです。
