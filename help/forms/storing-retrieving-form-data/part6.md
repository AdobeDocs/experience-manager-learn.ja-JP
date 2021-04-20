---
title: MySQLデータベースからのフォームデータの格納と取得
description: フォームデータの格納と取得に関する手順について説明するマルチパートチュートリアル
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 12%

---


# これをサーバーに展開する

>[!NOTE]
>
>これをシステムで実行するには、以下が必要です
>
>* AEM Forms（バージョン6.3以降）
>* MySqlデータベース


この機能をAEM Formsインスタンスでテストするには、次の手順に従ってください

* [felix Webコンソール](http://localhost:4502/system/console/bundles)を使用して、[MySql Driver Jar](assets/mysqldriver.jar)ファイルをダウンロードし、展開します
* [felix Webコンソール](http://localhost:4502/system/console/bundles)を使用して[OSGiバンドル](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar)をダウンロードし、展開します
* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して、クライアントlib、アダプティブフォームテンプレート、カスタムページコンポーネント](assets/store-and-fetch-af-with-data.zip)を含む[パッケージをダウンロードし、インストールします
* [FormsAndDocumentsインターフェイス](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)を使用して[サンプルのアダプティブフォーム](assets/sample-adaptive-form.zip)を読み込みます

* MySql Workbenchを使用して[form-data-db.sql](assets/form-data-db.sql)を読み込みます。 これにより、このチュートリアルを機能させるために必要なスキーマとテーブルがデータベースに作成されます。
* [configMgrにログインします。](http://localhost:4502/system/console/configMgr) 「Apache Sling Connection Pooled DataSource」を検索します。次のプロパティを使用して、**SaveAndContinue**&#x200B;という名前の新しいApache Sling接続プール済みデータソースエントリを作成します。

| プロパティ名 | 値 |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| JDBCドライバークラス | com.mysql.cj.jdbc.Driver |
| JDBC接続URI | jdbc:mysql://localhost:3306/aemformstutorial |


* [アダプティブフォーム](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)を開きます
* 詳細を入力し、「保存して後で続行」ボタンをクリックします。
* GUIDが含まれるURLを取得する必要があります。
* URLをコピーして、新しいブラウザータブに貼り付けます。 **URLの末尾に空白がないことを確認します。**
* アダプティブフォームには、前の手順のデータが入力されます。
