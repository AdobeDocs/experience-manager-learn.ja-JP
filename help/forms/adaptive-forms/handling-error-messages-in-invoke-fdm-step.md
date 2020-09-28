---
title: ワークフローの手順としてのForm Data Modelサービスでのエラーメッセージの取得
seo-title: ワークフローの手順としてのForm Data Modelサービスでのエラーメッセージの取得
description: AEM Forms6.5.1以降では、AEMワークフローのステップとして「フォームデータモデルサービスの呼び出し」を使用した場合に生成されるエラーメッセージを取り込めるようになりました。 ワークフロー.
seo-description: AEM Forms6.5.1以降では、AEMワークフローのステップとして「フォームデータモデルサービスの呼び出し」を使用した場合に生成されるエラーメッセージを取り込めるようになりました。 ワークフロー.
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.5.1,6.5.2
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---


# Invoke Form Data Modelサービスの手順でのエラーメッセージの取得

AEM Forms6.5.1以降では、エラーメッセージを取得して検証オプションを指定するオプションが追加されました。 「Invoke Form Data Model Service」手順が強化され、以下の機能が提供されました。

* Form Data Model Serviceの呼び出し時に発生した例外を処理する3層の検証（「OFF」、「BASIC」および「FULL」）のオプションを指定する。 3つのオプションは、データベース固有の要件をより厳密にチェックするバージョンを順に示します。
   ![検証レベル](assets/validation-level.PNG)

* ワークフローの実行をカスタマイズするためのチェックボックスの指定 したがって、フォームデータモデルの呼び出しの手順でExceptionsがスローされた場合でも、ワークフローの実行を柔軟に実行できるようになりました。

* 検証の例外に起因するエラーの重要な情報の保存。 ErrorCode(String)、ErrorMessage(String)およびErrorDetails(JSON)を格納する関連変数を選択するために、オートコンプリートタイプの変数セレクターが3つ組み込まれています。 ただし、例外がCaremValidationExceptionでない場合は、ErrorDetailsはnullに設定されます。
   ![エラーメッセージの取得](assets/fdm-error-details.PNG)

これらの変更により、「Invoke Form Data Model Service」手順では、入力値がSwaggerファイルで指定されたデータ制約に従っていることを確認します。 例えば、accountId値とbalance値がSwaggerファイルで指定されたデータ制約に準拠していない場合、次のエラーメッセージがスローされます。

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


