---
title: AEM Forms でのドキュメントの認証
description: Doc Assurance サービスを使用した AEM Forms での PDF ドキュメントの認証
feature: Document Security
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
duration: 75
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 100%

---

# AEM Forms でのドキュメントの認証

認証済みドキュメントは、PDF ドキュメントと Forms の受信者に対して、信頼性と整合性をさらに保証します。

ドキュメントを認証するには、サーバー上の自動プロセスの一部として、デスクトップ上の Acrobat DC または AEM Forms Document Services を使用します。

この記事では、AEM Forms Document Services を使用して PDF ドキュメントを認証するためのサンプル OSGi バンドルを提供します。サンプルで使用されるコードは[ここから入手](https://helpx.adobe.com/jp/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)できます。

AEM Forms を使用してドキュメントを認証するには、次の手順に従う必要があります。

## Trust Store への証明書の追加 {#adding-certificate-to-trust-store}

次に示す手順に従って、証明書を AEM のキーストアに追加してください

* [グローバル Trust Store の初期化](http://localhost:4502/libs/granite/security/content/truststore.html)
* [fd-service ユーザーの検索](http://localhost:4502/security/users.html)
* **fd-service ユーザを検索するには、結果ページをスクロールして、すべてのユーザを読み込む必要があります**。
* fd-service ユーザをダブルクリックして、ユーザ設定ウィンドウを開きます。
* 「秘密鍵をキーストアファイルから追加」をクリックします。証明書に固有のエイリアスとパスワードを指定します。
  ![add-certificate](assets/adding-certificate-keystore.PNG)
* 変更を保存します。

## OSGi サービスの作成

独自の OSGi バンドルを記述し、AEM Forms Client SDK を使用して、PDF ドキュメントを認証するサービスを実装します。次のリンクは、独自の OSGi バンドルを記述する際に役立ちます。

* [最初の OSGi バンドルの作成](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Document Service API の使用](https://helpx.adobe.com/jp/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

または、このチュートリアルアセットの一部に含まれているサンプルバンドルを使用できます。

>[!NOTE]
>
>サンプルバンドルは、「ares」というエイリアスを使用してドキュメントを認証します。したがって、このバンドルを使用する際は、エイリアスが「ares」であることを確認してください。

## ローカルシステムでのサンプルのテスト

* [カスタムドキュメントサービスバンドル](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)をダウンロードしインストールします。
* [サービスユーザーバンドルを使用した開発](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)をダウンロードしインストールします。
* [Apache Sling Service User Mapper Service に次のエントリが追加されていることを確認します](http://localhost:4502/system/console/configMgr)。
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**（以下のスクリーンショットを参照）
  ![User-Mapper](assets/user-mapper-service.PNG)
* [サンプルのアダプティブフォームを読み込みます。](assets/certify-pdf-af.zip)
* [カスタム送信を読み込みインストールします。](assets/custom-submit-certify.zip)
* [アダプティブフォームを開きます](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)。
* 認証が必要な PDF ドキュメントをアップロードします。
  **オプション** - ドキュメントの認証に使用する署名フィールドを指定します。
* 送信をクリックします。
* 認証済みの PDF が返されます。
