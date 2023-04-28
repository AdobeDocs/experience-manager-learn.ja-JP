---
title: サーバーへのサンプルアセットのデプロイ
description: インタラクティブ通信の「ドラフトとして保存」機能のテスト
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
source-git-commit: db99787c48e49a9861de893e6cb7fbb7b31807b8
workflow-type: ht
source-wordcount: '165'
ht-degree: 100%

---

# サーバーへのサンプルアセットのデプロイ

この機能を AEM サーバーで動作させるには、以下の手順に従ってください。

* [データベーススキーマを作成します。](assets/icdrafts.sql)
* [クライアントライブラリを読み込みます。](assets/icdrafts.zip)
* [アダプティブフォームを読み込みます。](assets/SavedDraftsAdaptiveForm.zip)
* _SaveAndContinue_ という名前のデータソースを作成します。

![データソースの作成](assets/data-source.png)

| プロパティ名 | プロパティの値 |
|---|---|
| データソース名 | SaveAndContinue |
| JDBC driver class | com.mysql.cj.jdbc.Driver |
| JDBC connection URI | jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&amp;useSSL=false&amp;characterEncoding=utf8&amp;useUnicode=true |

* [icdrafts バンドルをデプロイします。](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* OSGI 設定で必ず「_Enable Save Using CCRDocumentInstanceService_」オプションをオンにします（下図を参照）。
   ![ドラフトの有効化](assets/enable-drafts.png)
* 任意のインタラクティブ通信を開きます。「ドラフトとして保存」をクリックして保存します。
* [保存済みのドラフトを表示します](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)。

>[!NOTE]
>xml ファイルは、AEM サーバーインストールのルートフォルダーに保存されます。要件に応じてソリューションをカスタマイズできるように、Eclipse プロジェクトが提供されています。

サンプル実装を含んだ Eclipse プロジェクトは、[こちらからダウンロード](assets/icdrafts-eclipse-project.zip)できます。
