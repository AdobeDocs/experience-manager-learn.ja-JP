---
title: ワークフローステップを使用して SharePoint リストにデータを送信
description: FDM を呼び出しワークフローステップを使用して SharePoint リストにデータを挿入
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-15126
exl-id: b369ed05-ba25-4b0e-aa3b-e7fc1621067d
duration: 52
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: ht
source-wordcount: '296'
ht-degree: 100%

---

# FDM 呼び出しワークフローステップを使用して SharePoint リストにデータを挿入


この記事では、AEM ワークフローの FDM 呼び出しステップを使用して SharePoint リストにデータを挿入するために必要なステップについて説明します。

この記事は、[SharePoint リストにデータを送信するようにアダプティブフォームが正常に設定されている](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=ja#connect-af-sharepoint-list)ことを前提としています。


## SharePoint リストデータソースに基づいてフォームデータモデルを作成

* SharePoint リストデータソースに基づいて新しいフォームデータモデルを作成します。
* 適切なモデルと、フォームデータモデルの get サービスを追加します。
* トップレベルのモデルオブジェクトを挿入する挿入サービスを設定します。
* 挿入サービスをテストします。


## ワークフローの作成

* FDM 呼び出しステップを使用して単純なワークフローを作成します。
* 前のステップで作成したフォームデータモデルを使用する FDM 呼び出しステップを設定します。
* ![associate-fdm](assets/fdm-insert-1.png)

## コアコンポーネントに基づくアダプティブフォーム

次の形式でデータが送信されます。スクリーンショットに示すように、フォームデータモデルサービスを呼び出しワークフローステップでドット表記を使用して ContactUS オブジェクトを抽出する必要があります。

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


* ![map-input-parameters](assets/fdm-insert-2.png)


## 基盤コンポーネントに基づくアダプティブフォーム

次の形式でデータが送信されます。フォームデータモデルサービスを呼び出しワークフローステップでドット表記を使用して ContactUS JSON オブジェクトを抽出します。

```json
{
    "afData": {
        "afUnboundData": {
            "data": {}
        },
        "afBoundData": {
            "data": {
                "ContactUS": {
                    "Title": "Lord",
                    "HighNetWorth": "true",
                    "SubmitterName": "John Doe",
                    "Products": "Forms"
                }
            }
        },
        "afSubmissionInfo": {
            "lastFocusItem": "guide[0].guide1[0].guideRootPanel[0].afJsonSchemaRoot[0]",
            "stateOverrides": {},
            "signers": {},
            "afPath": "/content/dam/formsanddocuments/foundationform",
            "afSubmissionTime": "20240517100126"
        }
    }
}
```

![foundation-based-form](assets/foundation-based-form.png)

## AEM ワークフローをトリガーするアダプティブフォームの設定

* 前のステップで作成したフォームデータモデルを使用してアダプティブフォームを作成します。
* データソースからフォームに一部のフィールドをドラッグ＆ドロップします。
* 次に示すように、フォームの送信アクションを設定します
* ![送信アクション](assets/configure-af.png)



## フォームのテスト

前のステップで作成したフォームをプレビューします。フォームに入力して送信します。フォームのデータが SharePoint リストに挿入されます。
