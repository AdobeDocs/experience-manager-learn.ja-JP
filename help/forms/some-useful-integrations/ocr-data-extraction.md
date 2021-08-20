---
title: OCRデータ抽出
description: 政府発行ドキュメントからデータを抽出し、フォームに入力します。
feature: Barcoded Forms
version: 6.4,6.5
kt: 6679
topic: 開発
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 3%

---



# OCRデータ抽出

政府発行の様々なドキュメントから自動的にデータを抽出し、アダプティブフォームに入力します。

このサービスを提供する組織は多数あり、REST APIについて詳しく文書化されている限り、データ統合機能を使用してAEM Formsと簡単に統合できます。 このチュートリアルの目的で、[IDアナライザ](https://www.idanalyzer.com/)を使用して、アップロードしたドキュメントのOCRデータ抽出のデモを行いました。

IDアナライザーサービスを使用してAEM FormsでOCRデータ抽出を実装するには、次の手順に従いました。

## 開発者アカウントの作成

[ID Analyzer](https://portal.idanalyzer.com/signin.html)で開発者アカウントを作成します。 APIキーをメモしておきます。 このキーは、IDアナライザーのサービスのREST APIを呼び出すために必要です。

## Swagger/OpenAPIファイルの作成

OpenAPI仕様（旧称Swagger仕様）は、REST APIのAPI説明形式です。 OpenAPIファイルを使用すると、次のようなAPI全体を記述できます。

* 使用可能なエンドポイント(/users)と各エンドポイントでの操作(GET/users、POST/users)
* 操作パラメータ各操作の入出力
認証方法
* 連絡先情報、ライセンス、利用条件、その他の情報。
* APIの仕様はYAMLまたはJSONで記述できます。 フォーマットは、学習が容易で、人間と機械の両方にとって読み取りが容易です。

最初のSwagger/OpenAPIファイルを作成するには、[OpenAPIのドキュメント](https://swagger.io/docs/specification/2-0/basic-structure/)に従ってください。

>[!NOTE]
> AEM Formsは、OpenAPI仕様バージョン2.0(fka Swagger)をサポートしています。

[swaggerエディター](https://editor.swagger.io/)を使用して、SMSを使用して送信し、OTPコードを検証する操作を記述するswaggerファイルを作成します。 Swaggerファイルは、JSON形式またはYAML形式で作成できます。 完成したSwaggerファイルは、[こちら](assets/drivers-license-swagger.zip)からダウンロードできます。

## データソースの作成

AEM/AEM Formsをサードパーティのアプリケーションと統合するには、クラウドサービス設定で[データソース](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)を作成する必要があります。 [swaggerファイル](assets/drivers-license-swagger.zip)を使用して、データソースを作成してください。

## フォームデータモデルの作成

AEM Formsのデータ統合機能は、[フォームデータモデル](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)を作成して操作するための直感的なユーザーインターフェイスを提供します。 前の手順で作成したデータソースを基にフォームデータモデルを作成します。

![fdm](assets/test-dl-fdm.PNG)

## クライアントライブラリの作成

アップロードしたドキュメントのbase64エンコードされた文字列を取得する必要があります。 次に、このbase64エンコードされた文字列が、REST呼び出しのパラメーターの1つとして渡されます。
クライアントライブラリは、[こちらからダウンロードできます。](assets/drivers-license-client-lib.zip)

## アダプティブフォームの作成

フォームデータモデルのPOSTの呼び出しをアダプティブフォームに統合して、フォーム内のユーザーがアップロードしたドキュメントからデータを抽出する。 独自のアダプティブフォームを自由に作成し、フォームデータモデルのPOST呼び出しを使用して、アップロードされたドキュメントのbase64エンコードされた文字列を送信できます。

## サーバーにデプロイする

サンプルアセットをAPIキーと共に使用する場合は、次の手順に従います。

* [データソースをダウンロ](assets/drivers-license-source.zip) ードし、パッケージマネージャーを使用してAEMに [読み込みます](http://localhost:4502/crx/packmgr/index.jsp)
* [フォームデータモデルをダウンロ](assets/drivers-license-fdm.zip) ードし、パッケージマネージャーを使用してAEMに [読み込みます。](http://localhost:4502/crx/packmgr/index.jsp)
* [クライアントライブラリのダウンロード](assets/drivers-license-client-lib.zip)
* サンプルのアダプティブフォームは、[こちら](assets/adaptive-form-dl.zip)からダウンロードできます。 このサンプルフォームは、この記事の一部として提供されるフォームデータモデルのサービス呼び出しを使用しています。
* [FormsとDocument UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)からAEMにフォームを読み込みます。
* [編集モードでフォームを開きます。](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* APIキーをapikeyフィールドのデフォルト値として指定し、変更を保存します。
* 「 Base 64 String 」フィールドのルールエディターを開きます。 このフィールドの値が変更された場合にサービスが呼び出されることに注意してください。
* フォームの保存
* [フォームをプレビューし](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled)、ドライバーライセンスのフロントピクチャをアップロードします。


