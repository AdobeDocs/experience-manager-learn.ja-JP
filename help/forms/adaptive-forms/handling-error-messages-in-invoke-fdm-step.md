---
title: ワークフローのステップとしてのフォームデータモデルサービスのエラーメッセージを取り込む方法
description: AEM Forms 6.5.1 以降、「フォームデータモデルサービスを呼び出し」を AEM ワークフローのステップとして使用する際に発生する、エラーメッセージを取り込めるようになりました。ワークフロー。
feature: Workflow
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
last-substantial-update: 2020-06-09T00:00:00Z
duration: 51
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '249'
ht-degree: 100%

---

# 「フォームデータモデルサービスを呼び出し」ステップでのエラーメッセージの取り込み

AEM Forms 6.5.1 以降では、エラーメッセージを取り込んだり検証オプションを指定したりできるようになりました。「フォームデータモデルサービスを呼び出し」ステップが強化されて、以下の機能が提供されるようになりました。

* フォームデータモデルサービスの呼び出し時に発生した例外を処理する 3 層検証（「OFF」、「BASIC」、「FULL」）のオプションが用意されています。これら 3 つのオプションは、データベース固有の要件のチェックレベルを表し、順にチェックが厳しくなります。
  ![validation-levels](assets/validation-level.PNG)

* ワークフローの実行をカスタマイズするためのチェックボックスが用意されています。したがって、「フォームデータモデルを呼び出し」ステップで例外がスローされた場合でも、ワークフローの実行を柔軟に進めることができるようになりました。

* 検証の例外に起因して発生したエラーの重要な情報が保存されます。ErrorCode（文字列）、ErrorMessage（文字列）および ErrorDetails（JSON）を格納する適切な変数を選択するための オートコンプリートタイプの変数セレクターが 3 つ組み込まれました。ただし、例外が DermisValidationException でない場合は、ErrorDetails は null に設定されます。
  ![エラーメッセージの取り込み](assets/fdm-error-details.PNG)

これらの変更を受け、「フォームデータモデルサービスを呼び出し」ステップでは、入力値が、Swagger ファイルで指定されたデータ制約に従っていることを確認します。例えば、次のエラーメッセージがスローされるのは、accountId と balance の値が、Swagger ファイルで指定されたデータ制約に準拠していない場合です。

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
