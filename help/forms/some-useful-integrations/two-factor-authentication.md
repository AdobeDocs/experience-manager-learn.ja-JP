---
title: SMS 2要素認証
description: 特定のアクティビティを実行する際にユーザーのIDを確認するのに役立つ、セキュリティのレイヤーを追加します。
feature: アダプティブフォーム
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 3%

---



# 携帯電話番号を使用したユーザーの検証

SMS 2要素認証(Dual Factor Authentication)は、セキュリティ検証手順で、ユーザーがWebサイト、ソフトウェアまたはアプリケーションにログインすることでトリガーされます。 ログインプロセスでは、ユーザーは一意の数値コードを含むSMSを携帯電話番号に自動的に送信します。

このサービスを提供する組織は多数あり、REST APIについて詳しく文書化されている限り、AEM Formsのデータ統合機能を使用してAEM Formsを簡単に統合できます。 このチュートリアルの目的で、[Nexmo](https://developer.nexmo.com/verify/overview)を使用してSMS 2FAの使用例を示しました。

Nexmo Verifyサービスを使用してAEM FormsでSMS 2FAを実装するには、次の手順に従いました。

## 開発者アカウントの作成

[Nexmo](https://dashboard.nexmo.com/sign-in)で開発者アカウントを作成します。 APIキーとAPI秘密鍵をメモしておきます。 これらのキーは、NexmoのサービスのREST APIを呼び出すために必要です。

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

[swaggerエディター](https://editor.swagger.io/)を使用して、SMSを使用して送信し、OTPコードを検証する操作を記述するswaggerファイルを作成します。 Swaggerファイルは、JSON形式またはYAML形式で作成できます。 完成したSwaggerファイルは、[こちら](assets/two-factore-authentication-swagger.zip)からダウンロードできます。

## データソースの作成

AEM/AEM Formsをサードパーティのアプリケーションと統合するには、クラウドサービス設定で[データソース](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)を作成する必要があります。

## フォームデータモデルを作成

AEM Formsのデータ統合機能は、[フォームデータモデル](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html)を作成して操作するための直感的なユーザーインターフェイスを提供します。 フォームデータモデルは、データの交換にデータソースを利用します。
完成したフォームデータモデルは、[こちら](assets/sms-2fa-fdm.zip)からダウンロードできます。

![fdm](assets/2FA-fdm.PNG)

## アダプティブフォームの作成

フォームデータモデルのPOSTの呼び出しをアダプティブフォームに統合して、フォームにユーザーが入力した携帯電話番号を確認します。 独自のアダプティブフォームを自由に作成し、フォームデータモデルのPOST呼び出しを使用して、必要に応じてOTPコードを送信および検証できます。

サンプルアセットをAPIキーと共に使用する場合は、次の手順に従います。

* [フォームデータモデルをダウンロ](assets/sms-2fa-fdm.zip) ードし、パッケージマネージャーを使用してAEMに [読み込みます。](http://localhost:4502/crx/packmgr/index.jsp)
* サンプルのアダプティブフォームは、[こちら](assets/sms-2fa-verification-af.zip)からダウンロードできます。 このサンプルフォームは、この記事の一部として提供されるフォームデータモデルのサービス呼び出しを使用しています。
* [FormsとDocument UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)からAEMにフォームを読み込みます。
* フォームを編集モードで開きます。 次のフィールドのルールエディターを開きます。

![sms-send](assets/check-sms.PNG)

* フィールドに関連付けられたルールを編集します。 適切なAPIキーの指定
* フォームの保存
* [フォームのプレ](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) ビューと機能のテスト


