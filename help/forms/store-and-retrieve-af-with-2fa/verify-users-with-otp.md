---
title: OTP でのユーザーの検証
description: OTP を使用して、アプリケーション番号に関連付けられている携帯電話番号を確認します。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 0%

---

# OTP でのユーザーの検証

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

AEM/AEM Formsをサードパーティのアプリケーションと統合するには、 [Swagger ファイルを使用した REST ベースのデータソース](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) クラウドサービス設定の「 」。 完成したデータソースは、このコースのアセットの一部として提供されます。

## フォームデータモデルの作成

AEM Formsのデータ統合は、を作成して操作するための直感的なユーザーインターフェイスを提供します [フォームデータモデル](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). フォームデータモデルでは、データの交換にデータソースを使用します。
入力済みのフォームデータモデルは、 [ここからダウンロード](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)
