---
title: MySQLデータベースからのフォームデータの格納と取得
description: フォームデータの保存と取得に関する手順について説明するマルチパートチュートリアル
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 6%

---

# データソースの設定

AEMで外部データベースとの統合を有効にする方法は多数あります。 データベース統合の最も一般的で標準的な方法の1つは、[configMgr](http://localhost:4502/system/console/configMgr)を通じてApache Sling Connection Pooled DataSource設定プロパティを使用することです。
最初の手順は、適切な[MySqlドライバー](https://mvnrepository.com/artifact/mysql/mysql-connector-java)をAEMにダウンロードしてデプロイすることです。
Apache Sling接続プールに入れられたデータソースを作成し、以下のスクリーンショットで指定されているプロパティを指定します。 データベーススキーマは、このチュートリアルアセットの一部として提供されます。

![data-source](assets/save-continue.PNG)

以下のスクリーンショットに示すように、データベースにはformdataと呼ばれる1つの列と3つの列があります。

![データベース](assets/data-base-tables.PNG)

スキーマを作成するSQLファイルは、[ここから](assets/form-data-db.sql)ダウンロードできます。 MySql Workbenchを使用してこのファイルをインポートし、スキーマとテーブルを作成する必要があります。

>[!NOTE]
>データソースに&#x200B;**SaveAndContinue**&#x200B;という名前を付けてください。 サンプルコードでは、という名前を使用してデータベースに接続します。

| プロパティ名 | 値 |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| JDBCドライバクラス | com.mysql.cj.jdbc.Driver |
| JDBC接続uri | jdbc:mysql://localhost:3306/aemformstutorial |


