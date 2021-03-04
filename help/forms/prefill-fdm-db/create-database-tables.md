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
role: デベロッパー
level: 初心者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '104'
ht-degree: 3%

---


# データベーステーブルの作成

フォームデータモデルは、RDBMS、RESTfull、SOAP、ODataソースに基づく場合があります。 このコースでは、RDBMSデータソースによるフォームデータモデルを使用したアダプティブフォームの事前入力に重点を置いています。 このチュートリアルでは、MYSQLデータベースを使用しました。 使用例を示す次の2つのテーブルを作成しました

* **newhiretable — このテーブルには、新しい情報が格納されます。** 

   ![newhire](assets/newhire-table.png)


* **受** 益者安定所有者

   ![受益者](assets/beneficiaries-table.png)

MySQL Workbenchを使用して[sqlファイル](assets/db-schema.sql)を読み込み、サンプルデータを含むテーブルに作成できます。