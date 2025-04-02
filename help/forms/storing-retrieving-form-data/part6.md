---
title: MySQL データベースからのフォームデータの格納と取得 - デプロイ
description: フォームデータの保存と取得に関わる手順について説明する、複数のパートで構成されているチュートリアル
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: Experience Manager 6.4, Experience Manager 6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
duration: 47
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '243'
ht-degree: 100%

---

# お使いのサーバーにこれをデプロイする方法

>[!NOTE]
>
>これをシステムで実行するには、次の操作が必要です
>
>* AEM Forms（バージョン 6.3 以降）
>* MySql データベース

お使いの AEM Forms インスタンスでこの機能をテストするには、次の手順に従ってください。

* [felix web コンソール](http://localhost:4502/system/console/bundles)を使用して、[MySql Driver Jar](assets/mysqldriver.jar) ファイルをダウンロードしてデプロイします。
* [felix web コンソール](http://localhost:4502/system/console/bundles)を使用して、[OSGi バンドル](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar)をダウンロードしてデプロイします。
* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して、[クライアントライブラリ、アダプティブフォームテンプレート、カスタムページコンポーネントを含むパッケージ](assets/store-and-fetch-af-with-data.zip)をダウンロードしてインストールします
* [FormsAndDocuments インターフェイス](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)を使用して、[アダプティブフォームのサンプル](assets/sample-adaptive-form.zip)を読み込みます

* MySql Workbench を使用して、[form-data-db.sql](assets/form-data-db.sql) を次を読み込みます。これにより、このチュートリアルが機能するために必要なスキーマとテーブルがデータベースに作成されます。
* [configMgr にログインします。](http://localhost:4502/system/console/configMgr)「Apache Sling Connection Pooled DataSource」を検索します。 次のプロパティを使用して、**SaveAndContinue** という名前の、新しい Apache Sling Connection Pooled Datasource エントリを作成します。

| プロパティ名 | 値 |
| ------------------------|---------------------------------------|
| データソース名 | `SaveAndContinue` |
| JDBC ドライバークラス | `com.mysql.cj.jdbc.Driver` |
| JDBC 接続 URI | `jdbc:mysql://localhost:3306/aemformstutorial` |

* [アダプティブフォーム](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)を開きます。
* 詳細を入力し、「保存して後で続行」ボタンをクリックします
* GUID を含んだ URL が返されます。
* URL をコピーして、新しいブラウザータブに貼り付けます。 **URL の末尾に空のスペースがないことを確認します。**
* アダプティブフォームには、前の手順で作成したデータが入力されます。
