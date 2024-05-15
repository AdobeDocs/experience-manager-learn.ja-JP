---
title: AEM ワークフローの変数 [第 4 部]
description: AEM ワークフローでの XML、JSON、ArrayList、Document タイプの変数の使用
version: 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: 269e43f7-24cf-4786-9439-f51bfe91d39c
duration: 102
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 100%

---

# AEM ワークフローの ArrayList 変数

AEM Forms 6.5 では、ArrayList 型の変数が導入されました。ArrayList 変数の一般的な使用例は、AssignTask で使用するカスタムルートを定義することです。

AEM ワークフローで ArrayList 変数を使用するには、送信されたデータに繰り返し要素を生成するアダプティブフォームを作成する必要があります。 一般的な方法は、配列要素を含むスキーマを定義することです。 この記事の目的で、配列要素を含む単純な JSON スキーマを作成しました。 使用例は、従業員が経費報告書を入力する場合です。 経費報告書では、提出者のマネージャー名と、マネージャーのマネージャー名を取り込みます。 マネージャー名は managerchain と呼ばれる配列に格納されます。 次のスクリーンショットは、経費報告フォームと、アダプティブフォーム送信からのデータを示しています。

![expensereport](assets/expensereport.jpg)

アダプティブフォームの送信データを次に示します。 アダプティブフォームは JSON スキーマに基づいており、スキーマにバインドされたデータは、afBoundData 要素の data 要素の下に保存されます。managerchain は配列であり、ArrayList に managerchain 配列内のオブジェクトの名前要素を設定する必要があります。

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

サブタイプ文字列の ArrayList 変数を初期化するには、JSON ドット表記または XPath マッピングモードを使用します。次のスクリーンショットは、JSON ドット表記を使用して CustomRoutes と呼ばれる ArrayList 変数を生成する方法を示しています。次のスクリーンショットに示すように、配列オブジェクト内の要素を指していることを確認します。 CustomRoutes ArrayList に managerchain 配列オブジェクトの名前を入力します。
次に、CustomRoutes ArrayList を使用して、AssignTask コンポーネント内の Routes にデータを設定します
![customroutes](assets/arraylist.jpg)
CustomRoutes ArrayList 変数が送信データからの値で初期化されると、AssignTask コンポーネントの Routes が CustomRoutes 変数を使用して設定されます。 以下のスクリーンショットは、AssignTask のカスタムルートを示しています
![asingtask](assets/customactions.jpg)

お使いのシステム上でこのワークフローをテストするには、次の手順に従ってください

* ArrayListVariable.zip ファイルをダウンロードしてファイルシステムに保存します
* AEM パッケージマネージャーを使用して [zip ファイルを読み込みます](assets/arraylistvariable.zip) 
* [TravelExpenseReport フォームを開きます](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* 費用を 2 つ入力し、2 人のマネージャーの名前を入力します
* 「送信」ボタンを押します
* [インボックスを開きます](http://localhost:4502/aem/inbox)
* 「経費管理者に割り当て」というタイトルの新しいタスクが表示されます
* タスクに関連付けられたフォームを開きます
* マネージャー名を持つ 2 つのカスタムルートが表示されます
  [ReviewExpenseReportWorkflow を探索します。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) このワークフローでは、ArrayList 変数、JSON 型変数、Or-Sokut コンポーネントのルールエディターを使用します
