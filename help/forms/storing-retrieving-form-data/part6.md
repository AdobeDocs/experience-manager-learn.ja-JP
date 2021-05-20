---
title: MySQLデータベースからのフォームデータの格納と取得
description: フォームデータの保存と取得に関する手順について説明するマルチパートチュートリアル
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 12%

---


# サーバーにデプロイします

>[!NOTE]
>
>これをシステムで実行するには、次の操作が必要です
>
>* AEM Forms（バージョン6.3以降）
>* MySqlデータベース


AEM Formsインスタンスでこの機能をテストするには、次の手順に従います

* [felix Webコンソール](http://localhost:4502/system/console/bundles)を使用して、[MySql Driver Jar](assets/mysqldriver.jar)ファイルをダウンロードしてデプロイします。
* [felix Webコンソール](http://localhost:4502/system/console/bundles)を使用して、[OSGiバンドル](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar)をダウンロードしてデプロイします。
* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して、クライアントライブラリ、アダプティブフォームテンプレート、カスタムページコンポーネント](assets/store-and-fetch-af-with-data.zip)を含む[パッケージをダウンロードし、インストールします。
* [FormsAndDocumentsインターフェイス](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)を使用して、[サンプルのアダプティブフォーム](assets/sample-adaptive-form.zip)を読み込みます。

* MySql Workbenchを使用して、[form-data-db.sql](assets/form-data-db.sql)を読み込みます。 これにより、このチュートリアルが機能するために必要なスキーマとテーブルがデータベースに作成されます。
* [configMgrにログインします。](http://localhost:4502/system/console/configMgr) 「Apache Sling Connection Pooled DataSource」を検索します。次のプロパティを使用して、新しいApache Sling接続プール済みデータソースエントリ&#x200B;**SaveAndContinue**&#x200B;を作成します。

| プロパティ名 | 値 |
------------------------|---------------------------------------
| Datasource Name | SaveAndContinue |
| JDBCドライバクラス | com.mysql.cj.jdbc.Driver |
| JDBC接続uri | jdbc:mysql://localhost:3306/aemformstutorial |


* [アダプティブフォーム](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)を開きます。
* 詳細を入力し、「保存して後で続行」ボタンをクリックします。
* GUIDが含まれるURLを返す必要があります。
* URLをコピーして、新しいブラウザータブに貼り付けます。 **URLの末尾に空のスペースがないことを確認します。**
* アダプティブフォームには、前の手順で作成したデータが入力されます。
