---
title: データソースの設定
description: MySQLデータベースを指すデータソースの作成
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 4%

---


# データソースの設定

AEMで外部データベースとの統合を有効にする方法は多数あります。 データベース統合の最も一般的で標準的な方法の1つは、[configMgr](http://localhost:4502/system/console/configMgr)を通じてApache Sling Connection Pooled DataSource設定プロパティを使用することです。
最初の手順は、適切な[MySQLドライバ](https://mvnrepository.com/artifact/mysql/mysql-connector-java)をAEMにダウンロードしてデプロイすることです。
次に、データベースに固有のSling接続プールに入れられたデータソースプロパティを設定します。 次のスクリーンショットは、このチュートリアルで使用する設定を示しています。 データベーススキーマは、このチュートリアルアセットの一部として提供されます。

![data-source](assets/data-source.JPG)


* JDBCドライバクラス：`com.mysql.cj.jdbc.Driver`
* JDBC接続URI:`jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>データソースに`StoreAndRetrieveAfData`という名前を付けてください。これはOSGiサービスで使用される名前です。


## データベースの作成


この使用例では、次のデータベースを使用しました。 データベースには、`formdatawithattachments`という1つのテーブルがあり、下のスクリーンショットに示す4つの列があります。
![データベース](assets/table-schema.JPG)

* 列&#x200B;**afdata**&#x200B;にはアダプティブフォームのデータが格納されます。
* 列&#x200B;**attachmentsInfo**&#x200B;には、フォームの添付ファイルに関する情報が格納されます。
* 列&#x200B;**telephoneNumber**&#x200B;には、フォームの記入者の携帯電話番号が格納されます。

[データベーススキーマ](assets/data-base-schema.sql)をインポートして、データベースを作成してください
MySQL Workbenchを使用する。

## フォームデータモデルを作成

フォームデータモデルを作成し、前の手順で作成したデータソースをベースにします。
以下のスクリーンショットに示すように、このフォームデータモデルの**get**サービスを設定します。
**get**&#x200B;サービスで配列が返されていないことを確認します。

この&#x200B;**get**&#x200B;サービスは、アプリケーションIDに関連付けられた電話番号を取得するために使用されます。

![get-service](assets/get-service.JPG)

次に、このフォームデータモデルが&#x200B;**MyAccountForm**&#x200B;で使用され、アプリケーションIDに関連付けられた電話番号が取得されます。
