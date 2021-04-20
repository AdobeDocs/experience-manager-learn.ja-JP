---
title: MySQLデータベースからのフォームデータの格納と取得
description: フォームデータの格納と取得に関する手順について説明するマルチパートチュートリアル
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 開発
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 6%

---

# データソースの設定

AEMを使用して外部データベースとの統合を可能にする方法は多数あります。 データベース統合の最も一般的で標準的な方法の1つは、[configMgr](http://localhost:4502/system/console/configMgr)を介してApache Sling接続プールされたDataSource設定プロパティを使用することです。
最初の手順は、適切な[MySqlドライバ](https://mvnrepository.com/artifact/mysql/mysql-connector-java)をAEMにダウンロードして展開することです。
Apache Sling接続プールされたデータソースを作成し、以下のスクリーンショットに指定されているプロパティを指定します。 データベーススキーマは、このチュートリアルアセットの一部として提供されます。

![データソース](assets/save-continue.PNG)

データベースにはformdataと呼ばれるテーブルが1つあり、下のスクリーンショットに示すように、3つの列があります。

![データベース](assets/data-base-tables.PNG)

スキーマを作成するSQLファイルは、[ここから](assets/form-data-db.sql)ダウンロードできます。 スキーマとテーブルを作成するには、MySql Workbenchを使用してこのファイルを読み込む必要があります。

>[!NOTE]
>データソースに&#x200B;**SaveAndContinue**&#x200B;という名前を付けてください。 サンプルコードは、データベースに接続する際に名前を使用します。

| プロパティ名 | 値 |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| JDBCドライバークラス | com.mysql.cj.jdbc.Driver |
| JDBC接続URI | jdbc:mysql://localhost:3306/aemformstutorial |


