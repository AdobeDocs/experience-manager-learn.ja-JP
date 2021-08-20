---
title: データベーステーブルの作成
description: フォームデータモデルで使用するデータベースを作成する
feature: アダプティブフォーム
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: 開発
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 2%

---


# データベーステーブルの作成

フォームデータモデルは、RDBMS、RESTfull、SOAP、ODataの各ソースに基づいています。 このコースでは、RDBMSデータソースに基づくフォームデータモデルを使用したアダプティブフォームの事前入力に焦点を当てます。 このチュートリアルでは、MYSQLデータベースを使用しました。 以下の2つの表を作成し、使用例を示します

* **** newhiretable — このテーブルには、新しい情報が格納されます

   ![根性](assets/newhire-table.png)


* **** 受益者安定 — 受益者がいない店舗

   ![受益者](assets/beneficiaries-table.png)

[sqlファイル](assets/db-schema.sql)は、MySQL Workbenchを使用してインポートし、サンプルデータを含むテーブルに作成できます。