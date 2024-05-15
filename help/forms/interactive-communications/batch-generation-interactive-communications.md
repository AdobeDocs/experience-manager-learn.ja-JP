---
title: Batch API を使用したインタラクティブ通信ドキュメントの生成
description: Batch API を使用した印刷チャネルドキュメントの生成用のサンプルアセット
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
duration: 77
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 100%

---

# Batch API

Batch API を使用すると、テンプレートから複数のインタラクティブ通信を作成できます。テンプレートは、データのないインタラクティブ通信です。Batch API は、データとテンプレートを組み合わせて、インタラクティブな通信を作成します。この API は、インタラクティブ通信を大量に作成する際に役立ちます。例えば、複数の顧客に電話料金やクレジットカードの明細書を送信する場合です。

[バッチ生成 API について詳しくは、こちらを参照してください](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html?lang=ja)

この記事では、Batch API を使用してインタラクティブなコミュニケーションドキュメントを生成するためのサンプルアセットを提供します。

## 監視フォルダーを使用したバッチ生成

* [インタラクティブなコミュニケーションテンプレート](assets/Beneficiaries-confirmation.zip)を AEM Forms サーバーに読み込みます。
* [監視フォルダーの設定](assets/batch-generation-api.zip)を読み込みます。これにより、C ドライブに `batchAPI` というフォルダーが作成されます。

**Windows 以外のオペレーティングシステムで AEM Forms を実行している場合は、次の 3 つの手順に従ってください。**

1. [監視フォルダーを開きます](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. BatchAPIWatchedFolder を選択し、「編集」をクリックします。
3. オペレーティングシステムに合わせてパスを変更します。

![パス](assets/watched-folder-batch-api-basic.PNG)

* [zip ファイル](assets/jsonfile.zip)をダウンロードしてコンテンツを解凍します。zip ファイルには、`beneficiaries.json` ファイルを含む `jsonfile` という名前のフォルダーが含まれています。このファイルには、3 つのドキュメントを生成するためのデータが含まれています。

* `jsonfile` フォルダーを監視フォルダーの入力フォルダーにドロップします。
* フォルダーを処理用に取得したら、監視フォルダーの結果フォルダーを確認します。生成された 3 つの PDF ファイルが表示されます

## REST リクエストを使用したバッチ生成

[Batch API](https://helpx.adobe.com/jp/experience-manager/6-5/forms/javadocs/index.html) は、REST リクエストを通じて 呼び出すことができます。他のアプリケーションの REST エンドポイントを公開して、API を呼び出してドキュメントを生成できます。
提供されるサンプルアセットは、インタラクティブ通信ドキュメントを生成するための REST エンドポイントを公開します。サーブレットは、次のパラメーターを受け入れます。

* fileName - ファイルシステム上のデータファイルの場所。
* templatePath - IC テンプレートのパス
* saveLocation - 生成したドキュメントをファイルシステム上に保存する場所
* channelType - 印刷、web またはその両方
* recordId - インタラクティブ通信の名前を設定する要素の JSON パス

次のスクリーンショットは、パラメーターとその値を示しています
![サンプルリクエスト](assets/generate-ic-batch-servlet.PNG)

## サーバーへのサンプルアセットのデプロイ

* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用した [ICTemplate](assets/ICTemplate.zip) の読み込み
* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用した[カスタム送信ハンドラー](assets/BatchAPICustomSubmit.zip)の読み込み
* [Forms とドキュメントインターフェイス](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)を使用した[アダプティブフォーム](assets/BatchGenerationAPIAF.zip)の読み込み
* [Felix web コンソール](http://localhost:4502/system/console/bundles)を使用した[カスタム OSGI バンドル](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar)のデプロイと開始
* [フォームを送信したバッチ生成のトリガー](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
