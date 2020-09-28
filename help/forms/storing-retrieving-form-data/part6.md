---
title: MySQLデータベースからのフォームデータの格納と取得
description: フォームデータの格納と取得に関する手順について説明するマルチパートチュートリアル
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 6ae8110d4f4bc80682c35b0dab3fe7a62cad88f3
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 6%

---


# これをサーバーに展開する

>[!NOTE]
システム上でAEM Database（バージョン6.3以降）MYSQLデータベースを実行するには、次が必要です

この機能をAEM Formsインスタンスでテストするには、次の手順に従ってください

* チュートリアルアセットをローカルシステムにダウンロードして [解凍し](assets/store-retrieve-form-data.zip) ます。
* Felix Webコンソールを使用して、techmarketingdemos.jarおよびmysqldriver.jarバンドルをデプロイおよび開始します。 [](http://localhost:4502/system/console/configMgr)
* MYSQL Workbenchを使用して、aemformstutorial.sqlを読み込みます。 これにより、このチュートリアルを機能させるために必要なスキーマとテーブルがデータベースに作成されます。
* AEMパッケージマネージャーを使用してStoreAndRetrieve.zipを読み込み [ます。](http://localhost:4502/crx/packmgr/index.jsp) このパッケージには、アダプティブフォームテンプレート、ページコンポーネントのクライアントライブラリ、アダプティブフォームとデータソースのサンプル設定が含まれています。
* configMgrにログインし [ます。](http://localhost:4502/system/console/configMgr) 「Apache Sling Connection Pooled DataSource」を検索します。 電子メールのチュートリアルに関連付けられたデータソースエントリを開き、データベースインスタンスに固有のユーザー名とパスワードを入力します。
* アダプティブ [フォームを開く](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* 詳細を入力し、「保存して後で続行」ボタンをクリックします
* GUIDが含まれるURLを取得する必要があります。
* URLをコピーして、新しいブラウザータブに貼り付けます。 **URLの末尾に空白がないことを確認します。**
* アダプティブフォームには、前の手順のデータが入力されます
