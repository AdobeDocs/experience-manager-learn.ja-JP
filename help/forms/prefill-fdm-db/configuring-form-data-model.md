---
title: フォームデータモデルの設定
description: RDBMSデータソースに基づくフォームデータモデルの作成
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5812
thumbnail: kt-5812.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 2%

---



# フォームデータモデルの設定

## Apache Sling接続プールされたデータソース

RDBMSでバックアップされたフォームデータモデルを作成する最初の手順は、Apache Sling接続プールされたデータソースを設定することです。 データソースを設定するには、次の手順に従います。

* Point your browser to [configMgr](http://localhost:4502/system/console/configMgr)
* Search for **Apache Sling Connection Pooled DataSource**
* 新し追加いエントリが作成され、スクリーンショットに示すように値が提供されます。
* ![データソース](assets/data-source.png)
* 変更を保存する

>[!NOTE]
>JDBC接続のURI、ユーザー名、パスワードは、MySQLデータベースの設定に応じて変わります。


## フォームデータモデルの作成

* ブラウザーで [データ統合を参照](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)
* Click _Create_->_Form Data Model_
* 従業員などのフォームデータモデルに意味のある名前とタイトルを付け **ます。**
* 「_次へ_」をクリックします。
* 前のセクション（フォーラム）で作成したデータソースを選択します
* 「 _作成_」 —>「編集」をクリックして、新しく作成したフォームデータモデルを編集モードで開きます
* 「 _フォーラム_ 」ノードを展開して、従業員のスキーマを確認します。 employeeノードを展開して2つの表を表示します

## モデル追加のエンティティ

* employeeノードが展開されていることを確認します。
* 新規エンティティと受益者エンティティを選択し、「 _追加選択済」をクリックします_

## エンティティを追加すぐに読み取る

* 全エンティティの選択
* Click on _Edit Properties_
* 「サービスを読み取り」ドロップダウンリストから「get」を選択します。
* 「+」アイコンをクリックして、getサービスにパラメーターを追加します。
* スクリーンショットに示すように値を指定する
* ![get-service](assets/get-service.png)
>[!NOTE]
> getサービスでは、エンティティのempID列にマッピングされた値が必要です。この値を渡す方法は複数あり、このチュートリアルでは、empIDがempIDというリクエストパラメータを通じて渡されます。
* 「 _完了_ 」をクリックして、getサービスの引数を保存します
* 「 _完了_ 」をクリックして、変更をフォームデータモデルに保存します

## 2追加エンティティ間の関連付け

データベースエンティティ間で定義された関連付けは、フォームデータモデルでは自動的に作成されません。 エンティティ間の関連付けは、フォームデータモデルエディターを使用して定義する必要があります。 全ての組織が1人以上の受益者を持つことができます私たちは新しい権利者と受益者の間に1対多の関連性を定義する必要があります
次の手順で、1対多の関連付けを作成するプロセスを説明します

* エンティティを選択し、「 _追加関連付け」をクリックします_
* 以下のスクリーンショットに示すように、関連付けと他のプロパティに意味のあるタイトルと識別子を指定します
   ![協会](assets/association-entities-1.png)

* 「引数」セクションの下にある _編集_ アイコンをクリックします。

* このスクリーンショットに示すように値を指定します。
* ![association-2](assets/association-entities.png)
* **受益者エンティティと新規エンティティのempID列を使用して、2つのエンティティをリンクします。**
* Click on _Done_ to save your changes

## フォームデータモデルのテスト

フォームデータモデルには、empIDを受け取り、新聞社とその受益者の詳細を返す **_get_** serviceが追加されました。 getサービスをテストするには、次の手順に従ってください。

* 全エンティティの選択
* 「 _Test Model Object」をクリックします。_
* 有効なempIDを入力し、「 _テスト」をクリックします。_
* 以下のスクリーンショットに示すように、結果が得られます。
* ![テストFDM](assets/test-form-data-model.png)
