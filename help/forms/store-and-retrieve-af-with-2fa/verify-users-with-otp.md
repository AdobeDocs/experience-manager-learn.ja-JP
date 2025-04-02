---
title: OTP でユーザーを検証
description: OTP を使用して、アプリケーション番号に関連付けられている携帯電話番号を確認します。
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
duration: 84
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '403'
ht-degree: 100%

---

# OTP でユーザーを検証

SMS 2 要素認証は、セキュリティ検証手順で、web サイト、ソフトウェアまたはアプリケーションにユーザーがログインする際にトリガーされます。ログインプロセスでは、一意の数値コードを含む SMS が携帯電話番号に自動的に送信されます。

このサービスを提供する組織は多数あり、REST API が明確に文書化されている限り、AEM Forms のデータ統合機能を使用してAEM Forms を簡単に統合できます。 このチュートリアルでは、[Nexmo](https://developer.nexmo.com/verify/overview) を参照して、SMS 2FA の使用例を示します。

Nexmo Verify サービスを使用して AEM Forms で SMS 2FA を実装するために、次の手順に従いました。

## 開発者アカウントの作成

[Nexmo](https://dashboard.nexmo.com/sign-in) で開発者アカウントを作成します。API キーと API 秘密鍵をメモしておきます。 これらのキーは、Nexmo のサービスの REST API を呼び出すために必要です。

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

[swagger エディター](https://editor.swagger.io/) を使用して、SMS を使用して送信された OTP コードを送信および検証する操作を記述した swagger ファイルを作成します。Swagger ファイルは、JSON 形式または YAML 形式で作成できます。 完成した Swagger ファイルは、 [こちら](assets/two-factore-authentication-swagger.zip) からダウンロードできます

## データソースの作成

AEM／AEM Forms をサードパーティ製アプリケーションと統合するには、クラウドサービス設定で [swagger ファイルを使用した ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=ja)REST ベースのデータソースが必要です。完成したデータソースは、このコースのアセットの一部として提供されます。

## フォームデータモデルの作成

AEM Forms データ統合は、[フォームデータモデル](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=ja)を作成して操作するための直感的なユーザーインターフェイスを提供します。フォームデータモデルでは、データの交換にデータソースを使用します。
入力済みのフォームデータモデルは、[ここからダウンロード](assets/sms-2fa-fdm.zip)できます。

![fdm](assets/2FA-fdm.PNG)

[メインフォームの作成](./create-the-main-adaptive-form.md)
