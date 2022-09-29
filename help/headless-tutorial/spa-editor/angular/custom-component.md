---
title: カスタムコンポーネントの作成 | AEM SPA Editor とAngularの概要
description: AEM SPA Editor で使用するカスタムコンポーネントを作成する方法を説明します。 JSON モデルを拡張してカスタムコンポーネントを設定するためのオーサーダイアログと Sling モデルの開発方法について説明します。
sub-product: sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1471'
ht-degree: 3%

---

# カスタムコンポーネントの作成 {#custom-component}

AEM SPA Editor で使用するカスタムコンポーネントを作成する方法を説明します。 JSON モデルを拡張してカスタムコンポーネントを設定するためのオーサーダイアログと Sling モデルの開発方法について説明します。

## 目的

1. AEMが提供する JSON モデル API を操作する際の Sling モデルの役割を理解します。
2. AEMコンポーネントダイアログの作成方法を説明します。
3. 作成方法 **カスタム** SPAエディターフレームワークと互換性のあるAEMコンポーネント。

## 作成する内容

以前の章では、SPAコンポーネントの開発と、それらのマッピングに焦点を当てていました。 *既存* AEMコアコンポーネント。 この章では、の作成および拡張方法について説明します。 *新規* AEMコンポーネントを操作し、AEMが提供する JSON モデルを操作します。

シンプルな `Custom Component` は、新しいAEMコンポーネントを作成するために必要な手順を示しています。

![オールキャップスで表示されたメッセージ](assets/custom-component/message-displayed.png)

## 前提条件

設定に必要なツールと手順を確認します。 [ローカル開発環境](overview.md#local-dev-environment).

### コードの取得

1. このチュートリアルの開始点を Git からダウンロードします。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. Maven を使用して、ローカルのAEMインスタンスにコードベースをデプロイします。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   を使用する場合 [AEM 6.x](overview.md#compatibility) 追加 `classic` プロファイル：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 従来の [WKND リファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/latest). が提供する画像 [WKND リファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/latest) が WKND SPAで再利用されます。 パッケージは、 [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![パッケージマネージャーによる wknd.all のインストール](./assets/map-components/package-manager-wknd-all.png)

完成したコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) または、ブランチに切り替えて、コードをローカルでチェックアウトします。 `Angular/custom-component-solution`.

## AEMコンポーネントの定義

AEMコンポーネントは、ノードおよびプロパティとして定義されます。 プロジェクトでは、これらのノードおよびプロパティは、 `ui.apps` モジュール。 次に、 `ui.apps` モジュール。

>[!NOTE]
>
> に関する簡単なリフレッシャー [AEMコンポーネントの基本が役立つ場合があります](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. を開きます。 `ui.apps` 選択した IDE 内のフォルダー。
2. に移動します。 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` 次の名前のフォルダーを作成します。 `custom-component`.
3. という名前のファイルを作成します。 `.content.xml` の下 `custom-component` フォルダー。 次の項目に `custom-component/.content.xml` を次のように設定します。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![カスタムコンポーネント定義の作成](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"`  — このノードがAEMコンポーネントであることを示します。

   `jcr:title` は、コンテンツ作成者に表示される値で、 `componentGroup` オーサリング UI でのコンポーネントのグループ化を決定します。

4. の `custom-component` フォルダー、名前を付けた別のフォルダーを作成 `_cq_dialog`.
5. の `_cq_dialog` フォルダ作成ファイル名： `.content.xml` を設定し、次のように設定します。

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

   ![カスタムコンポーネントの定義](assets/custom-component/dialog-custom-component-defintion.png)

   上記の XML ファイルは、 `Custom Component`. ファイルの重要な部分は、内部 `<message>` ノード。 このダイアログには、 `textfield` 名前付き `Message` textifeld の値を `message`.

   次に Sling モデルが作成され、の値が公開されます。 `message` JSON モデル経由のプロパティ。

   >[!NOTE]
   >
   > もっと多くを見ることができます [コアコンポーネントの定義を表示したダイアログの例](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). また、 `select`, `textarea`, `pathfield`の `/libs/granite/ui/components/coral/foundation/form` in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   従来のAEMコンポーネントでは、 [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) スクリプトは通常必要です。 SPAによってコンポーネントがレンダリングされるので、HTL スクリプトは不要です。

## Sling モデルの作成

Sling モデルは、JCR から Java™変数へのデータのマッピングを容易にする注釈駆動の Java™ &quot;POJO&quot;(Plain Old Java™ Objects) です。 [Sling モデル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html#sling-models) 通常は、AEMコンポーネント用の複雑なサーバー側のビジネスロジックをカプセル化するために関数を使用します。

SPA Editor のコンテキストでは、 Sling Models は、 [Sling Model Exporter](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=ja).

1. 任意の IDE で、 `core` モジュール。 `CustomComponent.java` および `CustomComponentImpl.java` は既に作成されており、チャプタースターターコードの一部としてスタブアウトされています。

   >[!NOTE]
   >
   > Visual Studio Code IDE を使用している場合は、 [Java™用拡張機能](https://code.visualstudio.com/docs/java/extensions).

2. Java™インターフェイスを開きます。 `CustomComponent.java` 時刻 `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![CustomComponent.java インターフェイス](assets/custom-component/custom-component-interface.png)

   これは、Sling Model によって実装される Java™インターフェイスです。

3. 更新 `CustomComponent.java` それが延びるように `ComponentExporter` インターフェイス：

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   の実装 `ComponentExporter` インターフェイスは、JSON モデル API で Sling モデルを自動的に取得するための要件です。

   この `CustomComponent` インターフェイスには、単一のゲッターメソッドが含まれます `getMessage()`. これは、JSON モデルを使用してオーサーダイアログの値を表示するメソッドです。 空のパラメーターを持つゲッターメソッドのみ `()` は JSON モデルで書き出されます。

4. 開く `CustomComponentImpl.java` 時刻 `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   これは、 `CustomComponent` インターフェイス。 この `@Model` annotation は、Java™クラスを Sling モデルとして識別します。 この `@Exporter` 注釈を使用すると、Sling Model Exporter を通じて Java™クラスをシリアル化および書き出しできます。

5. 静的変数の更新 `RESOURCE_TYPE` AEMコンポーネントを指す `wknd-spa-angular/components/custom-component` 前の練習で作成しました。

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   コンポーネントのリソースタイプは、Sling モデルをAEMコンポーネントにバインドし、最終的にAngularコンポーネントにマッピングするものです。

6. を `getExportedType()` メソッドから `CustomComponentImpl` コンポーネントのリソースタイプを返すクラス：

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   このメソッドは、 `ComponentExporter` インターフェイスを開き、リソースコンポーネントへのマッピングを可能にするAngularタイプを公開します。

7. を更新します。 `getMessage()` メソッドを使用して、 `message` プロパティが作成者ダイアログで保持されます。 以下を使用： `@ValueMap` 注釈は JCR 値にマップされます `message` を Java™変数に追加します。

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

   メッセージの値を大文字で返すための「ビジネスロジック」がいくつか追加されました。 これにより、オーサーダイアログで保存される生の値と Sling モデルで公開される値の違いを確認できます。

   >[!NOTE]
   次の項目を表示すると、 [CustomComponentImpl.java がここで終了しました](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## angularコンポーネントの更新

カスタムコンポーネントのAngularコードは既に作成されています。 次に、AngularコンポーネントをAEMコンポーネントにマッピングするために、いくつかの更新をおこないます。

1. 内 `ui.frontend` モジュールはファイルを開く `ui.frontend/src/app/components/custom/custom.component.ts`
2. 以下を確認します。 `@Input() message: string;` 行 変換後の大文字の値は、この変数にマッピングされる必要があります。
3. 次をインポート： `MapTo` オブジェクトをAEM SPA Editor JS SDK から取得し、それを使用してAEMコンポーネントにマッピングします。

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. 開く `cutom.component.html` そして `{{message}}` が `<h2>` タグを使用します。
5. 開く `custom.component.css` 次のルールを追加します。

   ```css
   :host-context {
       display: block;
   }
   ```

   コンポーネントが空の場合にAEM Editor Placeholder が正しく表示されるようにするには、 `:host-context` または別の `<div>` に設定する必要があります `display: block;`.

6. Maven のスキルを使用して、プロジェクトディレクトリのルートからローカルAEM環境にアップデートをデプロイします。

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## テンプレートポリシーの更新

次に、AEMに移動して更新を確認し、 `Custom Component` をSPAに追加します。

1. 次の場所に移動して、新しい Sling Model の登録を検証します。 [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   上記の 2 行が、 `CustomComponentImpl` が `wknd-spa-angular/components/custom-component` コンポーネントに書き込まれ、Sling Model Exporter を介して登録されていること。

2. SPA Page Template( ) に移動します。 [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. レイアウトコンテナのポリシーを更新して、新しい `Custom Component` 許可されたコンポーネントとして：

   ![レイアウトコンテナポリシーの更新](assets/custom-component/custom-component-allowed.png)

   変更をポリシーに保存し、 `Custom Component` 許可されたコンポーネントとして：

   ![許可されたコンポーネントとしてのカスタムコンポーネント](assets/custom-component/custom-component-allowed-layout-container.png)

## カスタムコンポーネントのオーサリング

次に、 `Custom Component` AEM SPA Editor を使用する。

1. に移動します。 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. In `Edit` モード、 `Custom Component` から `Layout Container`:

   ![新規コンポーネントを挿入](assets/custom-component/insert-custom-component.png)

3. コンポーネントのダイアログを開き、小文字を含むメッセージを入力します。

   ![カスタムコンポーネントの設定](assets/custom-component/enter-dialog-message.png)

   これは、の章で前述した XML ファイルに基づいて作成されたダイアログです。

4. 変更内容を保存します。表示されるメッセージがすべて大文字で表示されることを確認します。

   ![オールキャップスで表示されたメッセージ](assets/custom-component/message-displayed.png)

5. に移動して、JSON モデルを表示します。 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). を検索 `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   JSON 値は、Sling モデルに追加されたロジックに基づいて、すべての大文字に設定されます。

## おめでとうございます。 {#congratulations}

これで、カスタムAEMコンポーネントの作成方法と、Sling モデルとダイアログが JSON モデルと連携する仕組みを学びました。

完成したコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) または、ブランチに切り替えて、コードをローカルでチェックアウトします。 `Angular/custom-component-solution`.

### 次の手順 {#next-steps}

[コアコンポーネントの拡張](extend-component.md)  — 既存のコアコンポーネントを拡張してAEM SPA Editor で使用する方法を説明します。 既存のコンポーネントにプロパティとコンテンツを追加する方法を理解することは、AEM SPA Editor の実装の機能を拡張するための強力な手法です。
