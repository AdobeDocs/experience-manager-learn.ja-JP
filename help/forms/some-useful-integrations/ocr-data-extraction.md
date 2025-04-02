---
title: OCR データ抽出
description: 政府発行ドキュメントからデータを抽出し、フォームに入力します。
feature: Barcoded Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
last-substantial-update: 2019-07-07T00:00:00Z
duration: 145
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '661'
ht-degree: 100%

---

# OCR データ抽出

政府発行の様々なドキュメントからデータを自動的に抽出して、アダプティブフォームに入力します。

このサービスを提供する組織は多数あります。REST API が十分に文書化されている限り、データ統合機能を使用して AEM Forms と簡単に統合できます。このチュートリアルでは、[ID アナライザー](https://www.idanalyzer.com/)を使用して、アップロードされたドキュメントの OCR データ抽出を実演しました。

ID アナライザーサービスを使用して AEM Forms で OCR データ抽出を実装するには、次の手順に従いました。

## 開発者アカウントの作成

[ID アナライザー](https://portal.idanalyzer.com/signin.html)で開発者アカウントを作成します。 API キーをメモします。 このキーは、ID アナライザーのサービスの REST API を呼び出すために必要です。

## Swagger／OpenAPI ファイルの作成

OpenAPI Specification（旧称 Swagger Specification）は、REST API の API 記述形式です。 OpenAPI ファイルを使用すると、次のような API 全体を記述できます。

* 使用可能なエンドポイント（/users）および各エンドポイントでの操作（GET/users、POST/users）
* 操作パラメーター各操作の入出力
認証方法
* 連絡先情報、ライセンス、利用条件、その他の情報。
* API の仕様は、YAML または JSON で記述できます。 形式は、学習が容易で、人と機械の両方が読み取り可能です。

最初の swagger／OpenAPI ファイルを作成するには、[OpenAPI ドキュメント](https://swagger.io/docs/specification/2-0/basic-structure/) に従ってください。

>[!NOTE]
> AEM Forms は、OpenAPI Specification version 2.0（旧 Swagger）をサポートしています。

[swagger エディター](https://editor.swagger.io/) を使用して、SMS を使用して送信された OTP コードを送信および検証する操作を記述した swagger ファイルを作成します。Swagger ファイルは、JSON 形式または YAML 形式で作成できます。 完成した Swagger ファイルは、[こちら](assets/drivers-license-swagger.zip)からダウンロードできます

## Swagger ファイルを定義する際の考慮事項

* 定義は必須です
* メソッドの定義には $ref を使用する必要があります
* 消費セクションと生成セクションの定義があるほうが望ましいです
* インラインリクエスト本文パラメーターや応答パラメーターを定義せず、可能な限りモジュール化を試みます。例えば、次の定義はサポートされていません。

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

次の定義は、requestBody 定義への参照でサポートされています。

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [参照用のサンプル Swagger ファイル](assets/sample-swagger.json)

## データソースの作成

AEM／AEM Forms をサードパーティのアプリケーションと統合するには、クラウドサービス設定で[データソースを作成](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=ja)する必要があります。データソースを作成するには、[swagger ファイル](assets/drivers-license-swagger.zip)を使用してください。

## フォームデータモデルの作成

AEM Forms データ統合は、[フォームデータモデル](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=ja)を作成して操作するための直感的なユーザーインターフェイスを提供します。前の手順で作成したデータソースに基づいてフォームデータモデルを作成します。

![fdm](assets/test-dl-fdm.PNG)

## クライアントライブラリの作成

アップロードしたドキュメントの base64 エンコードされた文字列を取得する必要があります。 この base64 エンコードされた文字列は、REST 呼び出しのパラメーターの 1 つとして渡されます。
[こちら](assets/drivers-license-client-lib.zip)からクライアントライブラリをダウンロードできます。

## アダプティブフォームの作成

フォームデータモデルの POST 呼び出しをアダプティブフォームに統合して、フォーム内のユーザーがアップロードしたドキュメントからデータを抽出します。 独自のアダプティブ フォームを自由に作成し、フォーム データ モデルの POST 呼び出しを使用して、アップロードされたドキュメントの base64 でエンコードされた文字列を送信できます。

## サーバーへのデプロイ

API キーでサンプルアセットを使用する場合は、次の手順に従ってください。

* [データソースをダウンロード](assets/drivers-license-source.zip)し、[パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して AEM に読み込みます。
* [フォームデータモデルをダウンロードし](assets/drivers-license-fdm.zip)、[パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して AEM に読み込みます。
* [クライアントライブラリのダウンロード](assets/drivers-license-client-lib.zip)
* サンプルのアダプティブフォームは[こちら](assets/adaptive-form-dl.zip)からダウンロードできます。このサンプルフォームは、この記事の一部として提供されているフォームデータモデルのサービス呼び出しを使用しています。
* [フォームとドキュメントの UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) から AEM にフォームを読み込みます
* フォームを[編集モード](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)で開きます。
* API キーをデフォルト値として「 apikey 」フィールドに指定し、変更を保存します。
* 「Base 64 文字列」フィールドのルールエディターを開きます。 このフィールドの値が変更された場合、サービスの呼び出しに注意してください。
* フォームを保存する
* [フォームをプレビュー](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled)して、運転免許証の前面の写真をアップロードします
