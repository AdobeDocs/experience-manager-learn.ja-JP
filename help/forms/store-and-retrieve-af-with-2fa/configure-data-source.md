---
title: データソースの設定
description: MySQLデータベースを指すDataSourceを作成します
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 3%

---


# データソースの設定

AEMを使用して外部データベースとの統合を可能にする方法は多数あります。 データベース統合の最も一般的で標準的な方法の1つは、[configMgr](http://localhost:4502/system/console/configMgr)を介してApache Sling接続プールされたDataSource設定プロパティを使用することです。
最初の手順は、適切な[MySQLドライバ](https://mvnrepository.com/artifact/mysql/mysql-connector-java)をAEMにダウンロードし、展開することです。
次に、Sling Connection Pooled DataSourceプロパティをデータベースに固有に設定します。 次のスクリーンショットは、このチュートリアルで使用する設定を示しています。 データベーススキーマは、このチュートリアルアセットの一部として提供されます。

![データソース](assets/data-source.JPG)


* JDBCドライバークラス：`com.mysql.cj.jdbc.Driver`
* JDBC接続URI:`jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>データソース`StoreAndRetrieveAfData`はOSGiサービスで使用されている名前なので、必ず名前を付けてください。


## データベースの作成


この使用例では、次のデータベースが使用されています。 データベースには`formdatawithattachments`という名前のテーブルが1つあり、下のスクリーンショットに示すように、4つの列があります。
![データベース](assets/table-schema.JPG)

* **afdata**&#x200B;列にはアダプティブフォームデータが格納されます。
* 列&#x200B;**attachmentsInfo**&#x200B;には、フォームの添付ファイルに関する情報が格納されます。
* **telephoneNumber**&#x200B;列には、フォームに入力した人のモバイル番号が格納されます。

[データベーススキーマ](assets/data-base-schema.sql)をインポートしてデータベースを作成してください
MySQL Workbenchの使用」を参照してください。

## フォームデータモデルを作成

フォームデータモデルを作成し、前の手順で作成したデータソースに基づきます。
以下のスクリーンショットに示すように、このフォームデータモデルの**get**サービスを設定します。
**get**&#x200B;サービスで配列が返されていないことを確認してください。

この&#x200B;**get**&#x200B;サービスは、アプリケーションIDに関連付けられた電話番号を取得するために使用されます。

![get-service](assets/get-service.JPG)

次に、このフォームデータモデルを&#x200B;**MyAccountForm**&#x200B;で使用して、アプリケーションIDに関連付けられた電話番号を取得します。
