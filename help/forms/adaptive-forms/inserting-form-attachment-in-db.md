---
title: フォームの添付ファイルをデータベースに挿入
description: AEMワークフローを使用して、フォームの添付ファイルをデータベースに挿入します。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 10488
exl-id: e8a6cab8-423b-4a8e-b2b7-9b24ebe23834
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 1%

---

# データベースへのフォーム添付の挿入

この記事では、MySQL データベースにフォームの添付ファイルを保存する使用例について説明します。

顧客からの一般的な質問は、取り込んだフォームデータとフォームの添付ファイルをデータベーステーブルに保存することです。
この使用例を達成するには、次の手順に従いました

## フォームデータと添付ファイルを格納するデータベーステーブルを作成します

フォームデータを格納する newhire というテーブルが作成されました。 タイプの列名のピクチャに注意してください。 **LONGBLOB** フォームの添付ファイルを保存するには
![table-schema](assets/insert-picture-table.png)

## フォームデータモデルの作成

MySQL データベースと通信するフォームデータモデルが作成されました。 次を作成する必要があります

* [AEMの JDBC データソース](./data-integration-technical-video-setup.md)
* [JDBC データソースに基づくフォームデータモデル](./jdbc-data-model-technical-video-use.md)

## ワークフローを作成

アダプティブフォームをAEMワークフローに送信するように設定すると、フォームの添付ファイルをワークフロー変数に保存するか、ペイロードの下の指定されたフォルダーに添付ファイルを保存するかの選択が可能になります。 この使用例では、添付ファイルを Document の ArrayList 型のワークフロー変数に保存する必要があります。 この ArrayList から、最初の項目を抽出し、ドキュメント変数を初期化する必要があります。 「 」と呼ばれるワークフロー変数 **listOfDocuments** および **employeePhoto** が作成されました。
アダプティブフォームがワークフローのトリガーに送信されると、ワークフロー内の手順で、ECMA スクリプトを使用して employeePhoto 変数が初期化されます。 以下は ECMA スクリプトコードです

```javascript
log.info("executing script now...");
var metaDataMap = graniteWorkItem.getWorkflow().getWorkflowData().getMetaDataMap();
var listOfAttachments = [];
// Make sure you have a workflow variable caled listOfDocuments defined
listOfAttachments = metaDataMap.get("listOfDocuments");
log.info("$$$  got listOfAttachments");
//Make sure you have a workflow variable caled employeePhoto defined
var employeePhoto = listOfAttachments[0];
metaDataMap.put("employeePhoto", employeePhoto);
log.info("Employee Photo updated");
```

ワークフローの次の手順では、「フォームデータモデルを起動」サービスコンポーネントを使用して、データとフォームの添付ファイルをテーブルに挿入します。
![insert-pic](assets/fdm-insert-pic.png)
[サンプル ecma スクリプトを含む完全なワークフローは、こちらからダウンロードできます。](assets/add-new-employee.zip).

>[!NOTE]
> 新しい JDBC ベースのフォームデータモデルを作成し、そのフォームデータモデルをワークフローで使用する必要があります

## アダプティブフォームを作成

前の手順で作成したフォームデータモデルに基づいて、アダプティブフォームを作成します。 フォームデータモデル要素をフォームにドラッグ&amp;ドロップします。 ワークフローをトリガーするためのフォーム送信を設定し、以下のスクリーンショットに示すように、次のプロパティを指定します。
![form-attachments](assets/form-attachments.png)
