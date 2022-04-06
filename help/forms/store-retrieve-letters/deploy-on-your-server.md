---
title: サーバーにサンプルアセットをデプロイします。
description: インタラクティブ通信の下書きとして保存機能のテスト
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
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# サーバーにサンプルアセットをデプロイします。

この機能をAEM Server で動作させるには、以下の手順に従ってください

* c ドライブに icdrafts というフォルダーを作成します。
* [データベーススキーマの作成](assets/icdrafts.sql)
* [クライアントライブラリのインポート](assets/icdrafts.zip)
* [アダプティブフォームの読み込み](assets/SavedDraftsAdaptiveForm.zip)
* 次の名前のデータソースを作成 _SaveAndContinue_

![データソースを作成](assets/data-source.png)

* [icdrafts バンドルをデプロイします。](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* 必ず _CCRDocumentInstanceService を使用して保存を有効にする_ （以下に示すように）
   ![下書きの有効化](assets/enable-drafts.png)
* 任意のインタラクティブ通信を開きます。 ドラフトとして保存をクリックして保存
* [保存済みの下書きの表示](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>xml ファイルは、AEMサーバーインストール環境のルートフォルダーに保存されます。 Eclipse プロジェクトが提供され、必要に応じてソリューションをカスタマイズできます。

サンプル実装を使用した Eclipse プロジェクトは、次のようになります。 [ここからダウンロード](assets/icdrafts-eclipse-project.zip)
