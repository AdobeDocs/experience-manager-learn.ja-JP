---
title: SMS 2 要素認証
description: 特定のアクティビティを実行する際にユーザーの ID を確認するのに役立つ、セキュリティのレイヤーを追加します。
feature: Adaptive Forms
version: 6.4,6.5
kt: 6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 2%

---

# 携帯電話番号を使用したユーザーの検証

SMS 2 要素認証 (Dual Factor Authentication) は、セキュリティ検証手順で、Web サイト、ソフトウェアまたはアプリケーションにログインする際にユーザーがトリガーされます。 ログインプロセスでは、一意の数値コードを含む SMS が携帯電話番号に自動的に送信されます。

このサービスを提供する組織は多数あり、REST API が明確に文書化されている限り、AEM Formsのデータ統合機能を使用してAEM Formsを簡単に統合できます。 このチュートリアルでは、 [Nexmo](https://developer.nexmo.com/verify/overview) を参照して、SMS 2FA の使用例を示します。

Nexmo Verify サービスを使用してAEM Formsで SMS 2FA を実装するには、次の手順に従いました。

## 開発者アカウントを作成

で開発者アカウントを作成します。 [Nexmo](https://dashboard.nexmo.com/sign-in). API キーと API 秘密鍵をメモしておきます。 これらのキーは、Nexmo のサービスの REST API を呼び出すために必要です。

## Swagger/OpenAPI ファイルの作成

OpenAPI Specification（旧称 Swagger Specification）は、REST API の API 記述形式です。 OpenAPI ファイルを使用すると、次のような API 全体を記述できます。

* 使用可能なエンドポイント (/users) および各エンドポイントでの操作 (GET/users、POST/users)
* 操作パラメータ各操作の入力と出力認証方法
* 連絡先情報、ライセンス、利用条件、その他の情報。
* API の仕様は、YAML または JSON で記述できます。 フォーマットは、学習が容易で、人間と機械の両方に対して読み取り可能です。

最初の Swagger/OpenAPI ファイルを作成するには、 [OpenAPI ドキュメント](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Formsは、OpenAPI Specification version 2.0(fka Swagger) をサポートしています。

以下を使用： [swagger editor](https://editor.swagger.io/) :SMS を使用して送信し、OTP コードを検証する操作を記述する swagger ファイルを作成します。 Swagger ファイルは、JSON 形式または YAML 形式で作成できます。 完成した Swagger ファイルは、 [ここ](assets/two-factore-authentication-swagger.zip)

## データソースを作成

AEM/AEM Formsをサードパーティのアプリケーションと統合するには、 [データソースを作成](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) クラウドサービス設定の「 」。

## フォームデータモデルの作成

AEM Formsのデータ統合は、を作成して操作するための直感的なユーザーインターフェイスを提供します [フォームデータモデル](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). フォームデータモデルでは、データの交換にデータソースを使用します。
入力済みのフォームデータモデルは、 [ここからダウンロード](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## アダプティブフォームを作成

フォームデータモデルのPOSTの呼び出しをアダプティブフォームに統合して、フォーム内のユーザーが入力した携帯電話番号を検証します。 独自のアダプティブフォームを自由に作成し、フォームデータモデルのPOST呼び出しを使用して、必要に応じて OTP コードを送信および検証することができます。

API キーでサンプルアセットを使用する場合は、次の手順に従ってください。

* [フォームデータモデルをダウンロードする](assets/sms-2fa-fdm.zip) を使用してAEMにインポートします。 [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)
* サンプルのアダプティブフォームをダウンロードすると、 [ここからダウンロード](assets/sms-2fa-verification-af.zip). このサンプルフォームは、この記事の一部として提供されているフォームデータモデルのサービス呼び出しを使用しています。
* フォームをからAEMに読み込む [Formsとドキュメント UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* フォームを編集モードで開きます。 次のフィールドのルールエディターを開きます。

![sms-send](assets/check-sms.PNG)

* フィールドに関連付けられたルールを編集します。 適切な API キーを指定する
* フォームを保存する
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) 機能をテストします。
