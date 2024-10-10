---
title: データベーステーブルの作成
description: フォームデータモデルで使用するデータベースを作成する
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
duration: 21
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '105'
ht-degree: 100%

---

# データベーステーブルの作成

フォームデータモデルは、RDBMS、RESTfull、SOAP、OData のいずれかのソースに基づいて作成できます。 このコースでは、RDBMS データソースに基づくフォームデータモデルを使用したアダプティブフォームの事前入力に焦点を当てます。 このチュートリアルの目的では、MYSQL データベースが使用されました。 以下の 2 つの表を作成して、使用例を示しました

* **新規採用者**&#x200B;テーブル - このテーブルには、新規採用者の情報が格納されます。

  ![新規採用者](assets/newhire-table.png)


* **受取人**&#x200B;テーブル - 新規採用者の受取人を格納します

  ![受取人](assets/beneficiaries-table.png)

MySQL ワークベンチを使用して [sql ファイル](assets/db-schema.sql)を読み込み、いくつかのサンプルデータを含むテーブルを作成できます。

## 次の手順

[フォームデータモデルの設定](./configuring-form-data-model.md)
