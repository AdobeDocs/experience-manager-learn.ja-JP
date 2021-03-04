---
title: アダプティブフォームデータの保存
seo-title: アダプティブフォームデータの保存
description: アダプティブフォームデータのDataBaseへの格納(AEMワークフローの一部)
seo-description: アダプティブフォームデータのDataBaseへの格納(AEMワークフローの一部)
feature: アダプティブフォーム，ワークフロー
topics: integrations
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 1%

---


# アダプティブフォーム送信のデータベースへの保存

送信されたフォームデータを任意のデータベースに格納する方法はいくつかあります。 JDBCデータソースを使用して、データを直接データベースに格納できます。 カスタムOSGIバンドルを書き込んで、データをデータベースに格納できます。 この記事では、AEMワークフローのカスタムプロセスステップを使用してデータを保存します。
使用例は、アダプティブフォームの送信時にAEMワークフローをトリガーし、ワークフローの手順で送信されたデータをデータベースに保存する場合です。

**ご使用のシステムで動作させるには、次の手順に従ってください**

* [Zipファイルをダウンロードし、その内容をハードドライブに展開します](assets/storeafdataindb.zip)

   * パッケージマネージャーを使用して、StoreAFInDBWorkflow.zipをAEMに読み込みます。 パッケージには、AFデータをDBに格納するサンプルワークフローが含まれています。 ワークフローモデルを開きます。 ワークフローには1つのステップしかありません。 この手順では、バンドルに書き込まれたコードを呼び出し、AFデータをデータベースに格納します。 私はそのプロセスに1つの引数を渡す。 データを保存するアダプティブフォームの名前です。
   * Felix Webコンソールを使用して、insertdata.core-0.0.1-SNAPSHOT.jarをデプロイします。 このバンドルには、送信されたフォームデータをデータベースに書き込むためのコードが含まれています

* [ConfigMgr](http://localhost:4502/system/console/configMgr)に移動

   * 「JDBC Connection Pool」を検索します。 新しいDay Commons JDBC Connection Poolを作成します。 データベースに固有の設定を指定します。

   * ![jdbc接続プール](assets/jdbc-connection-pool.png)
   * 「**Insert Form Data Into DB**」を検索します。
   * データベースに固有のプロパティを指定します。
      * DataSourceName：前に設定したデータソースの名前。
      * TableName - AFデータを格納するテーブルの名前
      * FormName — フォーム名を保持する列名
      * ColumnName - AFデータを保持する列名

   ![insertdata](assets/insertdata.PNG)

* アダプティブフォームの作成を参照してください。

* 以下のスクリーンショットに示すように、アダプティブフォームをAEM Workflow(StoreAFValusinDB)に関連付けます。

* 以下のスクリーンショットに示すように、データファイルのパスに「data.xml」を必ず指定してください。

   ![提出](assets/submissionafforms.png)

* フォームのプレビューと送信

* すべてうまくいけば、指定したテーブルと列にフォームデータが保存されていることを確認できます



