---
title: SMS 2要素認証
description: 特定追加のアクティビティを実行する場合にユーザーのIDを確認するのに役立つ、セキュリティの追加層
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
translation-type: tm+mt
source-git-commit: 9f8c858197e44de020ab195373f30e3d38dfd2cc
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 2%

---



# 携帯電話番号を使用したユーザーの確認

SMS Two Factor Authentication (Dual Factor Authentication)は、セキュリティ検証手順です。セキュリティ検証手順は、ユーザーがWebサイト、ソフトウェア、またはアプリケーションにログインすることでトリガされます。 ログインプロセスでは、ユーザは自動的にSMSを自動的に自分のモバイル番号に送信し、一意の数値コードを含めます。

このサービスを提供する組織は多数あり、REST APIについて詳しく文書化されている限り、AEM Formsのデータ統合機能を使用してAEM Formsを容易に統合できます。 このチュートリアルの目的で、 [Nexmoを使用してSMS 2FAの使用例をデモしました](https://developer.nexmo.com/verify/overview) 。

Nexmo Verifyサービスを使用して、SMS 2FAをAEM Formsと実装するには、次の手順に従います。

## 開発者アカウントの作成

[Nexmoで開発者アカウントを作成します](https://dashboard.nexmo.com/sign-in)。 APIキーとAPI秘密鍵をメモしておきます。 これらのキーは、NexmoのサービスのREST APIを呼び出すために必要です。

## Swagger/OpenAPIファイルの作成

OpenAPI仕様（旧称Swagger仕様）は、REST APIのAPI説明形式です。 OpenAPIファイルを使用すると、次のようなAPI全体を記述できます。

* 使用可能なエンドポイント(/users)と各エンドポイントでの操作(GET/users、POST/users)
* 各operationAuthenticationメソッドの操作パラメーターInputおよび出力
* 連絡先情報、ライセンス、利用条件、その他の情報。
* API仕様はYAMLまたはJSONで記述できます。 この形式は、人間と機械の両方が簡単に学習でき、読み取ることができます。

最初のSwagger/OpenAPIファイルを作成するには、OpenAPIのドキュメントに従ってく [ださい](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Formsは、OpenAPI Specificationバージョン2.0(fka Swagger)をサポートしています。

Swagger [エディタを使用して](https://editor.swagger.io/) 、SMSを使用して送信し、OTPコードを検証する操作を記述するSwaggerファイルを作成します。 Swaggerファイルは、JSON形式またはYAML形式で作成できます。 完成したSwaggerファイルは、 [ここからダウンロードできます](assets/two-factore-authentication-swagger.zip)

## データソースの作成

AEM/AEM Formsをサードパーティのアプリケーションと統合するには、クラウドサービスの設定でデータソース [を](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 作成する必要があります。

## フォームデータモデルを作成

AEM Forms data integration provides an intuitive user interface to create and work with [form data models](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). フォームデータモデルは、データの交換にデータソースを利用します。
完成したフォームデータモデルは、こちらから [ダウンロードできます。](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## アダプティブフォームの作成

Form Data ModelのPOST呼び出しをアダプティブフォームに統合し、フォームにユーザーが入力した携帯電話番号を確認します。 独自のアダプティブフォームを自由に作成し、フォームデータモデルのPOST呼び出しを使用して、必要に応じてOTPコードを送信および検証できます。

APIキーでサンプルアセットを使用する場合は、次の手順に従います。

* [フォームデータモデルをダウンロードし](assets/sms-2fa-fdm.zip) 、 [Package Managerを使用してAEMに読み込みます](http://localhost:4502/crx/packmgr/index.jsp)
* サンプルのアダプティブフォームは、ここから [ダウンロードできます](assets/sms-2fa-verification-af.zip)。 このサンプルフォームは、この記事の一部として提供されるフォームデータモデルのサービス呼び出しを使用しています。
* [FormsとドキュメントのUIからAEMにフォームを読み込む](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* フォームを編集モードで開きます。 次のフィールドのルールエディターを開きます
   ![sms-send](assets/check-sms.PNG)
* フィールドに関連付けられているルールを編集します。 適切なAPIキーの指定
* フォームを保存する
* [フォームのプレビュー](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) 、および機能のテスト


