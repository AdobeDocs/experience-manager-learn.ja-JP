---
title: MySQL データベースからのフォームデータの格納と取得 — デプロイ
description: フォームデータの保存と取得に関する手順について説明するマルチパートチュートリアル
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.3,6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 11%

---

# サーバーにデプロイ

>[!NOTE]
>
>これをシステムで実行するには、次の操作が必要です
>
>* AEM Forms（バージョン 6.3 以降）
>* MySql データベース


AEM Formsインスタンスでこの機能をテストするには、次の手順に従ってください

* をダウンロードしてデプロイします。 [MySql Driver Jar](assets/mysqldriver.jar) ファイルを [felix web コンソール](http://localhost:4502/system/console/bundles)
* をダウンロードしてデプロイします。 [OSGi バンドル](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) の使用 [felix web コンソール](http://localhost:4502/system/console/bundles)
* をダウンロードしてインストールする [クライアントライブラリ、アダプティブフォームテンプレート、カスタムページコンポーネントを含むパッケージ](assets/store-and-fetch-af-with-data.zip) の使用 [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)
* 次をインポート： [アダプティブフォームのサンプル](assets/sample-adaptive-form.zip) の使用 [FormsAndDocuments インターフェイス](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 次をインポート： [form-data-db.sql](assets/form-data-db.sql) MySql Workbench の使用 これにより、このチュートリアルが機能するために必要なスキーマとテーブルがデータベースに作成されます。
* ログイン先 [configMgr.](http://localhost:4502/system/console/configMgr) 「Apache Sling Connection Pooled DataSource」を検索します。 次の名前の新しい Apache Sling 接続プール済みデータソースエントリを作成します。 **SaveAndContinue** 次のプロパティを使用します。

| プロパティ名 | 値 |
| ------------------------|---------------------------------------|
| Datasource Name | SaveAndContinue |
| JDBC ドライバークラス | com.mysql.cj.jdbc.Driver |
| JDBC 接続 URI | jdbc:mysql://localhost:3306/aemformstutorial |

* を開きます。 [アダプティブフォーム](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* 詳細を入力し、「保存して後で続行」ボタンをクリックします。
* GUID が含まれる URL を返す必要があります。
* URL をコピーして、新しいブラウザータブに貼り付けます。 **URL の末尾に空のスペースがないことを確認します。**
* アダプティブフォームには、前の手順で作成したデータが入力されます。
