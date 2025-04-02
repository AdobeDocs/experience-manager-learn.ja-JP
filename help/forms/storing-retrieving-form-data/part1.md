---
title: MySQL データベースからのフォームデータの格納と取得 - データソースの設定
description: フォームデータの保存と取得に関わる手順について説明する、複数のパートで構成されているチュートリアル
version: Experience Manager 6.4, Experience Manager 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
duration: 36
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '187'
ht-degree: 100%

---

# データソースの設定

AEM で外部データベースとの統合を有効にする方法は多数あります。 データベース統合の最も一般的で標準的な方法の 1 つは、[configMgr](http://localhost:4502/system/console/configMgr) を通じて Apache Sling Connection Pooled DataSource 設定プロパティを使用することです。
最初のステップは、適切な [MySql ドライバー](https://mvnrepository.com/artifact/mysql/mysql-connector-java) を AEM にダウンロードしてデプロイすることです。
Apache Sling Connection Pooled DataSource を作成し、以下のスクリーンショットで指定されているプロパティを指定します。 データベーススキーマは、このチュートリアルアセットの一部として提供されます。

![data-source](assets/save-continue.PNG)

データベースには、次のスクリーンショットに示すように、3 つの列を持つ formdata という 1 つのテーブルがあります。

![data-base](assets/data-base-tables.PNG)

スキーマを作成するための SQL ファイルは、[ここからダウンロードできます](assets/form-data-db.sql)。MySql Workbench を使用してこのファイルを読み込んで、スキーマとテーブルを作成する必要があります。

>[!NOTE]
>データソースに **SaveAndContinue** という名前を付けてください。サンプルコードでは、この名前を使用してデータベースに接続します。

| プロパティ名 | 値 |
| ------------------------|---------------------------------------|
| データソース名 | `SaveAndContinue` |
| JDBC ドライバークラス | `com.mysql.cj.jdbc.Driver` |
| JDBC 接続 URI | `jdbc:mysql://localhost:3306/aemformstutorial` |
