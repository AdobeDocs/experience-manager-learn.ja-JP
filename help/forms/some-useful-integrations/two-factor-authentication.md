---
title: SMS 2 要素認証
description: 特定のアクティビティを実行する際にユーザーの ID を確認するのに役立つ、セキュリティのレイヤーを追加します。
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 115
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 100%

---

# 携帯電話番号を使用したユーザーの検証

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

AEM／AEM Forms をサードパーティのアプリケーションと統合するには、クラウドサービス設定で[データソースを作成](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=ja)する必要があります。

## フォームデータモデルの作成

AEM Forms データ統合は、[フォームデータモデル](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=ja)を作成して操作するための直感的なユーザーインターフェイスを提供します。フォームデータモデルでは、データの交換にデータソースを使用します。
入力済みのフォームデータモデルは、[ここからダウンロード](assets/sms-2fa-fdm.zip)できます。

![fdm](assets/2FA-fdm.PNG)

## アダプティブフォームの作成

フォームデータモデルの POST の呼び出しをアダプティブフォームに統合して、フォーム内のユーザーが入力した携帯電話番号を検証します。 独自のアダプティブフォームを自由に作成し、フォームデータモデルの POST 呼び出しを使用して、必要に応じて OTP コードを送信および検証できます。

API キーでサンプルアセットを使用する場合は、次の手順に従ってください。

* [フォームデータモデルをダウンロード](assets/sms-2fa-fdm.zip)して、[パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して AEM に読み込む
* サンプルのアダプティブフォームは[こちらからダウンロード](assets/sms-2fa-verification-af.zip)できます。このサンプルフォームは、この記事の一部として提供されているフォームデータモデルのサービス呼び出しを使用しています。
* [Forms とドキュメント UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) からフォームを AEM に読み込む
* フォームを編集モードで開きます。次のフィールドのルールエディターを開きます

![sms-send](assets/check-sms.PNG)

* フィールドに関連付けられたルールを編集します。 適切な API キーを指定する
* フォームを保存する
* [フォームをプレビュー](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled)して、機能をテストする
