---
title: AEM Forms 6.3および6.4でのSalesforceを使用したデータソースの設定
seo-title: AEM Forms 6.3および6.4でのSalesforceを使用したデータソースの設定
description: フォームデータモデルを使用したAEM FormsとSalesforceの統合
seo-description: フォームデータモデルを使用したAEM FormsとSalesforceの統合
uuid: 0124526d-f1a3-4f57-b090-a418a595632e
feature: アダプティブForms、フォームデータモデル
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 8e314fc3-62d0-4c42-b1ff-49ee34255e83
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---


# AEM Forms 6.3および6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}でのSalesforceを使用したデータソースの設定

## 前提条件 {#prerequisites}

この記事では、Salesforceを使用したデータソースの作成プロセスを順を追って説明します

このチュートリアルの前提条件：

* このページの下部までスクロールし、Swaggerファイルをダウンロードして、ハードドライブに保存します。
* SSLが有効なAEM Forms

   * [AEM 6.3でのSSLの有効化に関する公式ドキュメント](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [AEM 6.4でのSSLの有効化に関する公式ドキュメント](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Salesforceアカウントが必要です。
* 接続済みアプリを作成する必要があります。 アプリを作成するための公式ドキュメントフォームSalesforceは、[ここ](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0)に記載されています。
* アプリに適したOAuth範囲を指定します（テストのために使用可能なすべてのOAuth範囲を選択しました）。
* コールバックURLを指定します。 この場合のコールバックURLは

   * **AEM Forms 6.3**&#x200B;を使用している場合、コールバックURLはhttps://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.htmlになります。 このURLでは、createleadはフォームデータモデルの名前になります。

   * ** AEM Forms 6.4**を使用している場合、コールバックURLは[https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html](https://gbedekar-w7-1:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html)になります。

この例では、 gbedekar -w7-1:6443は、AEMが実行されているサーバーとポートの名前です。

Connected Appを作成したら、**Consumer KeyとSecret Key**&#x200B;をメモします。 AEM Formsでデータソースを作成する場合は、これらが必要になります。

接続されたアプリを作成したら、Salesforceで実行する必要がある操作用のSwaggerファイルを作成する必要があります。 サンプルのSwaggerファイルが、ダウンロード可能なアセットの一部として含まれています。 このSwaggerファイルを使用すると、アダプティブフォーム送信時に「リード」オブジェクトを作成できます。 このSwaggerファイルを参照してください。

次の手順は、AEM Formsでデータソースを作成することです。 ご使用のAEM Formsのバージョンに従って、次の手順に従ってください

## AEM Forms 6.3 {#aem-forms}

* httpsプロトコルを使用してAEM Formsにログインします。
* https://&lt;servername>:&lt;serverport> /etc/cloudservices.htmlと入力して、クラウドサービスに移動します。例：https://gbedekar-w7-1:6443/etc/cloudservices.html
* 下の「フォームデータモデル」までスクロールします。
* 「設定を表示」をクリックします。
* 「+」をクリックして新しい設定を追加します。
* 「Rest Full Service」を選択します。 設定に意味のあるタイトルと名前を指定します。 次に例を示します。

   * 名前：CreateLeadInSalesForce
   * タイトル：CreateLeadInSalesForce

* 「作成」をクリックします。

**次の画面で**

* Swaggerソースファイルのオプションとして「File」を選択します。 前にダウンロードしたファイルを参照します。
* 「認証タイプ」として「 OAuth2.0 」を選択します。
* ClientIDとClient Secretの値を指定します。
* OAuth Urlは — **https://login.salesforce.com/services/oauth2/authorize**&#x200B;です
* 更新トークンUrl - **https://na5.salesforce.com/services/oauth2/token**
* **トークURLへのアクセス — https://na5.salesforce.com/services/oauth2/token**
* 承認範囲：** api   chatter_api完全ID   openid   refresh_token visualforce web**
* 認証ハンドラー：承認ベアラ
* 「OAUTHに接続」をクリックします。問題が解決した場合、エラーは表示されません

Salesforceを使用してフォームデータモデルを作成したら、作成したデータソースを使用してフォームデータ統合を作成できます。 フォームデータ統合の作成に関する公式ドキュメントは、[こちら](https://helpx.adobe.com/aem-forms/6-3/data-integration.html)にあります。

SFDCでリードオブジェクトを作成するPOSTサービスを含めるように、フォームデータモデルを設定してください。

また、リードオブジェクトの読み取りサービスと書き込みサービスも設定する必要があります。 このページの下部にあるスクリーンショットを参照してください。

フォームデータモデルの作成後、このモデルに基づいてアダプティブFormsを作成し、フォームデータモデルの送信方法を使用してSFDCでリードを作成することができます。

## AEM Forms 6.4 {#aem-forms-1}

* データソースの作成

   * [データソースへの移動](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * 「作成」ボタンをクリックします。
   * 意味のある値の提供

      * 名前：CreateLeadInSalesForce
      * タイトル：CreateLeadInSalesForce
      * サービスの種類：RESTfulサービス
   * 「次へ」をクリックします。
   * Swaggerソース：ファイル
   * 前の手順でダウンロードしたSwaggerファイルを参照して選択します。
   * 認証の種類：OAuth 2.0。次の値を指定します。
   * ClientIDとClient Secretの値を指定します。
   * OAuth Urlは — **https://login.salesforce.com/services/oauth2/authorize**&#x200B;です
   * 更新トークンUrl - **https://na5.salesforce.com/services/oauth2/token**
   * アクセストークンUr **l - https://na5.salesforce.com/services/oauth2/token**
   * 承認範囲：** api chatter_api完全id openid refresh_token visualforce web**
   * 認証ハンドラー：承認ベアラ
   * 「OAuthに接続」ボタンをクリックします。 エラーが発生した場合は、上記の手順を確認して、すべての情報が正しく入力されていることを確認してください。


SalesForceを使用してデータソースを作成したら、作成したデータソースを使用してフォームデータ統合を作成できます。 のドキュメントリンクは[here](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)です。

SFDCでリードオブジェクトを作成するPOSTサービスを含めるように、フォームデータモデルを設定してください。

また、リードオブジェクトの読み取りサービスと書き込みサービスも設定する必要があります。 このページの下部にあるスクリーンショットを参照してください。

フォームデータモデルの作成後、このモデルに基づいてアダプティブFormsを作成し、フォームデータモデルの送信方法を使用してSFDCでリードを作成することができます。

>[!NOTE]
>
>Swaggerファイル内のURLが地域に対応していることを確認します。 例えば、サンプルのswaggerファイルのURLは、北米でアカウントが作成されたので、「na46.salesforce.com」です。 最も簡単な方法は、SalesforceアカウントにログインしてURLを確認することです。

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
