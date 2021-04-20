---
title: データベーステーブルの作成
description: フォームデータモデルで使用するデータベースの作成
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: 開発
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 2%

---


# データベーステーブルの作成

フォームデータモデルは、RDBMS、RESTfull、SOAP、ODataソースに基づく場合があります。 このコースでは、RDBMSデータソースによるフォームデータモデルを使用したアダプティブフォームの事前入力に重点を置いています。 このチュートリアルでは、MYSQLデータベースを使用しました。 使用例を示す次の2つのテーブルを作成しました

* **newhiretable — このテーブルには、新しい情報が格納されます。** 

   ![newhire](assets/newhire-table.png)


* **受** 益者安定所有者

   ![受益者](assets/beneficiaries-table.png)

MySQL Workbenchを使用して[sqlファイル](assets/db-schema.sql)を読み込み、サンプルデータを含むテーブルに作成できます。