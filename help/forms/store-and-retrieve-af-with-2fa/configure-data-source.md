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

AEMを使用して外部データベースとの統合を可能にする方法は多数あります。 データベース統合の最も一般的で標準的な方法の1つは、configMgrを介してApache Sling Connection Pooled DataSource設定プロパティを使用すること [です](http://localhost:4502/system/console/configMgr)。
最初の手順は、適切な [MySQLドライバーをAEMにダウンロードして展開する](https://mvnrepository.com/artifact/mysql/mysql-connector-java) ことです。
次に、Sling Connection Pooled DataSourceプロパティをデータベースに固有に設定します。 次のスクリーンショットは、このチュートリアルで使用する設定を示しています。 データベーススキーマは、このチュートリアルアセットの一部として提供されます。

![データソース](assets/data-source.JPG)


* JDBC Driver Class: `com.mysql.cj.jdbc.Driver`
* JDBC Connection URI: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>データソースにはOSGiサービスで使用され `StoreAndRetrieveAfData` る名前が付いているので、名前を付けてください。


## データベースの作成


この使用例では、次のデータベースが使用されています。 このデータベースには、次のスクリーンショット `formdatawithattachments` に示すように、4つの列を持つという名前のテーブルが1つあります。
![データベース](assets/table-schema.JPG)

* afdata列には、アダプティブフォ **ームのデータが格納されます** 。
* 列 **attachmentsInfo** には、フォーム添付ファイルに関する情報が格納されます。
* 列 **telephoneNumber** には、フォームに入力した人のモバイル番号が格納されます。

MySQL Workbenchを使用して [データベーススキーマを読み込んでデータベースを作成し](assets/data-base-schema.sql)てください。

## フォームデータモデルを作成

フォームデータモデルを作成し、前の手順で作成したデータソースに基づきます。
以下のスクリーンショットに示すように、このフォームデータモデルの **get** serviceを設定します。
**get** サービスで配列が返されていないことを確認します。

この **get** サービスは、アプリケーションIDに関連付けられた電話番号を取得するために使用されます。

![get-service](assets/get-service.JPG)

次に、このフォームデータモデルがMyAccountFormで使用され **、アプリケーションIDに関連付けられた電話番号が取得されます** 。
