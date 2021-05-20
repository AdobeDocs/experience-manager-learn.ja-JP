---
title: AEMデータソースの設定
description: フォームデータを保存および取得するためのMySQLベースのデータソースの設定
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 7%

---

# データソースの設定

AEMを使用して、外部データベースとの統合を可能にする方法は多数あります。 データベースを統合する最も一般的な方法の1つは、[configMgr](http://localhost:4502/system/console/configMgr)を使用して、Apache Sling Connection Pooled DataSource設定プロパティを使用することです。
最初の手順は、適切な[MySqlドライバー](https://mvnrepository.com/artifact/mysql/mysql-connector-java)をAEMにダウンロードしてデプロイすることです。
Apache Sling接続プールに入れられたデータソースを作成し、以下のスクリーンショットで指定されているプロパティを指定します。 データベーススキーマは、このチュートリアルアセットの一部として提供されます。

![data-source](assets/data-source.PNG)

以下のスクリーンショットに示すように、データベースにはformdataと呼ばれる1つの列と3つの列があります。

![データベース](assets/data-base.PNG)


>[!NOTE]
>データソースに&#x200B;**aemformstutorial**&#x200B;という名前を付けてください。 サンプルコードでは、という名前を使用してデータベースに接続します。

| プロパティ名 | 値 |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| JDBCドライバクラス | com.mysql.cj.jdbc.Driver |
| JDBC接続uri | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

スキーマを作成するSQLファイルは、[ここから](assets/sign-multiple-forms.sql)ダウンロードできます。 MySql Workbenchを使用してこのファイルをインポートし、スキーマとテーブルを作成する必要があります。


