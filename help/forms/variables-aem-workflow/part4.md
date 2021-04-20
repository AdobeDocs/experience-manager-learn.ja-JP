---
title: AEMワークフロー内の変数[パート4]
seo-title: AEMワークフロー内の変数[パート4]
description: aemワークフローでのxml,json,arraylist,ドキュメント型の変数の使用
seo-description: aemワークフローでのxml,json,arraylist,ドキュメント型の変数の使用
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 0%

---


# AEMワークフローのArrayList変数

ArrayList型の変数は、AEM Forms6.5で導入されました。ArrayList変数の一般的な使用例は、AssignTaskで使用するカスタムルートを定義することです。

AEMワークフローでArrayList変数を使用するには、送信済みデータ内で繰り返し要素を生成するアダプティブフォームを作成する必要があります。 一般的に、配列要素を含むスキーマを定義します。 この記事の目的で、配列要素を含む単純なJSONスキーマを作成しました。 使用例は、経費報告書に記入している従業員です。 経費精算書で、送信者の管理者名と管理者の管理者名を取得します。 マネージャの名前は、managerchainと呼ばれるアレイに格納されます。 下のスクリーンショットは、経費報告書フォームとアダプティブFormsの提出データを示しています。

![支出報告](assets/expensereport.jpg)

アダプティブフォーム送信のデータを以下に示します。 アダプティブフォームはJSONスキーマに基づいており、スキーマに連結されたデータはafBoundData要素のdata要素の下に格納されます。 managerchainは配列であり、managerchain配列内のオブジェクトのname要素をArrayListに入力する必要があります。

```json
{
    "afData": {
        "afUnboundData": {
            "data": {
                "numericbox_2762582281554154833426": 700
            }
        },
        "afBoundData": {
            "data": {
                "Employee": {
                    "Name": "Conrad Simms",
                    "Department": "IT",
                    "managerchain": [{
                        "name": "Gloria Rios"
                    }, {
                        "name": "John Jacobs"
                    }]
                },
                "expense": [{
                    "description": "Hotel",
                    "amount": 300
                }, {
                    "description": "Air Fare",
                    "amount": 400
                }]
            }
        },
        "afSubmissionInfo": {
            "computedMetaInfo": {},
            "stateOverrides": {},
            "signers": {},
            "afPath": "/content/dam/formsanddocuments/helpx/travelexpensereport",
            "afSubmissionTime": "20190402102953"
            }
        }
}
```

サブタイプ文字列のArrayList変数を初期化するには、JSONドット表記またはXPathマッピングモードを使用します。 次のスクリーンショットは、JSONドット表記を使用してCustomRoutesというArrayList変数を設定していることを示しています。 以下のスクリーンショットに示すように、配列オブジェクト内の要素を指していることを確認します。 CustomRoutes ArrayListにmanagerchain配列オブジェクトの名前を入力します。
次に、CustomRoutes ArrayListを使用して、AssignTaskコンポーネント内のルートを設定します
![customroutes](assets/arraylist.jpg)
CustomRoutes ArrayList変数が送信されたデータからの値で初期化されると、AssignTaskコンポーネントのルートは、CustomRoutes変数を使用して設定されます。 以下のスクリーンショットは、AssignTaskのカスタムルートを示しています
![asingtask](assets/customactions.jpg)

お使いのシステムでこのワークフローをテストするには、次の手順に従ってください

* ArrayListVariable.zipファイルをダウンロードし、ファイルシステムに保存します
* [AEM Package Managerを使用してzip](assets/arraylistvariable.zip) ファイルを読み込みます
* [TravelExpenseReportフォームを開きます](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* 2つの経費と2つの管理者の名前を入力します
* 送信ボタンを押す
* [受信トレイを開く](http://localhost:4502/aem/inbox)
* 「経費管理者に割り当て」という新しいタスクが表示されます。
* タスクに関連付けられたフォームを開く
* マネージャー名が付いた2つのカスタムルートが表示されます
   [ReviewExpenseReportWorkflowを調べます。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) このワークフローでは、ArrayList変数、JSON型変数、またはOr-Splitコンポーネントのルールエディターを使用します
