---
title: Batch APIを使用した対話型通信ドキュメントの生成
description: バッチAPIを使用して印刷チャネルドキュメントを生成するためのサンプルアセット
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
translation-type: tm+mt
source-git-commit: 3dc1bd3f2f7b6324c53640f01a263fa0728d439c
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 6%

---


# バッチAPI

Batch APIを使用すると、テンプレートから複数のインタラクティブな通信を作成できます。 テンプレートは、データを一切使用しないインタラクティブな通信です。 Batch APIは、データとテンプレートを組み合わせてインタラクティブな通信を行います。 このAPIは、インタラクティブ通信の大量生産に役立ちます。 例えば、電話料金、複数の顧客のクレジットカード明細などです。

[バッチ生成APIの詳細](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

この記事では、Batch APIを使用してインタラクティブ通信ドキュメントを生成するためのサンプルアセットを提供します。

## 監視フォルダーを使用したバッチ生成

* Interactive Communicationテンプレートを [AEM Formsサーバに読み込みます](assets/Beneficiaries-confirmation.zip) 。
* 監 [視フォルダーの設定を読み込みます](assets/batch-generation-api.zip)。 これにより、Cドライブにという名前 `batchAPI` のフォルダが作成されます。

**Windows以外のオペレーティングシステムでAEM Formsを実行している場合は、次の3つの手順に従ってください。**

1. [監視フォルダーを開く](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. BatchAPIWatchedFolderを選択し、「編集」をクリックします。
3. Pathを、使用しているオペレーティングシステムに合わせて変更します。

![path](assets/watched-folder-batch-api-basic.PNG)

* zipファイルの内容をダウンロードして抽出し [ます](assets/jsonfile.zip)。 zipファイルには、ファイルを含む名前のフォルダ `jsonfile` ーが含まれ `beneficiaries.json` ています。 このファイルには、3つのドキュメントを生成するデータが含まれています。

* 監視フォルダーの入力フォルダーに `jsonfile` フォルダーをドロップします。
* フォルダーが処理対象として取得されたら、監視フォルダーの結果フォルダーを確認します。 3つのPDFファイルが生成されます

## REST要求を使用したバッチ生成

REST要求を通じて [Batch API](https://helpx.adobe.com/jp/experience-manager/6-5/forms/javadocs/index.html) （バッチAPI）を呼び出すことができます。 他のアプリケーション用のRESTエンドポイントを公開して、APIを呼び出し、ドキュメントを生成できます。
提供されるサンプルアセットは、インタラクティブ通信ドキュメントを生成するためのRESTエンドポイントを公開します。 サーブレットは次のパラメーターを受け付けます。

* fileName — ファイルシステム上のデータファイルの場所。
* templatePath - ICテンプレートパス
* saveLocation — 生成されたドキュメントをファイルシステムに保存する場所
* channelType — 印刷、Webまたはその両方
* recordId — インタラクティブ通信の名前を設定する要素のJSONパス

次のスクリーンショットは、パラメーターとその値の![サンプルリクエストを示しています](assets/generate-ic-batch-servlet.PNG)

## サーバーへのサンプルアセットのデプロイ

* [パッケージマネージャーを使用してICTemplateを読み込み](assets/ICTemplate.zip)[ます](http://localhost:4502/crx/packmgr/index.jsp)
* [パッケージマネージャーを使用したカスタム送信ハンドラーの読み込み](assets/BatchAPICustomSubmit.zip)[](http://localhost:4502/crx/packmgr/index.jsp)
* [Formsとドキュメントのインターフェイスを使用した](assets/BatchGenerationAPIAF.zip) アダプティブフォームの読み込み [](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Felix Webコンソールを使用した](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) カスタムOSGIバンドルのデプロイと開始 [](http://localhost:4502/system/console/bundles)
* [フォームを送信してバッチ生成をトリガーする](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
