---
title: AEMワークフローでドキュメントのリストからドキュメントを抽出する
description: ドキュメントのリストから特定のドキュメントを抽出するカスタムワークフローコンポーネント
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-13918
last-substantial-update: 2023-09-12T00:00:00Z
exl-id: b0baac71-3074-49d5-9686-c9955b096abb
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 3%

---

# ドキュメントのリストからドキュメントを抽出

一般的な使用例としては、AEMワークフローの「フォームデータモデルを起動」ステップを使用して、フォームデータとフォームの添付ファイルを外部システムに送信する場合があります。 例えば、ServiceNow でケースを作成する場合、サポート文書と共にケースの詳細を送信する必要があります。 アダプティブフォームに追加された添付ファイルは、配列リスト型のドキュメントの変数に保存され、この配列リストから特定のドキュメントを抽出するには、カスタムコードを記述する必要があります。

この記事では、カスタムワークフローコンポーネントを使用してドキュメントを抽出し、ドキュメント変数に保存する手順について説明します。

## ワークフローの作成

フォームの送信を処理するには、ワークフローを作成する必要があります。 ワークフローでは、次の変数を定義する必要があります

* Document の ArrayList 型の変数（この変数には、ユーザーが追加したフォーム添付ファイルが格納されます）
* Document 型の変数。（この変数は、ArrayList から抽出されたドキュメントを保持します）

* カスタムコンポーネントをワークフローに追加し、そのプロパティを設定します
  ![extract-item-workflow](assets/extract-document-array-list.png)

## アダプティブフォームの設定

* アダプティブフォームの送信アクションを設定してAEMワークフローをトリガーする
  ![submit-action](assets/store-attachments.png)

## ソリューションのテスト

[OSGi Web コンソールを使用してカスタムバンドルをデプロイします。](assets/ExtractItemsFromArray.core-1.0.0-SNAPSHOT.jar)

[パッケージマネージャーを使用したワークフローコンポーネントのインポート](assets/Extract-item-from-documents-list.zip)

[サンプルワークフローをインポート](assets/extract-item-sample-workflow.zip)

[アダプティブフォームの読み込み](assets/test-attachment-extractions-adaptive-form.zip)

[フォームをプレビューします](http://localhost:4502/content/dam/formsanddocuments/testattachmentsextractions/jcr:content?wcmmode=disabled)

フォームに添付ファイルを追加して送信します。

>[!NOTE]
>
>その後、抽出したドキュメントは、「電子メールを送信」や「FDM ステップを起動」など、他のワークフローステップで使用できます
