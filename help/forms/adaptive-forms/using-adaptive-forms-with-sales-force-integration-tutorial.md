---
title: AEM Forms6.3および6.4でのSalesforceを使用したデータソースの設定
seo-title: AEM Forms6.3および6.4でのSalesforceを使用したデータソースの設定
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
role: デベロッパー
level: 経験豊富な
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '936'
ht-degree: 0%

---


# AEM Forms6.3および6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}でのSalesforceを使用したDataSourceの設定

## 前提条件 {#prerequisites}

この記事では、Salesforceを使用したデータソースの作成プロセスについて説明します。

このチュートリアルの前提条件：

* このページの下部までスクロールし、Swaggerファイルをダウンロードして、ハードドライブに保存します。
* SSLが有効なAEM Forms

   * [AEM 6.3でSSLを有効にするための公式ドキュメント](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [AEM 6.4でSSLを有効にするための公式ドキュメント](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Salesforceアカウントが必要です
* 接続されたアプリを作成する必要があります。 Salesforceのアプリ作成用の公式ドキュメントフォームは、[ここ](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0)に記載されています。
* アプリに適切なOAuthスコープを指定します（テストの目的で使用可能なすべてのOAuthスコープを選択しました）
* コールバックURLを指定します。 この場合のコールバックURLは

   * **AEM Forms6.3**&#x200B;を使用している場合、コールバックURLはhttps://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.htmlになります。 このURLでは、createleadがmy form data modelの名前です。

   * **AEM Forms6.4**を使用している場合、コールバックURLは[https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html](https://gbedekar-w7-1:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html)になります。

この例では、gbedekar -w7-1:6443は、AEMが実行されているサーバーの名前とポートです。

接続されたアプリのメモを作成したら、**Consumer keyと秘密鍵**&#x200B;を書き留めます。 これらは、AEM Formsでデータソースを作成する場合に必要です。

接続されたアプリを作成したら、salesforceで実行する必要がある操作用のスウォッガーファイルを作成する必要があります。 サンプルのSwaggerファイルは、ダウンロード可能なアセットの一部として含まれています。 このSwaggerファイルを使用すると、アダプティブフォームの送信時に「リード」オブジェクトを作成できます。 このSwaggerファイルを探索してください。

次の手順は、AEM Formsでデータソースを作成することです。 ご使用のAEM Forms版に従い、次の手順に従ってください

## AEM Forms6.3 {#aem-forms}

* httpsプロトコルを使用してAEM Formsにログインする
* https://&lt;servername>:&lt;serverport> /etc/cloudservices.htmlを入力して、クラウドサービスに移動します。例：https://gbedekar-w7-1:6443/etc/cloudservices.html
* 「Form Data Model」まで下にスクロールします。
* 「設定を表示」をクリックします。
* 「+」をクリックして新しい設定を追加します
* 「Rest Full Service」を選択します。 設定に意味のあるタイトルと名前を指定します。 次に例を示します。

   * 名前：CreateLeadInSalesForce
   * タイトル：CreateLeadInSalesForce

* 「作成」をクリックします。

**次の画面に表示**

* Swaggerソースファイルのオプションとして「ファイル」を選択します。 以前にダウンロードしたファイルを参照します
* 「認証の種類」に「OAuth2.0」を選択します
* ClientIDとClient Secret値の指定
* OAuth URLは — **https://login.salesforce.com/services/oauth2/authorize**&#x200B;です。
* 更新トークンURL - **https://na5.salesforce.com/services/oauth2/token**
* **アクセストークURL - https://na5.salesforce.com/services/oauth2/token**
* 承認範囲：** api   chatter_apiフルID   openid   refresh_token visualforce web**
* 認証ハンドラー：認可担持人
* 「OAUTHに接続」をクリックします。問題が解決しない場合は、エラーは表示されません

Salesforceを使用してフォームデータモデルを作成したら、先ほど作成したデータソースを使用してフォームデータ統合を作成できます。 フォームデータ統合を作成するための公式ドキュメントは[ここ](https://helpx.adobe.com/aem-forms/6-3/data-integration.html)です。

SFDCでリードオブジェクトを作成するPOSTサービスが含まれるように、Form Data Modelを設定していることを確認します。

また、リードオブジェクトに対して読み取り/書き込みサービスを設定する必要もあります。 このページの下部にあるスクリーンショットを参照してください。

フォームデータモデルの作成後は、このモデルに基づいてアダプティブFormsを作成し、フォームデータモデルの送信方法を使用してSFDCでリードを作成できます。

## AEM Forms6.4 {#aem-forms-1}

* データソースの作成

   * [データソースに移動します。](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * 「作成」ボタンをクリックします
   * 意味のある値をいくつか指定する

      * 名前：CreateLeadInSalesForce
      * タイトル：CreateLeadInSalesForce
      * サービスの種類：RESTfulサービス
   * 「次へ」をクリック
   * Swagger Source:ファイル
   * 前の手順でダウンロードしたSwaggerファイルを参照して選択します
   * 認証の種類：OAuth 2.0。次の値を指定します
   * ClientIDとClient Secret値の指定
   * OAuth URLは — **https://login.salesforce.com/services/oauth2/authorize**&#x200B;です。
   * 更新トークンURL - **https://na5.salesforce.com/services/oauth2/token**
   * アクセストークンUr **l - https://na5.salesforce.com/services/oauth2/token**
   * 承認範囲：** api chatter_api full id openid refresh_token visualforce web**
   * 認証ハンドラー：認可担持人
   * 「OAuthに接続」ボタンをクリックします。 エラーが表示された場合は、前述の手順を確認して、すべての情報が正しく入力されていることを確認してください。


SalesForceを使用してデータソースを作成したら、先ほど作成したデータソースを使用してフォームデータ統合を作成できます。 そのドキュメントのリンクは[ここ](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)です

SFDCでリードオブジェクトを作成するPOSTサービスが含まれるように、Form Data Modelを設定していることを確認します。

また、リードオブジェクトに対して読み取り/書き込みサービスを設定する必要もあります。 このページの下部にあるスクリーンショットを参照してください。

フォームデータモデルの作成後は、このモデルに基づいてアダプティブFormsを作成し、フォームデータモデルの送信方法を使用してSFDCでリードを作成できます。

>[!NOTE]
>
>Swaggerファイル内のurlが、使用する地域に対応していることを確認します。 例えば、サンプルのSwaggerファイルのURLは、北米で作成されたアカウント「na46.salesforce.com」です。 最も簡単な方法は、Salesforceアカウントにログインし、urlを確認することです。

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
