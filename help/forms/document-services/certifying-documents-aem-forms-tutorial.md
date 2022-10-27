---
title: AEM Formsでのドキュメントの認証
description: Docassurance サービスを使用したAEM FormsでのPDFドキュメントの認証
feature: Document Security
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 3%

---

# AEM Formsでのドキュメントの認証

認証済みドキュメントは、PDFドキュメントを提供し、受信者をフォーム化し、信頼性と整合性をさらに保証します。

ドキュメントを認証するには、サーバー上の自動プロセスの一部として、デスクトップ上のAcrobat DCまたはAEM Forms Document Services を使用します。

この記事では、AEM Forms Document Services を使用して PDF ドキュメントを認証するためのサンプル OSGi バンドルを提供します。サンプルで使用されるコードは次のとおりです。 [こちらから](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

AEM Formsを使用してドキュメントを認証するには、次の手順に従う必要があります

## Trust Store に証明書を追加中 {#adding-certificate-to-trust-store}

次に示す手順に従って、証明書をAEMのキーストアに追加してください

* [グローバルトラストストアを初期化](http://localhost:4502/libs/granite/security/content/truststore.html)
* [fd-service を検索します。](http://localhost:4502/security/users.html) ユーザー
* **fd-service ユーザを検索するには、すべてのユーザを読み込むために、結果ページをスクロールする必要があります**
* fd-service ユーザをダブルクリックして、ユーザ設定ウィンドウを開きます。
* 「秘密鍵をキーストアファイルから追加」をクリックします。証明書に固有のエイリアスとパスワードを指定します。
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* 変更を保存します

## OSGi サービスの作成

独自の OSGi バンドルを作成し、AEM Forms Client SDK を使用してサービスを実装し、PDFドキュメントを認証できます。 次のリンクは、独自の OSGi バンドルを書き込む際に役立ちます。

* [最初の OSGi バンドルの作成](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/aem-project-archetype.html?lang=ja)
* [ドキュメントサービス API の使用](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

または、このチュートリアルアセットの一部に含まれているサンプルバンドルを使用できます。

>[!NOTE]
>
>サンプルバンドルは、「ares」と呼ばれるエイリアスを使用してドキュメントを認証します。 このバンドルを使用する際は、エイリアスが「ares」と呼ばれていることを確認してください

## ローカルシステムでのサンプルのテスト

* ダウンロードとインストール [カスタムドキュメントサービスバンドル](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* ダウンロードとインストール [サービスユーザーバンドルを使用した開発](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Apache Sling Service User Mapper Service に次のエントリが追加されていることを確認します。](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** 次のスクリーンショットに示すように
   ![User-Mapper](assets/user-mapper-service.PNG)
* [サンプルのアダプティブフォームを読み込む](assets/certify-pdf-af.zip)
* [カスタム送信の読み込みとインストール](assets/custom-submit-certify.zip)
* [アダプティブフォームを開く](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* 認証が必要なPDFドキュメントをアップロード
   **オプション**  — ドキュメントの認証に使用する署名フィールドを指定します
* 「送信」をクリックします。
* 認定PDFが返送されます。
