---
title: SMS 2要素認証
description: 特定追加のアクティビティを実行する場合にユーザーのIDを確認するのに役立つ、セキュリティの追加層
feature: 統合
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '604'
ht-degree: 2%

---



# 携帯電話番号を使用したユーザーの確認

SMS Two Factor Authentication (Dual Factor Authentication)は、セキュリティ検証手順です。セキュリティ検証手順は、ユーザーがWebサイト、ソフトウェア、またはアプリケーションにログインすることでトリガされます。 ログインプロセスでは、ユーザは自動的にSMSを自動的に自分のモバイル番号に送信し、一意の数値コードを含めます。

このサービスを提供する組織は多数あり、REST APIについて詳しく文書化されている限り、AEM Formsのデータ統合機能を使用してAEM Formsを容易に統合できます。 このチュートリアルの目的で、[Nexmo](https://developer.nexmo.com/verify/overview)を使用してSMS 2FAの使用例を示しました。

Nexmo Verifyサービスを使用して、SMS 2FAをAEM Formsと実装するには、次の手順に従います。

## 開発者アカウントの作成

[Nexmo](https://dashboard.nexmo.com/sign-in)で開発者アカウントを作成します。 APIキーとAPI秘密鍵をメモしておきます。 これらのキーは、NexmoのサービスのREST APIを呼び出すために必要です。

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

[swagger editor](https://editor.swagger.io/)を使用して、SMSを使用して送信し、OTPコードを検証する操作を記述するswaggerファイルを作成します。 Swaggerファイルは、JSON形式またはYAML形式で作成できます。 完成したSwaggerファイルは、[ここ](assets/two-factore-authentication-swagger.zip)からダウンロードできます。

## データソースの作成

AEM/AEM Formsをサードパーティのアプリケーションと統合するには、クラウドサービス設定で[データソース](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)を作成する必要があります。

## フォームデータモデルを作成

AEM Formsデータ統合は、[フォームデータモデル](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html)を作成し、操作するための直観的なユーザーインターフェイスを提供します。 フォームデータモデルは、データの交換にデータソースを利用します。
完成したフォームデータモデルは、[ここから](assets/sms-2fa-fdm.zip)ダウンロードできます。

![fdm](assets/2FA-fdm.PNG)

## アダプティブフォームの作成

Form Data ModelのPOST呼び出しをアダプティブフォームに統合し、フォームにユーザーが入力した携帯電話番号を確認します。 独自のアダプティブフォームを自由に作成し、フォームデータモデルのPOST呼び出しを使用して、必要に応じてOTPコードを送信および検証できます。

APIキーでサンプルアセットを使用する場合は、次の手順に従います。

* [フォームデータ](assets/sms-2fa-fdm.zip) モデルをダウンロードし、 [パッケージマネージャーを使用してAEMに読み込みます](http://localhost:4502/crx/packmgr/index.jsp)
* サンプルのアダプティブフォームは、ここから[ダウンロードできます](assets/sms-2fa-verification-af.zip)。 このサンプルフォームは、この記事の一部として提供されるフォームデータモデルのサービス呼び出しを使用しています。
* [FormsおよびドキュメントUI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)からAEMにフォームを読み込みます
* フォームを編集モードで開きます。 次のフィールドのルールエディターを開きます

![sms-send](assets/check-sms.PNG)

* フィールドに関連付けられているルールを編集します。 適切なAPIキーの指定
* フォームを保存する
* [フォームのプレビュー](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) と機能のテスト


