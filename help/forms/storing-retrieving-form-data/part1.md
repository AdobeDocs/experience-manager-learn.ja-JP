---
title: MySQL データベースからのフォームデータの格納と取得 — データソースの設定
description: フォームデータの保存と取得に関する手順について説明するマルチパートチュートリアル
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 5%

---

# データソースの設定

AEMで外部データベースとの統合を有効にする方法は多数あります。 データベース統合の最も一般的で標準的な方法の 1 つは、Apache Sling 接続プールに入れられたデータソース設定プロパティを、 [configMgr](http://localhost:4502/system/console/configMgr).
最初の手順は、適切な [MySql ドライバ](https://mvnrepository.com/artifact/mysql/mysql-connector-java) AEMの
Apache Sling Connection Pooled DataSource を作成し、以下のスクリーンショットで指定されているプロパティを指定します。 データベーススキーマは、このチュートリアルアセットの一部として提供されます。

![data-source](assets/save-continue.PNG)

データベースには、以下のスクリーンショットに示すように、3 列の formdata というテーブルが 1 つあります。

![data-base](assets/data-base-tables.PNG)

スキーマを作成する SQL ファイルは、次のことができます。 [ここからダウンロード](assets/form-data-db.sql). MySql Workbench を使用してこのファイルをインポートし、スキーマとテーブルを作成する必要があります。

>[!NOTE]
>データソースに名前を付けてください **SaveAndContinue**. サンプルコードでは、という名前を使用してデータベースに接続します。

| プロパティ名 | 値 |
| ------------------------|---------------------------------------|
| Datasource Name | SaveAndContinue |
| JDBC ドライバークラス | com.mysql.cj.jdbc.Driver |
| JDBC 接続 URI | jdbc:mysql://localhost:3306/aemformstutorial |
