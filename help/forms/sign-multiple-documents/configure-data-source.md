---
title: AEM データソースの設定
description: フォームデータを保存および取得するように MySQL ベースのデータソースを設定します
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: 6.4,6.5
jira: KT-6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
duration: 39
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 100%

---

# データソースの設定

AEM が外部データベースとの統合を可能にする方法は、多数あります。データベースを統合する最も一般的な方法の 1 つは、[configMgr](http://localhost:4502/system/console/configMgr) を介して Apache Sling Connection Pooled DataSource 設定プロパティを使用することです。最初のステップは、適切な [MySql ドライバー](https://mvnrepository.com/artifact/mysql/mysql-connector-java) を AEM にダウンロードしてデプロイすることです。
Apache Sling Connection Pooled DataSource を作成し、以下のスクリーンショットで指定されているプロパティを指定します。 データベーススキーマは、このチュートリアルアセットの一部として提供されます。

![data-source](assets/data-source.PNG)

データベースには、次のスクリーンショットに示すように、3 つの列を持つ formdata という 1 つのテーブルがあります。

![data-base](assets/data-base.PNG)


>[!NOTE]
>お使いのデータソースに **aemformstudation** という名前を付けるようにしてください。サンプルコードでは、この名前を使用してデータベースに接続します。

| プロパティ名 | 値 |
| ------------------------|--------------------------------------- |
| データソース名 | `SaveAndContinue` |
| JDBC ドライバークラス | `com.mysql.cj.jdbc.Driver` |
| JDBC 接続 URI | `jdbc:mysql://localhost:3306/aemformstutorial` |

## Assets

スキーマを作成する SQL ファイルは、[こちらからダウンロード](assets/sign-multiple-forms.sql)できます。MySql Workbench を使用してこのファイルを読み込んで、スキーマとテーブルを作成する必要があります。

## 次の手順

[データベースのデータを格納および取得するための OSGi サービスの作成](./create-osgi-service.md)
