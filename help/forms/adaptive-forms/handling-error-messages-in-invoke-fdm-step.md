---
title: ワークフローのステップとしてのフォームデータモデルサービスのエラーメッセージのキャプチャ
description: AEM Forms 6.5.1 以降では、AEM Workflow のステップとして「フォームデータモデルサービスを起動」を使用したときに生成されるエラーメッセージを取り込めるようになりました。 ワークフロー.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# 「フォームデータモデルサービスを起動」ステップでのエラーメッセージのキャプチャ

AEM Forms 6.5.1 以降では、エラーメッセージを取得して検証オプションを指定するオプションが追加されました。 「フォームデータモデルサービスを起動」ステップが強化され、以下の機能が提供されました。

* フォームデータモデルサービスの呼び出し時に発生した例外を処理する 3 層検証 (「OFF」、「BASIC」、「FULL」) のオプションを提供します。 3 つのオプションは、データベース固有の要件を確認するより厳しいバージョンを順に示します。
   ![validation-levels](assets/validation-level.PNG)

* ワークフローの実行をカスタマイズするためのチェックボックスを指定します。 したがって、「フォームデータモデルを起動」ステップで例外がスローされた場合でも、ワークフローの実行を柔軟に進めることができるようになりました。

* 検証の例外に起因して発生したエラーの重要な情報を保存します。 ErrorCode(String)、ErrorMessage(String) および ErrorDetails(JSON) を格納する関連する変数を選択するための、3 つのオートコンプリート型変数セレクターが組み込まれました。 ただし、例外が DermisValidationException でない場合は、ErrorDetails は null に設定されます。
   ![エラーメッセージの取得](assets/fdm-error-details.PNG)

これらの変更を受け、「フォームデータモデルサービスを起動」ステップでは、入力値が Swagger ファイルで指定されたデータ制約に従っていることを確認します。 例えば、次のエラーメッセージがスローされるのは、accountId と balance の値が Swagger ファイルで指定されたデータ制約に準拠していない場合です。

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
