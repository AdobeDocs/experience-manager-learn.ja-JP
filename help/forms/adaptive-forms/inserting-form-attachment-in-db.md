---
title: フォームの添付ファイルをデータベースに挿入
description: AEM ワークフローを使用して、フォームの添付ファイルをデータベースに挿入します。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-10488
exl-id: e8a6cab8-423b-4a8e-b2b7-9b24ebe23834
last-substantial-update: 2020-06-09T00:00:00Z
duration: 98
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '351'
ht-degree: 100%

---

# データベースへのフォーム添付ファイルの挿入

この記事では、MySQL データベースにフォームの添付ファイルを保存する使用例について、順を追って説明します。

顧客からのよくある質問は、取り込んだフォームデータとフォームの添付ファイルをデータベーステーブルに保存にするものです。
このユースケースを達成するために、次の手順に従いました

## フォームデータと添付ファイルを格納するデータベーステーブルの作成

フォームデータを格納する newhire というテーブルが作成されました。 フォームの添付ファイルを格納するための **LONGBLOB** 型の列名の画像に注意してください
![table-schema](assets/insert-picture-table.png)

## フォームデータモデルの作成

MySQL データベースと通信するフォームデータモデルが作成されました。 以下のものを作成する必要があります

* [AEM 内の JDBC データソース](./data-integration-technical-video-setup.md)
* [JDBC データソースに基づくフォームデータモデル](./jdbc-data-model-technical-video-use.md)

## ワークフローの作成

アダプティブフォームを AEM ワークフローに送信するように設定すると、フォームの添付ファイルをワークフロー変数に保存するか、ペイロードの下の指定されたフォルダーに添付ファイルを保存するかの選択が可能になります。 この使用例では、添付ファイルをドキュメントの ArrayList 型のワークフロー変数に保存する必要があります。この ArrayList から最初の項目を抽出し、ドキュメント変数を初期化する必要があります。 **listOfDocuments** および **employeePhoto** と呼ばれるワークフロー変数が作成されました。
アダプティブフォームが送信されてワークフローがトリガーされると、ワークフロー内のステップで、ECMA スクリプトを使用して employeePhoto 変数が初期化されます。 以下は ECMA スクリプトコードです

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

ワークフローの次のステップでは、フォームデータモデルサービスの呼び出しコンポーネントを使用して、データとフォームの添付ファイルをテーブルに挿入します。
![insert-pic](assets/fdm-insert-pic.png)
[サンプル ecma スクリプトを含む完全なワークフローは、こちらからダウンロードできます](assets/add-new-employee.zip)。

>[!NOTE]
> 新しい JDBC ベースのフォームデータモデルを作成し、そのフォームデータモデルをワークフローで使用する必要があります

## アダプティブフォームの作成

前のステップで作成したフォームデータモデルに基づいて、アダプティブフォームを作成します。 フォームデータモデル要素をフォームにドラッグ＆ドロップします。 ワークフローをトリガーするためのフォーム送信を設定し、以下のスクリーンショットに示すように、次のプロパティを指定します。
![form-attachments](assets/form-attachments.png)
