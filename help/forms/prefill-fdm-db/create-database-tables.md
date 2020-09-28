---
title: データベーステーブルの作成
description: フォームデータモデルで使用するデータベースの作成
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
translation-type: tm+mt
source-git-commit: b085a2c75f8e0b4860d503774ea01a108773ad09
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 0%

---


# データベーステーブルの作成

フォームデータモデルは、RDBMS、RESTfull、SOAP、ODataソースに基づく場合があります。 このコースでは、RDBMSデータソースによるフォームデータモデルを使用したアダプティブフォームの事前入力に重点を置いています。 このチュートリアルでは、MYSQLデータベースを使用しました。 使用例を示す次の2つのテーブルを作成しました

* **newhire** table — このテーブルには、新しい情報が格納されます。

   ![newhire](assets/newhire-table.png)


* **受益者表** — 受益者の少ない店舗

   ![受益者](assets/beneficiaries-table.png)

MySQL Workbenchを使用して [SQLファイルを読み込み](assets/db-schema.sql) 、サンプルデータを含むテーブルに作成できます。