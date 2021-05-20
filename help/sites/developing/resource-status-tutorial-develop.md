---
title: AEM Sitesでのリソースステータスの作成
description: 'Adobe Experience ManagerのリソースステータスAPIは、AEMの様々なエディターのWeb UIでステータスメッセージを公開するためのプラグ可能なフレームワークです。 '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# リソースのステータス{#developing-resource-statuses-in-aem-sites}の開発

Adobe Experience ManagerのリソースステータスAPIは、AEMの様々なエディターのWeb UIでステータスメッセージを公開するためのプラグ可能なフレームワークです。

## 概要 {#overview}

エディターのリソースステータスフレームワークは、エディターのステータスを標準的かつ均一な方法で表示および操作するための、サーバー側およびクライアント側のAPIを提供します。

エディターのステータスバーは、AEMのページ、エクスペリエンスフラグメントおよびテンプレートエディターでネイティブに使用できます。

カスタムのリソースステータスプロバイダーの使用例を次に示します。

* ページがスケジュールされたアクティベーションから2時間以内に作成者に通知する
* 過去15分以内にページがアクティブ化されたことを作成者に通知する
* 過去5分以内にページが編集されたユーザーに対して作成者に通知

![AEMエディターのリソースステータスの概要](assets/sample-editor-resource-status-screenshot.png)

## リソース状態プロバイダーフレームワーク{#resource-status-provider-framework}

カスタムリソースステータスを作成する場合、開発作業は次の要素で構成されます。

1. ResourceStatusProvider実装。ステータスが必要かどうかを判断する役割を果たし、ステータスに関する基本情報を示します。タイトル、メッセージ、優先度、バリアント、アイコン、使用可能なアクションを示します。
2. オプションで、使用可能なアクションの機能を実装するGraniteUI JavaScriptを使用できます。

   ![リソースステータスアーキテクチャ](assets/sample-editor-resource-status-application-architecture.png)

3. ページ、エクスペリエンスフラグメント、テンプレートエディターの一部として提供されるステータスリソースには、リソースの「[!DNL statusType]」プロパティを使用して型が指定されます。

   * ページエディター：`editor`
   * エクスペリエンスフラグメントエディター：`editor`
   * テンプレートエディター: `template-editor`

4. ステータスリソースの`statusType`は、登録された`CompositeStatusType` OSGi設定の`name`プロパティに一致します。

   すべての一致に対して、`CompositeStatusType's`型が収集され、`ResourceStatusProvider.getType()`を介して、この型を持つ`ResourceStatusProvider`実装を収集するために使用されます。

5. 一致する`ResourceStatusProvider`がエディターで`resource`に渡され、`resource`が表示されるステータスかどうかが判断されます。 ステータスが必要な場合、この実装は0または多数の`ResourceStatuses`を作成し、それぞれが表示するステータスを表します。

   通常、`ResourceStatusProvider`は`resource`ごとに0または1 `ResourceStatus`を返します。

6. ResourceStatusは、顧客が実装できるインターフェイスです。また、役立つ`com.day.cq.wcm.commons.status.EditorResourceStatus.Builder`を使用してステータスを作成できます。 ステータスは、次の要素で構成されます。

   * タイトル
   * メッセージ
   * アイコン
   * バリアント
   * 優先度
   * アクション
   * データ

7. 必要に応じて、`Actions`が`ResourceStatus`オブジェクトに指定されている場合、機能をステータスバーのアクションリンクにバインドするには、サポートするclientlibsが必要です。

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. アクションをサポートするJavaScriptまたはCSSをサポートする場合、フロントエンドコードがエディターで使用できるように、各エディターの各クライアントライブラリを通じてプロキシ化する必要があります。

   * ページエディターカテゴリ：`cq.authoring.editor.sites.page`
   * エクスペリエンスフラグメントエディターカテゴリ：`cq.authoring.editor.sites.page`
   * テンプレートエディターカテゴリ：`cq.authoring.editor.sites.template`

## コード{#view-the-code}を表示します。

[GitHubのコードを参照してください。](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## その他のリソース {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
