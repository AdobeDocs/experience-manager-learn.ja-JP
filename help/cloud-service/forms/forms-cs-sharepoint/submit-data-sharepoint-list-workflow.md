---
title: ワークフローステップを使用してSharePointリストにデータを送信
description: FDM ワークフローを呼び出しステップを使用して、SharePoint リストにデータを挿入します
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-15126
source-git-commit: 3dc1aea74e2a7cf30da9f6fb96ecc5c7edcf6e34
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 3%

---

# FDM を起動ワークフローステップを使用してSharePointリストにデータを挿入


この記事では、AEMワークフローの FDM を起動ステップを使用して、SharePointリストにデータを挿入するために必要な手順を説明します。

この記事は、 [アダプティブフォームがSharePointリストにデータを送信するように正常に設定されました。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=ja#connect-af-sharepoint-list)


## SharePointリストのデータソースに基づいてフォームデータモデルを作成する

* SharePointリストのデータソースに基づいて、新しいフォームデータモデルを作成します。
* 適切なモデルを追加し、フォームデータモデルの get サービスを実行します。
* Insert サービスを設定して、トップレベルのモデルオブジェクトを挿入します。
* insert サービスをテストします。


## ワークフローの作成

* FDM を起動ステップで単純なワークフローを作成します。
* 前の手順で作成したフォームデータモデルを使用するように FDM を起動手順を設定します。
* ![associate-fdm](assets/fdm-insert-1.png)

* ![map-input-parameters](assets/fdm-insert-2.png)
* JSON ドット表記を使用していることに注意してください。 送信されたデータは以下の形式で、送信されたデータから ContactUS オブジェクトを抽出します。

```json
{
  "ContactUS": {
    "Title": "Mr",
    "Products": "Photoshop",
    "HighNetWorth": "1",
    "SubmitterName": "John Does"
  }
}
```



## アダプティブフォームをトリガーAEMワークフローに設定する

* 前の手順で作成したフォームデータモデルを使用して、アダプティブフォームを作成します。
* データソースからフォームにフィールドをドラッグ&amp;ドロップします。
* 次に示すように、フォームの送信アクションを設定します
* ![送信アクション](assets/configure-af.png)



## フォームのテスト

前の手順で作成したフォームをプレビューします。 フォームに入力して送信します。 フォームのデータがSharePointリストに挿入されます。

