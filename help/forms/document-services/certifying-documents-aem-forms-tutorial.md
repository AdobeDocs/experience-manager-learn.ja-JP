---
title: AEM Formsでのドキュメントの認証
seo-title: AEM Formsでのドキュメントの認証
description: Docassuranceサービスを使用したAEM FormsでのPDFドキュメントの認証
seo-description: Docassuranceサービスを使用したAEM FormsでのPDFドキュメントの認証
uuid: ecb1f9b6-bbb3-43a3-a0e0-4c04411acc9f
feature: Document Security
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開発
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 1%

---


# AEM Formsでのドキュメントの認証

認証済みドキュメントは、PDFドキュメントとフォームの受信者に対して、信頼性と整合性をさらに保証します。

ドキュメントを認証するには、デスクトップ上またはAEM Forms Document Services上でAcrobat DCを、サーバー上の自動プロセスの一部として使用できます。

この記事では、AEM Forms Document Servicesを使用してPDFドキュメントを認証するためのサンプルOSGIバンドルを提供します。サンプルで使用されるコードは、[こちら](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)から入手できます。

AEM Formsを使用してドキュメントを認証するには、次の手順に従う必要があります

## Trust Storeに証明書を追加しています{#adding-certificate-to-trust-store}

以下の手順に従って、AEMのキーストアに証明書を追加してください

* [グローバルTrust Storeの初期化](http://localhost:4502/libs/granite/security/content/truststore.html)
* [fd-serviceuserを検索し](http://localhost:4502/security/users.html) ます
* **fd-serviceユーザーを検索するには、結果ページをスクロールしてすべてのユーザーを読み込む必要があります**
* fd-serviceユーザーをダブルクリックして、ユーザー設定ウィンドウを開きます。
* 「キーストアファイルから秘密鍵を追加」をクリックします。証明書に固有のエイリアスとパスワードを指定します。
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* 変更を保存します

## OSGIサービスの作成

独自のOSGiバンドルを作成し、AEM Forms Client SDKを使用してPDFドキュメントを認証するサービスを実装できます。 次のリンクは、独自のOSGiバンドルを書き込む際に役立ちます。

* [最初のOSGiバンドルの作成](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Document Service APIの使用](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

または、このチュートリアルアセットの一部として含まれているサンプルバンドルを使用できます。

>[!NOTE]
>
>サンプルバンドルは、「ares」というエイリアスを使用してドキュメントを認証します。 このバンドルを使用する際は、エイリアスが「ares」と呼ばれていることを確認してください

## ローカルシステムでのサンプルのテスト

* [カスタムドキュメントサービスバンドル](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)をダウンロードしてインストールします。
* [Developing with Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)をダウンロードしてインストールします。
* [Apache Sling Service User Mapper Serviceに次のエントリが追加されていることを確認します。](http://localhost:4502/system/console/configMgr)

   **以下のスクリーンショットに示すDevelopingWithServiceUser.core:getformsresourceresolver=fd-** serviceas
   ![User-Mapper](assets/user-mapper-service.PNG)
* [サンプルアダプティブフォームの読み込み](assets/certify-pdf-af.zip)
* [カスタム送信の読み込みとインストール](assets/custom-submit-certify.zip)
* [アダプティブフォームを開く](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* 認証が必要なPDFドキュメントのアップロード
   **オプション**  — ドキュメントの認証に使用する署名フィールドを指定します
* 「送信」をクリックします。
* 認証済みのPDFが返されます。


