---
title: サーバーへのサンプルアセットのデプロイ
description: インタラクティブ通信の「ドラフトとして保存」機能のテスト
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
duration: 28
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '143'
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
| データソース名 | `SaveAndContinue` |
| JDBC ドライバークラス | `com.mysql.cj.jdbc.Driver` |
| JDBC 接続 URL | `jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&useSSL=false&characterEncoding=utf8&useUnicode=true` |

* [icdrafts バンドルをデプロイします。](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* OSGI 設定で必ず「_Enable Save Using CCRDocumentInstanceService_」オプションをオンにします（下図を参照）。
  ![ドラフトの有効化](assets/enable-drafts.png)
* 任意のインタラクティブ通信を開きます。「ドラフトとして保存」をクリックして保存します。
* [保存済みのドラフトを表示します](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)。

>[!NOTE]
>xml ファイルは、AEM サーバーインストールのルートフォルダーに保存されます。要件に応じてソリューションをカスタマイズできるように、Eclipse プロジェクトが提供されています。

サンプル実装を含んだ Eclipse プロジェクトは、[こちらからダウンロード](assets/icdrafts-eclipse-project.zip)できます。
