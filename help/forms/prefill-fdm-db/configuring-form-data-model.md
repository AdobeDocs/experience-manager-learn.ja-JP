---
title: フォームデータモデルの設定
description: RDBMS データソースに基づくフォームデータモデルの作成
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5812
thumbnail: kt-5812.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 5fa4638f-9faa-40e0-a20d-fdde3dbb528a
duration: 103
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '491'
ht-degree: 100%

---

# フォームデータモデルの設定

## Apache Sling Connection Pooled DataSource

RDBMS ベースのフォームデータモデルを作成する最初の手順は、Apache Sling Connection Pooled DataSource を設定することです。データソースを設定するには、以下の手順に従ってください。

* ブラウザーで [configMgr](http://localhost:4502/system/console/configMgr) を参照します。
* **Apache Sling Connection Pooled DataSource** を検索します。
* 新しいエントリを追加し、スクリーンショットに示されているように値を指定します。
* ![data-source](assets/data-source.png)
* 変更を保存します。

>[!NOTE]
>JDBC 接続 URI、ユーザー名およびパスワードは、MySQL データベース設定に応じて変わります。


## フォームデータモデルの作成

* ブラウザーで[データ統合](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)を参照します。
* _作成_／_フォームデータモデル_&#x200B;をクリックします。
* **従業員**&#x200B;など、データモデルを作成するための意味のある名前とタイトルを指定します。
* 「_次へ_」をクリックします。
* 前の節（フォーラム）で作成したデータソースを選択します。
* _作成_／編集をクリックして、新しく作成したフォームデータモデルを編集モードで開きます。
* _フォーラム_&#x200B;ノードを展開すると、従業員スキーマが表示されます。従業員ノードを展開すると、2 つのテーブルが表示されます。

## モデルへのエンティティの追加

* 従業員ノードが展開されていることを確認します。
* 新規採用者エンティティと受取人エンティティを選択し、「_選択項目を追加_」をクリックします。

## 新規採用者エンティティへの読み取りサービスの追加

* 新規採用者エンティティを選択します。
* 「_プロパティを編集_」をクリックします。
* 「読み取りサービス」ドロップダウンリストから「get」を選択します。
* 「+」アイコンをクリックして、get サービスにパラメーターを追加します。
* スクリーンショットに示されているように値を指定します。
* ![get-service](assets/get-service.png)
>[!NOTE]
> get サービスでは、新規採用者エンティティの empID 列にマッピングされた値を想定しています。この値を渡す方法は複数ありますが、このチュートリアルでは、empID は empID というリクエストパラメーターで渡されます。
* 「_完了_」をクリックして、get サービスの引数を保存します。
* 「_完了_」をクリックして、フォームデータモデルの変更内容を保存します。

## 2 つのエンティティ間の関連付けの追加

データベースエンティティ間に定義された関連付けは、フォームデータモデルでは自動的には作成されません。エンティティ間の関連付けは、フォームデータモデルエディターを使用して定義する必要があります。すべての新規採用者エンティティは 1 人以上の受取人を持つことができるので、新規採用者エンティティと受取人エンティティの間に 1 対多の関連付けを定義する必要があります。
次の手順では、1 対多の関連付けを作成する手順を説明します。

* 新規採用者エンティティを選択し、「_関連付けを追加_」をクリックします。
* 次のスクリーンショットに示すように、関連付けやその他のプロパティに意味のあるタイトルと識別子を指定します。
  ![association](assets/association-entities-1.png)

* 「引数」セクションで&#x200B;_編集_&#x200B;アイコンをクリックします。

* このスクリーンショットに示されているように値を指定します。
* ![association-2](assets/association-entities.png)
* **受取人エンティティと新規採用者エンティティの empID 列を使用して 2 つのエンティティをリンクしています。**
* 「_完了_」をクリックして、変更を保存します。

## フォームデータモデルのテスト

フォームデータモデルには、empID を承認し、新規採用者とその受取人の詳細を返す **_get_** サービスを追加しました。get サービスをテストするには、次の手順に従います。

* 新規採用者エンティティを選択します。
* 「_モデルオブジェクトをテスト_」をクリックします。
* 有効な empID を入力し、「_テスト_」をクリックします。
* 次のスクリーンショットに示されているような結果が得られます。
* ![test-fdm](assets/test-form-data-model.png)

## 次の手順

[URL からの empID の取得](./get-request-parameter.md)