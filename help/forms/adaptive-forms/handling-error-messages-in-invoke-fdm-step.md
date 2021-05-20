---
title: ワークフローのステップとしてのフォームデータモデルサービスのエラーメッセージの取り込み
seo-title: ワークフローのステップとしてのフォームデータモデルサービスのエラーメッセージの取り込み
description: AEM Forms 6.5.1以降では、「フォームデータモデルサービスを起動」をAEMワークフローのステップとして使用した場合に生成されるエラーメッセージを取り込めるようになりました。 ワークフロー.
seo-description: AEM Forms 6.5.1以降では、「フォームデータモデルサービスを起動」をAEMワークフローのステップとして使用した場合に生成されるエラーメッセージを取り込めるようになりました。 ワークフロー.
feature: ワークフロー
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.5.1,6.5.2
topic: 開発
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 1%

---


# 「フォームデータモデルサービスを起動」ステップでのエラーメッセージの取得

AEM Forms 6.5.1以降では、エラーメッセージをキャプチャして検証オプションを指定するオプションが追加されました。 「フォームデータモデルサービスを呼び出し」の手順が強化され、以下の機能が提供されました。

* フォームデータモデルサービスの呼び出し時に発生した例外を処理する3層検証(「OFF」、「BASIC」、「FULL」)のオプションを提供します。 3つのオプションは、データベース固有の要件を確認するより厳格なバージョンを順に示します。
   ![検証レベル](assets/validation-level.PNG)

* ワークフローの実行をカスタマイズするためのチェックボックスを指定します。 したがって、「フォームデータモデルを起動」ステップで例外がスローされても、ワークフローの実行を柔軟に実行できるようになりました。

* 検証の例外に起因して発生するエラーの重要な情報を格納します。 ErrorCode(String)、ErrorMessage(String)およびErrorDetails(JSON)を格納する関連変数を選択するために、3つのオートコンプリート型の変数セレクターが組み込まれています。 ただし、例外がDermisValidationExceptionでない場合は、ErrorDetailsはnullに設定されます。
   ![エラーメッセージのキャプチャ](assets/fdm-error-details.PNG)

これらの変更を反映し、「フォームデータモデルサービスを起動」ステップでは、入力値がSwaggerファイルで指定されたデータ制約に従っていることを確認します。 例えば、次のエラーメッセージは、 accountIdとbalanceの値がswaggerファイルで指定されたデータ制約に準拠していない場合にスローされます。

```json
{

"errorCode": "AEM-FDM-001-049"

"errorMessage": "Input validations failed during operation execution"

"violations": {

"/accountId": ["numeric instance is greater than the required maximum (maximum: 20, found: 97)"],

"/newAccount/balance": ["instance type (string) does not match any allowed primitive type (allowed: [\"integer\",\"number\"])"]

}

}
```


