---
title: AEM Sitesでのリソースステータスの作成
description: 'Adobe Experience Managerのリソースステータス API は、AEMの様々なエディター Web UI でステータスメッセージを公開するためのプラグ可能なフレームワークです。 '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.4, 6.5
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# リソースステータスの作成 {#developing-resource-statuses-in-aem-sites}

Adobe Experience Managerのリソースステータス API は、AEMの様々なエディター Web UI でステータスメッセージを公開するためのプラグ可能なフレームワークです。

## 概要 {#overview}

エディターのリソースステータスフレームワークは、エディターのステータスを標準的かつ均一な方法で表示および操作するための、サーバー側およびクライアント側の API を提供します。

エディターのステータスバーは、AEMのページ、エクスペリエンスフラグメントおよびテンプレートエディターでネイティブで使用できます。

カスタムのリソースステータスプロバイダーの使用例を次に示します。

* ページが予定されているアクティベーションから 2 時間以内になった場合に作成者に通知します
* 過去 15 分以内にページがアクティブ化されたことを作成者に通知します。
* 過去 5 分以内にページが編集されたこと、および編集者に通知

![AEMエディターのリソースステータスの概要](assets/sample-editor-resource-status-screenshot.png)

## リソースステータスプロバイダーフレームワーク {#resource-status-provider-framework}

カスタムのリソースステータスを作成する場合、開発作業は次の要素で構成されます。

1. ResourceStatusProvider 実装。ステータスが必須かどうかを判断する役割を果たし、ステータスに関する基本情報を示します。タイトル、メッセージ、優先度、バリアント、アイコン、使用可能なアクションを示します。
2. オプションで、使用可能なアクションの機能を実装する GraniteUI JavaScript。

   ![リソースステータスアーキテクチャ](assets/sample-editor-resource-status-application-architecture.png)

3. ページ、エクスペリエンスフラグメント、テンプレートエディターの一部として提供されるステータスリソースには、リソース「 」を使用してタイプが付与されます[!DNL statusType]&quot;プロパティ。

   * ページエディター： `editor`
   * エクスペリエンスフラグメントエディター： `editor`
   * テンプレートエディター: `template-editor`

4. ステータスリソースの `statusType` は登録済みと一致します `CompositeStatusType` OSGi が設定されました `name` プロパティ。

   すべての一致について、 `CompositeStatusType's` タイプが収集され、収集に使用されます `ResourceStatusProvider` を介してこのタイプを持つ実装 `ResourceStatusProvider.getType()`.

5. 一致 `ResourceStatusProvider` が渡された `resource` を編集し、 `resource` には、表示するステータスがあります。 ステータスが必要な場合、この実装は 0 または多数の `ResourceStatuses` を返す場合は、それぞれ、表示するステータスを表します。

   通常、 `ResourceStatusProvider` 0 または 1 を返します `ResourceStatus` 単位 `resource`.

6. ResourceStatus は、お客様が実装できるインターフェイス、または役に立つインターフェイスです `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` を使用して、ステータスを作成できます。 ステータスは、次の要素で構成されます。

   * タイトル
   * メッセージ
   * アイコン
   * バリアント
   * 優先度
   * アクション
   * データ

7. （オプション） `Actions` が `ResourceStatus` オブジェクトの場合、機能をステータスバーのアクションリンクにバインドするには、サポートする clientlib が必要です。

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. アクションをサポートするサポートする JavaScript または CSS は、フロントエンドコードがエディターで使用できるように、各エディターの各クライアントライブラリを通じてプロキシ化する必要があります。

   * ページエディターカテゴリ： `cq.authoring.editor.sites.page`
   * エクスペリエンスフラグメントエディターカテゴリ： `cq.authoring.editor.sites.page`
   * テンプレートエディターカテゴリ： `cq.authoring.editor.sites.template`

## コードを表示する {#view-the-code}

[GitHub のコードを参照してください。](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## その他のリソース {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
