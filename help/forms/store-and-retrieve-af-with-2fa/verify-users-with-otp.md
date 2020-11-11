---
title: OTPでのユーザーの確認
description: OTPを使用して、申込番号に関連付けられている携帯電話番号を確認します。
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 0%

---



# OTPでのユーザーの確認

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

最初のSwagger/OpenAPIファイルを作成するには、OpenAPIのドキュメントに従ってくだ [さい](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Formsは、OpenAPI Specificationバージョン2.0(fka Swagger)をサポートしています。

Swagger [エディタを使用して](https://editor.swagger.io/) 、SMSを使用して送信し、OTPコードを検証する操作を記述するSwaggerファイルを作成します。 Swaggerファイルは、JSON形式またはYAML形式で作成できます。 完成したSwaggerファイルは、 [ここからダウンロードできます](assets/two-factore-authentication-swagger.zip)

## データソースの作成

AEM/AEM Formsをサードパーティのアプリケーションと統合するには、クラウドサービス設定のSwaggerファイルを使用して [RESTベースのデータソースを使用する必要があります](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 。 完成したデータソースは、このコースアセットの一部として提供されます。

## フォームデータモデルを作成

AEM Forms data integration provides an intuitive user interface to create and work with [form data models](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). フォームデータモデルは、データの交換にデータソースを利用します。
完成したフォームデータモデルは、こちらから [ダウンロードできます。](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)
