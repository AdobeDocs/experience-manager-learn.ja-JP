---
title: Batch APIを使用した対話型通信ドキュメントの生成
description: バッチAPIを使用して印刷チャネルドキュメントを生成するためのサンプルアセット
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 7%

---


# バッチAPI

Batch APIを使用すると、テンプレートから複数のインタラクティブな通信を作成できます。 テンプレートは、データを一切使用しないインタラクティブな通信です。 Batch APIは、データとテンプレートを組み合わせてインタラクティブな通信を行います。 このAPIは、インタラクティブ通信の大量生産に役立ちます。 例えば、電話料金、複数の顧客のクレジットカード明細などです。

[バッチ生成APIの詳細](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

この記事では、Batch APIを使用してインタラクティブ通信ドキュメントを生成するためのサンプルアセットを提供します。

## 監視フォルダーを使用したバッチ生成

* [対話型通信テンプレート](assets/Beneficiaries-confirmation.zip)をAEM Formsサーバーに読み込みます。
* [監視フォルダー設定](assets/batch-generation-api.zip)を読み込みます。 これにより、Cドライブに`batchAPI`という名前のフォルダが作成されます。

**Windows以外のオペレーティングシステムでAEM Formsを実行している場合は、次の3つの手順に従ってください。**

1. [監視フォルダーを開く](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. BatchAPIWatchedFolderを選択し、「編集」をクリックします。
3. Pathを、使用しているオペレーティングシステムに合わせて変更します。

![path](assets/watched-folder-batch-api-basic.PNG)

* [zipファイル](assets/jsonfile.zip)の内容をダウンロードして抽出します。 zipファイルには`jsonfile`という名前のフォルダーが含まれ、このフォルダーには`beneficiaries.json`ファイルが含まれています。 このファイルには、3つのドキュメントを生成するデータが含まれています。

* `jsonfile`フォルダーを監視フォルダーの入力フォルダーにドロップします。
* フォルダーが処理対象として取得されたら、監視フォルダーの結果フォルダーを確認します。 3つのPDFファイルが生成されます

## REST要求を使用したバッチ生成

REST要求を使用して[Batch API](https://helpx.adobe.com/jp/experience-manager/6-5/forms/javadocs/index.html)を呼び出すことができます。 他のアプリケーション用のRESTエンドポイントを公開して、APIを呼び出し、ドキュメントを生成できます。
提供されるサンプルアセットは、インタラクティブ通信ドキュメントを生成するためのRESTエンドポイントを公開します。 サーブレットは次のパラメーターを受け付けます。

* fileName — ファイルシステム上のデータファイルの場所。
* templatePath - ICテンプレートパス
* saveLocation — 生成されたドキュメントをファイルシステムに保存する場所
* channelType — 印刷、Webまたはその両方
* recordId — インタラクティブ通信の名前を設定する要素のJSONパス

次のスクリーンショットは、パラメーターとその値を示しています
![サンプルリクエスト](assets/generate-ic-batch-servlet.PNG)

## サーバーへのサンプルアセットのデプロイ

* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して[ICTemplate](assets/ICTemplate.zip)を読み込み
* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して[カスタム送信ハンドラー](assets/BatchAPICustomSubmit.zip)を読み込む
* [[Formsおよびドキュメントインターフェイス](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)を使用してアダプティブフォーム](assets/BatchGenerationAPIAF.zip)を読み込む
* [Felix Webコンソール](http://localhost:4502/system/console/bundles)を使用した[カスタムOSGIバンドル](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar)のデプロイと開始
* [フォームを送信したトリガーバッチ生成](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
