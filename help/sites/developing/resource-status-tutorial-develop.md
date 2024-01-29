---
title: AEM Sites でのリソースステータスの作成
description: Adobe Experience Manager の Resource Status API は、AEM の様々なエディター web UI でステータスメッセージを公開するためのプラグイン可能なフレームワークです。
doc-type: Tutorial
version: 6.4, 6.5
duration: 115
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '410'
ht-degree: 100%

---


# リソースステータスの作成 {#developing-resource-statuses-in-aem-sites}

Adobe Experience Manager の Resource Status API は、AEM の様々なエディター web UI でステータスメッセージを公開するためのプラグイン可能なフレームワークです。

## 概要 {#overview}

エディターのリソースステータスフレームワークは、エディターのステータスを標準的かつ均一な方法で表示および操作するための、サーバーサイドおよびクライアントサイドの API を提供します。

エディターのステータスバーは、AEM のページ、エクスペリエンスフラグメントおよびテンプレートエディターでネイティブで使用できます。

カスタムのリソースステータスプロバイダーの使用例を次に示します。

* ページが予定されているアクティベーションから 2 時間以内になった場合に作成者に通知します。
* 過去 15 分以内にページがアクティブ化されたことを作成者に通知します。
* 過去 5 分以内にページが編集されたこと、および作成者に通知します。

![AEM エディターのリソースステータスの概要](assets/sample-editor-resource-status-screenshot.png)

## リソースステータスプロバイダーフレームワーク {#resource-status-provider-framework}

カスタムのリソースステータスを作成する場合、開発作業は次の要素で構成されます。

1. ResourceStatusProvider 実装。ステータスが必須かどうかを判断する役割を果たし、ステータスに関する基本情報を示します。タイトル、メッセージ、優先度、バリアント、アイコン、使用可能なアクションを示します。
2. オプションで、使用可能なアクションの機能を実装する GraniteUI JavaScript。

   ![リソースステータスアーキテクチャ](assets/sample-editor-resource-status-application-architecture.png)

3. ページ、エクスペリエンスフラグメント、テンプレートエディターの一部として提供されるステータスリソースには、リソース「[!DNL statusType]」プロパティからタイプが付与されます。

   * ページエディター：`editor`
   * エクスペリエンスフラグメントエディター：`editor`
   * テンプレートエディター：`template-editor`

4. ステータスリソースの `statusType` は、登録済みの `CompositeStatusType` OSGi 設定済み `name` プロパティと一致します。

   すべての一致について、`CompositeStatusType's` タイプが収集され、`ResourceStatusProvider.getType()` を介して、このタイプを持つ `ResourceStatusProvider` 実装を収集するために使用されます。

5. 一致する `ResourceStatusProvider` はエディターで `resource` に渡され、`resource` に表示するステータスがあるかどうかを判断します。ステータスが必要な場合、この実装は返すべき 0 個または多数の `ResourceStatuses` を構築し、それぞれが表示するステータスを表します。

   通常、`ResourceStatusProvider` は `resource` ごとに 0 または 1 つの `ResourceStatus` を返します。

6. ResourceStatus は、顧客が実装できるインターフェイスです。または、便利な `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` を使用してステータスを構築できます。ステータスは、次の要素で構成されます。

   * タイトル
   * メッセージ
   * アイコン
   * バリアント
   * 優先度
   * アクション
   * データ

7. オプションで、`ResourceStatus` オブジェクトに `Actions` が提供されている場合、ステータスバーのアクションリンクに機能をバインドするには、サポートする clientlibs が必要です。

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. アクションをサポートするサポート JavaScript または CSS は、各エディターのそれぞれのクライアントライブラリを介してプロキシし、エディターでフロントエンドコードを使用できるようにする必要があります。

   * ページエディターカテゴリ：`cq.authoring.editor.sites.page`
   * エクスペリエンスフラグメントエディターカテゴリ：`cq.authoring.editor.sites.page`
   * テンプレートエディターカテゴリ：`cq.authoring.editor.sites.template`

## コードの表示 {#view-the-code}

[GitHub でコードを参照してください。](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## その他のリソース {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html?lang=ja-JP)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html?lang=ja-JP)
