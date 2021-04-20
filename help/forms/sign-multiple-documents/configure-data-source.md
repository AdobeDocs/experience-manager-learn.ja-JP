---
title: AEMデータソースの設定
description: フォームデータを保存および取得するようにMySQLでバックアップされたデータソースを設定する
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
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 7%

---

# データソースの設定

AEMを使用して外部データベースとの統合を可能にする方法は多数あります。 データベースを統合する最も一般的な方法の1つは、[configMgr](http://localhost:4502/system/console/configMgr)を介してApache Sling接続プールされたDataSource設定プロパティを使用することです。
最初の手順は、適切な[MySqlドライバ](https://mvnrepository.com/artifact/mysql/mysql-connector-java)をAEMにダウンロードして展開することです。
Apache Sling接続プールされたデータソースを作成し、以下のスクリーンショットに指定されているプロパティを指定します。 データベーススキーマは、このチュートリアルアセットの一部として提供されます。

![データソース](assets/data-source.PNG)

データベースにはformdataと呼ばれるテーブルが1つあり、下のスクリーンショットに示すように、3つの列があります。

![データベース](assets/data-base.PNG)


>[!NOTE]
>データソースに&#x200B;**電子メールスチュートリアル**&#x200B;という名前を付けてください。 サンプルコードは、データベースに接続する際に名前を使用します。

| プロパティ名 | 値 |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| JDBCドライバークラス | com.mysql.cj.jdbc.Driver |
| JDBC接続URI | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

スキーマを作成するSQLファイルは、[ここから](assets/sign-multiple-forms.sql)ダウンロードできます。 スキーマとテーブルを作成するには、MySql Workbenchを使用してこのファイルを読み込む必要があります。


