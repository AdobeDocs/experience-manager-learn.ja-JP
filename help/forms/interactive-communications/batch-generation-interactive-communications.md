---
title: Batch API を使用したインタラクティブ通信ドキュメントの生成
description: バッチ API を使用して印刷チャネルドキュメントを生成するためのサンプルアセット
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 20%

---

# バッチ API

Batch API を使用すると、テンプレートから複数のインタラクティブ通信を作成できます。テンプレートは、データのないインタラクティブ通信です。Batch API は、データとテンプレートを組み合わせて、インタラクティブな通信を作成します。この API は、インタラクティブ通信を大量に作成する際に役立ちます。例えば、複数の顧客に電話料金やクレジットカードの明細書を送信する場合です。

[バッチ生成 API の詳細を説明します](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

この記事では、Batch API を使用してインタラクティブ通信ドキュメントを生成するためのサンプルアセットを提供します。

## 監視フォルダーを使用したバッチ生成

* 次をインポート： [インタラクティブ通信テンプレート](assets/Beneficiaries-confirmation.zip) をAEM Formsサーバーにアップロードします。
* 次をインポート： [監視フォルダーの設定](assets/batch-generation-api.zip). これにより、 `batchAPI` C ドライブ内に保存されます。

**Windows 以外のオペレーティングシステムでAEM Formsを実行している場合は、次の 3 つの手順に従ってください。**

1. [監視フォルダーを開く](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. BatchAPIWatchedFolder を選択し、「編集」をクリックします。
3. 「パス」を、使用しているオペレーティングシステムに合わせて変更します。

![path](assets/watched-folder-batch-api-basic.PNG)

* の内容をダウンロードして抽出する [zip ファイル](assets/jsonfile.zip). zip ファイルにはという名前のフォルダーが含まれます。 `jsonfile` 次を含む `beneficiaries.json` ファイル。 このファイルには、3 つのドキュメントを生成するデータが含まれています。

* 次をドロップ： `jsonfile` フォルダーを監視フォルダーの入力フォルダーに移動します。
* フォルダーを処理用に取得したら、監視フォルダーの結果フォルダーを確認します。 3 つのPDFファイルが生成されます

## REST リクエストを使用したバッチ生成

を呼び出すことができます。 [バッチ API](https://helpx.adobe.com/jp/experience-manager/6-5/forms/javadocs/index.html) を介して送信されます。 他のアプリケーションの REST エンドポイントを公開して、API を呼び出してドキュメントを生成できます。
提供されるサンプルアセットは、インタラクティブ通信ドキュメントを生成するための REST エンドポイントを公開します。 このサーブレットは、次のパラメーターを受け入れます。

* fileName — ファイルシステム上のデータファイルの場所。
* templatePath - IC テンプレートのパス
* saveLocation — 生成したドキュメントをファイルシステム上に保存する場所
* channelType — 印刷、Web またはその両方
* recordId — インタラクティブ通信の名前を設定する要素の JSON パス

次のスクリーンショットは、パラメーターとその値を示しています
![サンプルリクエスト](assets/generate-ic-batch-servlet.PNG)

## サーバーへのサンプルアセットのデプロイ

* インポート [ICTemplate](assets/ICTemplate.zip) using [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)
* インポート [カスタム送信ハンドラー](assets/BatchAPICustomSubmit.zip) using [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)
* インポート [アダプティブフォーム](assets/BatchGenerationAPIAF.zip) の使用 [Formsとドキュメントのインターフェイス](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* デプロイと開始 [カスタム OSGI バンドル](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) using [Felix Web コンソール](http://localhost:4502/system/console/bundles)
* [トリガーのバッチ生成（フォームの送信による）](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
