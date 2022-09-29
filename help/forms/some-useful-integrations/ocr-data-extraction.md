---
title: OCR データ抽出
description: 政府発行ドキュメントからデータを抽出し、フォームに入力します。
feature: Barcoded Forms
version: 6.4,6.5
kt: 6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 2%

---

# OCR データ抽出

政府発行の様々なドキュメントからデータを自動的に抽出して、アダプティブフォームに入力します。

このサービスを提供する組織は多数あり、REST API が詳細に文書化されている限り、データ統合機能を使用してAEM Formsと簡単に統合できます。 このチュートリアルでは、 [ID アナライザ](https://www.idanalyzer.com/) を使用して、アップロードされたドキュメントの OCR データ抽出を示す。

ID Analyzer サービスを使用してAEM Formsで OCR データ抽出を実装するには、次の手順に従いました。

## 開発者アカウントを作成

で開発者アカウントを作成します。 [ID アナライザ](https://portal.idanalyzer.com/signin.html). API キーをメモします。 このキーは、ID Analyzer のサービスの REST API を呼び出すために必要です。

## Swagger/OpenAPI ファイルの作成

OpenAPI Specification（旧称 Swagger Specification）は、REST API の API 記述形式です。 OpenAPI ファイルを使用すると、次のような API 全体を記述できます。

* 使用可能なエンドポイント (/users) および各エンドポイントでの操作 (GET/users、POST/users)
* 操作パラメータ各操作の入力と出力認証方法
* 連絡先情報、ライセンス、利用条件、その他の情報。
* API の仕様は、YAML または JSON で記述できます。 フォーマットは、学習が容易で、人間と機械の両方に対して読み取り可能です。

最初の Swagger/OpenAPI ファイルを作成するには、 [OpenAPI ドキュメント](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Formsは、OpenAPI Specification version 2.0(fka Swagger) をサポートしています。

以下を使用： [swagger editor](https://editor.swagger.io/) :SMS を使用して送信し、OTP コードを検証する操作を記述する swagger ファイルを作成します。 Swagger ファイルは、JSON 形式または YAML 形式で作成できます。 完成した Swagger ファイルは、 [ここ](assets/drivers-license-swagger.zip)

## Swagger ファイルを定義する際の考慮事項

* 定義が必要です
* $ref はメソッドの定義に使用する必要があります
* セクションを定義して消費し、生成することをお勧めします。
* インラインリクエスト本文パラメーターや応答パラメーターを定義しないでください。 可能な限りモジュール化を試みます。 例えば、次の定義はサポートされていません

```json
 "name": "body",
            "in": "body",
            "required": false,
            "schema": {
              "type": "object",
              "properties": {
                "Rollnum": {
                  "type": "string",
                  "description": "Rollnum"
                }
              }
            }
```

次は、requestBody 定義への参照でサポートされています

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [参照用のサンプル Swagger ファイル](assets/sample-swagger.json)

## データソースを作成

AEM/AEM Formsをサードパーティのアプリケーションと統合するには、 [データソースを作成](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) クラウドサービス設定の「 」。 以下を使用してください： [swagger ファイル](assets/drivers-license-swagger.zip) をクリックしてデータソースを作成します。

## フォームデータモデルの作成

AEM Formsのデータ統合は、を作成して操作するための直感的なユーザーインターフェイスを提供します [フォームデータモデル](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 前の手順で作成したデータソースに基づいてフォームデータモデルを作成します。

![fdm](assets/test-dl-fdm.PNG)

## クライアントライブラリを作成

アップロードしたドキュメントの base64 エンコードされた文字列を取得する必要があります。 この base64 エンコードされた文字列は、REST 呼び出しのパラメーターの 1 つとして渡されます。
クライアントライブラリをダウンロードできます [ここから。](assets/drivers-license-client-lib.zip)

## アダプティブフォームを作成

フォームデータモデルのPOST呼び出しをアダプティブフォームに統合して、フォーム内のユーザーがアップロードしたドキュメントからデータを抽出します。 自分でアダプティブフォームを作成し、フォームデータモデルのPOSTの呼び出しを使用して、アップロードされたドキュメントの base64 エンコードされた文字列を自由に送信できます。

## サーバーにデプロイ

API キーでサンプルアセットを使用する場合は、次の手順に従ってください。

* [データソースのダウンロード](assets/drivers-license-source.zip) を使用してAEMにインポートします。 [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)
* [フォームデータモデルをダウンロードする](assets/drivers-license-fdm.zip) を使用してAEMにインポートします。 [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)
* [クライアントライブラリのダウンロード](assets/drivers-license-client-lib.zip)
* サンプルのアダプティブフォームをダウンロードすると、 [ここからダウンロード](assets/adaptive-form-dl.zip). このサンプルフォームは、この記事の一部として提供されているフォームデータモデルのサービス呼び出しを使用しています。
* フォームをからAEMに読み込む [Formsとドキュメント UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* でフォームを開きます。 [編集モード。](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* API キーをデフォルト値として「 apikey 」フィールドに指定し、変更を保存します。
* 「 Base 64 String 」フィールドのルールエディターを開きます。 このフィールドの値が変更された場合、サービスの呼び出しに注意してください。
* フォームを保存する
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled)，ドライバライセンスのフロントピクチャをアップロードします
