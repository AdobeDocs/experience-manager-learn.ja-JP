---
title: AEM Sitesの資源状態の開発
description: 'Adobe Experience ManagerのリソースステータスAPIは、AEMの各種エディタWeb UIでステータスメッセージを公開するためのプラグ可能なフレームワークです。 '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# リソース・ステータスの開発 {#developing-resource-statuses-in-aem-sites}

Adobe Experience ManagerのリソースステータスAPIは、AEMの各種エディタWeb UIでステータスメッセージを公開するためのプラグ可能なフレームワークです。

## 概要 {#overview}

Resource Status for Editorsフレームワークは、標準的かつ均一な方法で、エディターのステータスを表示し、操作するためのサーバー側およびクライアント側のAPIを提供します。

エディターのステータスバーは、AEMのページエディター、エクスペリエンスフラグメントエディター、テンプレートエディターでネイティブに使用できます。

カスタムのリソースステータスプロバイダーの使用例を次に示します。

* ページがスケジュールされたアクティベーションから2時間以内に表示された場合に作成者に通知する
* 過去15分以内にページがアクティブ化されたことを作成者に通知する
* 過去5分以内にページが編集されたこと、および編集者に通知する

![AEMエディターのリソースの状態の概要](assets/sample-editor-resource-status-screenshot.png)

## リソース状態プロバイダーのフレームワーク {#resource-status-provider-framework}

カスタムのリソースステータスを開発する場合、開発作業は次のものから構成されます。

1. ResourceStatusProvider実装。ステータスが必要かどうかを判断し、ステータスに関する基本情報を管理します。タイトル、メッセージ、優先度、バリアント、アイコンおよび使用可能なアクション。
2. 必要に応じて、使用可能な任意のアクションの機能を実装するGraniteUI JavaScript。

   ![資源状態アーキテクチャ](assets/sample-editor-resource-status-application-architecture.png)

3. ページ、エクスペリエンスフラグメント、テンプレートの各エディターに含まれるステータスリソースには、リソースの「[!DNL statusType]」プロパティを介して型が割り当てられます。

   * Page editor: `editor`
   * Experience Fragment editor: `editor`
   * テンプレートエディター: `template-editor`

4. ステータスリソースの `statusType` 値は、登録済みの `CompositeStatusType` OSGi設定済み `name` プロパティと一致します。

   すべての一致に対して、 `CompositeStatusType's` 型が収集され、を介してこの型を持つ `ResourceStatusProvider` 実装が収集されます `ResourceStatusProvider.getType()`。

5. 一致 `ResourceStatusProvider` はエディタ `resource` ーで渡され、表示するステータス `resource` があるかどうかが判別されます。 ステータスが必要な場合、この実装は0または多数の返される値を作成し、それぞれが表示するステータス `ResourceStatuses` を表します。

   通常、は、1回につき0または1 `ResourceStatusProvider` を返し `ResourceStatus``resource`ます。

6. ResourceStatusは、顧客が実装できるインターフェイスです。または、有用な機能を使用してステータスを構築す `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` ることもできます。 ステータスは次のもので構成されます。

   * タイトル
   * メッセージ
   * アイコン
   * バリアント
   * 優先度
   * アクション
   * データ

7. 必要に応じて、 `Actions``ResourceStatus` オブジェクトに対してclientlibを指定する場合は、機能をアクションリンクとステータスバーに連結するために、clientlibをサポートする必要があります。

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. アクションをサポートするJavaScriptまたはCSSをサポートする場合は、フロントエンドコードをエディターで確実に使用できるように、各エディターの対応するクライアントライブラリを通じてプロキシ化する必要があります。

   * ページエディタカテゴリ: `cq.authoring.editor.sites.page`
   * エクスペリエンスフラグメントエディターのカテゴリ: `cq.authoring.editor.sites.page`
   * テンプレートエディターカテゴリ: `cq.authoring.editor.sites.template`

## コードの表示 {#view-the-code}

[GitHubのコードを参照](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## その他のリソース {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
