---
title: OCRデータ抽出
description: 政府発行のドキュメントからデータを抽出し、フォームに入力します。
feature: バーコードForms
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6679
topic: 開発
role: デベロッパー
level: 中間
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 3%

---



# OCRデータ抽出

様々な政府発行ドキュメントから自動的にデータを抽出し、アダプティブフォームに入力します。

このサービスを提供する組織は多数あり、REST APIについて十分に文書化されている限り、データ統合機能を使用してAEM Formsと簡単に統合できます。 このチュートリアルの目的で、[IDアナライザ](https://www.idanalyzer.com/)を使って、アップロードされたドキュメントのOCRデータ抽出を示しました。

ID Analyzerサービスを使用して、AEM FormsでOCRデータ抽出を実装するには、次の手順に従いました。

## 開発者アカウントの作成

[IDアナライザー](https://portal.idanalyzer.com/signin.html)を使用して開発者アカウントを作成します。 APIキーをメモしておきます。 このキーは、IDアナライザーのサービスのREST APIを呼び出すために必要です。

## Swagger/OpenAPIファイルの作成

OpenAPI仕様（旧称Swagger仕様）は、REST APIのAPI説明形式です。 OpenAPIファイルを使用すると、次のようなAPI全体を記述できます。

* 使用可能なエンドポイント(/users)と各エンドポイントでの操作(GET/users、POST/users)
* 操作パラメーター各操作の入力および出力
認証方法
* 連絡先情報、ライセンス、利用条件、その他の情報。
* API仕様はYAMLまたはJSONで記述できます。 この形式は、人間と機械の両方が簡単に学習でき、読み取ることができます。

最初のSwagger/OpenAPIファイルを作成するには、[OpenAPIドキュメント](https://swagger.io/docs/specification/2-0/basic-structure/)に従ってください。

>[!NOTE]
> AEM Formsは、OpenAPI Specificationバージョン2.0(fka Swagger)をサポートしています。

[swagger editor](https://editor.swagger.io/)を使用して、SMSを使用して送信し、OTPコードを検証する操作を記述するswaggerファイルを作成します。 Swaggerファイルは、JSON形式またはYAML形式で作成できます。 完成したSwaggerファイルは、[ここ](assets/drivers-license-swagger.zip)からダウンロードできます。

## データソースの作成

AEM/AEM Formsをサードパーティのアプリケーションと統合するには、クラウドサービス設定で[データソース](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)を作成する必要があります。 [swaggerファイル](assets/drivers-license-swagger.zip)を使用して、データソースを作成してください。

## フォームデータモデルを作成

AEM Formsデータ統合は、[フォームデータモデル](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html)を作成し、操作するための直観的なユーザーインターフェイスを提供します。 前の手順で作成したデータソースを基にフォームデータモデルを作成します。

![fdm](assets/test-dl-fdm.PNG)

## クライアントライブラリの作成

アップロードされたドキュメントのbase64エンコードされた文字列を取得する必要があります。 このbase64エンコードされた文字列は、次に、REST呼び出しのパラメーターの1つとして渡されます。
クライアントライブラリは、[ここからダウンロードできます。](assets/drivers-license-client-lib.zip)

## アダプティブフォームの作成

Form Data ModelのPOST呼び出しをアダプティブフォームに統合し、フォーム内のユーザーがアップロードしたドキュメントからデータを抽出します。 独自のアダプティブフォームを自由に作成し、フォームデータモデルのPOST呼び出しを使用して、アップロードされたドキュメントのbase64エンコードされた文字列を送信できます。

## サーバーにデプロイする

APIキーでサンプルアセットを使用する場合は、次の手順に従ってください。

* [データ](assets/drivers-license-source.zip) ソースをダウンロードし、 [パッケージマネージャーを使用してAEMにインポートします](http://localhost:4502/crx/packmgr/index.jsp)
* [フォームデータ](assets/drivers-license-fdm.zip) モデルをダウンロードし、 [パッケージマネージャーを使用してAEMに読み込みます](http://localhost:4502/crx/packmgr/index.jsp)
* [クライアントライブラリのダウンロード](assets/drivers-license-client-lib.zip)
* サンプルのアダプティブフォームは、ここから[ダウンロードできます](assets/adaptive-form-dl.zip)。 このサンプルフォームは、この記事の一部として提供されるフォームデータモデルのサービス呼び出しを使用しています。
* [FormsおよびドキュメントUI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)からAEMにフォームを読み込みます
* フォームを[編集モードで開きます。](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* APIキーをapikeyフィールドのデフォルト値として指定し、変更を保存します
* 「Base 64文字列」フィールドのルールエディターを開きます。 このフィールドの値が変更された場合のサービス呼び出しに注目してください。
* フォームを保存する
* [フォームをプレビューし](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled)、運転免許証の前面画像をアップロードする


