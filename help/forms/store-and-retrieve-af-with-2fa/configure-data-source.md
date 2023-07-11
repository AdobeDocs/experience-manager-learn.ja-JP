---
title: データソースの設定
description: MySQL データベースを指すデータソースを作成する
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: ht
source-wordcount: '303'
ht-degree: 100%

---

# データソースの設定

AEM で外部データベースとの統合を有効にする方法は多数あります。データベース統合において最も一般的で標準的な方法の 1 つは、[configMgr](http://localhost:4502/system/console/configMgr) を介した Apache Sling Connection Pooled DataSource の設定プロパティを使用することです。
最初の手順では、適切な [MySQL ドライバー](https://mvnrepository.com/artifact/mysql/mysql-connector-java)を AEM にダウンロードしてデプロイします。
次に、データベースに固有の Sling Connection Pooled DataSource プロパティを設定します。次のスクリーンショットは、このチュートリアルで使用する設定を示しています。データベーススキーマは、このチュートリアルアセットの一部として提供されます。

![data-source](assets/data-source.JPG)


* JDBC ドライバークラス：`com.mysql.cj.jdbc.Driver`
* JDBC 接続 URI：`jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>OSGi サービスで使用される名前なので、データソース `StoreAndRetrieveAfData` に名前を付けてください。


## データベースの作成


このユースケースでは、次のデータベースが使用されました。データベースには、以下のスクリーンショットに示すような 4 つの列を持つ `formdatawithattachments` と呼ばれるテーブルがあります。
![data-base](assets/table-schema.JPG)

* 列 **afdata** には、アダプティブフォームデータが格納されます。
* 列 **attachmentsInfo** には、フォームの添付ファイルに関する情報が格納されます。
* 列 **telephoneNumber** には、フォームを入力する人の携帯電話番号が格納されます。

MySQL Workbench で、[データベーススキーマ](assets/data-base-schema.sql)を読み込んで
データベースを作成してください。

## フォームデータモデルの作成

フォームデータモデルを作成し、前の手順で作成したデータソースをベースにします。
このフォームデータモデルの **GET** サービスを以下のスクリーンショットに示すように設定します。
**GET** サービスで配列を返していないことを確認してください。

この **GET** サービスの目的は、アプリケーション ID に関連付けられている電話番号を取得することです。

![get-service](assets/get-service.JPG)

このフォームデータモデルは、**MyAccountForm** で使用され、アプリケーション ID に関連付けられた電話番号を取得します。

## 次の手順

[フォームの添付ファイルを保存するコードの記述](./store-form-attachments.md)
