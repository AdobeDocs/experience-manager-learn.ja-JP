---
title: Batch APIを使用したインタラクティブ通信ドキュメントの生成
description: バッチAPIを使用して印刷チャネルドキュメントを生成するためのアセットの例
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: 開発
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 7%

---


# バッチAPI

Batch APIを使用すると、テンプレートから複数のインタラクティブ通信を作成できます。 テンプレートは、データのないインタラクティブ通信です。 Batch APIは、データとテンプレートを組み合わせて、インタラクティブ通信を作成します。 このAPIは、インタラクティブ通信を大量に生産する際に役立ちます。 例えば、電話料金、複数の顧客のクレジットカード明細などです。

[バッチ生成APIの詳細を説明します](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

この記事では、Batch APIを使用してインタラクティブ通信ドキュメントを生成するためのサンプルアセットを提供します。

## 監視フォルダーを使用したバッチ生成

* [インタラクティブ通信テンプレート](assets/Beneficiaries-confirmation.zip)をAEM Formsサーバーに読み込みます。
* [監視フォルダー設定](assets/batch-generation-api.zip)を読み込みます。 これにより、Cドライブに`batchAPI`というフォルダが作成されます。

**Windows以外のオペレーティングシステムでAEM Formsを実行している場合は、次の3つの手順に従ってください。**

1. [監視フォルダーを開く](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. BatchAPIWatchedFolderを選択し、「編集」をクリックします。
3. 使用しているオペレーティングシステムに合わせてパスを変更します。

![path](assets/watched-folder-batch-api-basic.PNG)

* [zipファイル](assets/jsonfile.zip)の内容をダウンロードして抽出します。 zipファイルには、`jsonfile`という名前のフォルダーが含まれ、このフォルダーに`beneficiaries.json`ファイルが格納されます。 このファイルには、3つのドキュメントを生成するデータが含まれています。

* `jsonfile`フォルダーを監視フォルダーの入力フォルダーにドロップします。
* フォルダーを処理用に取得したら、監視フォルダーの結果フォルダーを確認します。 3つのPDFファイルが生成されているはずです

## RESTリクエストを使用したバッチ生成

[Batch API](https://helpx.adobe.com/jp/experience-manager/6-5/forms/javadocs/index.html)は、RESTリクエストを通じて呼び出すことができます。 他のアプリケーションのRESTエンドポイントを公開して、APIを呼び出してドキュメントを生成できます。
提供されたサンプルアセットは、インタラクティブ通信ドキュメントを生成するためのRESTエンドポイントを公開します。 このサーブレットは、次のパラメーターを受け入れます。

* fileName — ファイルシステム上のデータファイルの場所。
* templatePath - ICテンプレートパス
* saveLocation — 生成したドキュメントをファイルシステムに保存する場所
* channelType — 印刷、Webまたはその両方
* recordId — インタラクティブ通信の名前を設定する要素のJSONパス

次のスクリーンショットは、パラメーターとその値を示しています
![サンプルリクエスト](assets/generate-ic-batch-servlet.PNG)

## サーバーへのサンプルアセットのデプロイ

* [[パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用してICTemplate](assets/ICTemplate.zip)を読み込みます
* [[パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用してカスタム送信ハンドラー](assets/BatchAPICustomSubmit.zip)を読み込みます
* [Formsとドキュメントインターフェイス](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)を使用して[アダプティブフォーム](assets/BatchGenerationAPIAF.zip)を読み込む
* [[Felix Webコンソール](http://localhost:4502/system/console/bundles)を使用して、カスタムOSGIバンドル](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar)をデプロイして起動します。
* [トリガーバッチ生成（フォーム送信による）](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
