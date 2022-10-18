---
title: データソースの設定
description: MySQL データベースを指すデータソースを作成
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
source-git-commit: 30c882da3a89820b5e11bc2902bb92dd0629efe9
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 3%

---

# データソースの設定

AEMで外部データベースとの統合を有効にする方法は多数あります。 データベース統合の最も一般的で標準的な方法の 1 つは、Apache Sling 接続プールに入れられたデータソース設定プロパティを [configMgr](http://localhost:4502/system/console/configMgr).
最初の手順は、適切な [MySQL ドライバ](https://mvnrepository.com/artifact/mysql/mysql-connector-java) をAEM.
次に、データベースに固有の Sling Connection Pooled DataSource プロパティを設定します。 次のスクリーンショットは、このチュートリアルで使用する設定を示しています。 データベーススキーマは、このチュートリアルアセットの一部として提供されます。

![data-source](assets/data-source.JPG)


* JDBC ドライバークラス： `com.mysql.cj.jdbc.Driver`
* JDBC 接続 URI: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>データソースに名前を付けてください `StoreAndRetrieveAfData` これは OSGi サービスで使用される名前なので、


## データベースを作成


この使用例では、次のデータベースを使用しました。 データベースには、 `formdatawithattachments` 以下のスクリーンショットに示す 4 列の
![data-base](assets/table-schema.JPG)

* 列 **afdata** はアダプティブフォームデータを保持します。
* 列 **attachmentsInfo** はフォームの添付ファイルに関する情報を保持します。
* 列 **telephoneNumber** は、フォームに入力する人の携帯電話番号を保持します。

データベースをインポートして作成してください [データベーススキーマ](assets/data-base-schema.sql)
MySQL Workbench の使用

## フォームデータモデルの作成

フォームデータモデルを作成し、前の手順で作成したデータソースをベースにします。
の設定 **get** 以下のスクリーンショットに示すように、このフォームデータモデルのサービス。
が **get** サービス。

この目的 **get** サービスは、アプリケーション id に関連付けられている電話番号を取得します。

![get-service](assets/get-service.JPG)

このフォームデータモデルは、 **MyAccountForm** をクリックして、アプリケーション id に関連付けられた電話番号を取得します。
