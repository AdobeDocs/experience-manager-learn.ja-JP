---
title: AEM Forms 6.3 および 6.4 での Salesforce とのデータソースの設定
description: フォームデータモデルを使用したAEM Formsと Salesforce の統合
feature: Adaptive Forms, Form Data Model
topics: integrations
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7a4fd109-514a-41a8-a3fe-53c1de32cb6d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '892'
ht-degree: 0%

---

# AEM Forms 6.3 および 6.4 での Salesforce とのデータソースの設定{#configuring-datasource-with-salesforce-in-aem-forms-and}

## 前提条件 {#prerequisites}

この記事では、Salesforce を使用したデータソースの作成プロセスを順を追って説明します

このチュートリアルの前提条件：

* このページの下部までスクロールし、Swagger ファイルをダウンロードして、ハードドライブに保存します。
* SSL が有効なAEM Forms

   * [AEM 6.3 での SSL の有効化に関する公式ドキュメント](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [AEM 6.4 での SSL の有効化に関する公式ドキュメント](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Salesforce アカウントが必要です
* 接続アプリを作成する必要があります。 アプリ作成用の Salesforce 公式ドキュメントが一覧表示されます [ここ](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* アプリに適した OAuth 範囲を指定します（テストの目的で使用可能なすべての OAuth 範囲を選択しました）。
* コールバック URL を指定します。 この場合のコールバック URL は

   * 次を使用する場合： **AEM Forms 6.3**&#x200B;の場合、コールバック URL はhttps://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.htmlです。 この URL createlead は、フォームデータモデルの名前です。

   * ** AEM Forms 6.4**を使用している場合、コールバック URL はhttps://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.htmlです。

この例では、gbedekar -w7-1:6443 は、AEMが実行されているサーバーとポートの名前です。

Connected App を作成した後、 **消費者キーと秘密鍵**. AEM Formsでデータソースを作成する場合は、これらが必要です。

接続されたアプリを作成したら、Salesforce で実行する必要のある操作用の Swagger ファイルを作成する必要があります。 ダウンロード可能なアセットの一部として、サンプルの Swagger ファイルが含まれています。 この Swagger ファイルを使用すると、アダプティブフォーム送信時に「リード」オブジェクトを作成できます。 この Swagger ファイルを参照してください。

次の手順では、AEM Formsでデータソースを作成します。 ご使用のAEM Formsのバージョンに従って、次の手順に従ってください

## AEM Forms 6.3 {#aem-forms}

* https プロトコルを使用してAEM Formsにログインします。
* https://と入力してクラウドサービスに移動します。&lt;servername>:&lt;serverport> /etc/cloudservices.html、例： https://gbedekar-w7-1:6443/etc/cloudservices.html
* 下にスクロールして「フォームデータモデル」を表示します。
* 「設定を表示」をクリックします。
* 「+」をクリックして新しい設定を追加
* 「Rest Full Service」を選択します。 設定に意味のあるタイトルと名前を指定します。 次に例を示します。

   * 名前：CreateLeadInSalesForce
   * タイトル：CreateLeadInSalesForce

* 「作成」をクリックします。

**次の画面で**

* Swagger ソースファイルのオプションとして「ファイル」を選択します。 前にダウンロードしたファイルを参照します。
* 「認証タイプ」として「 OAuth2.0 」を選択します。
* ClientID と Client Secret の値を指定します。
* OAuth Url は — です **https://login.salesforce.com/services/oauth2/authorize**
* 更新トークン URL - **https://na5.salesforce.com/services/oauth2/token**
* **トーク URL にアクセス — https://na5.salesforce.com/services/oauth2/token**
* 認証範囲：** api chatter_api 完全 id openid refresh_token visualforce web**
* 認証ハンドラ：認証ベアラ
* 「OAuth に接続」をクリックします。問題が解決した場合、エラーは表示されません

Salesforce を使用してフォームデータモデルを作成したら、作成したデータソースを使用してフォームデータ統合を作成できます。 フォームデータ統合の作成に関する公式ドキュメントは次のとおりです。 [ここ](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

SFDC でリードオブジェクトを作成するPOSTサービスを含めるようにフォームデータモデルを設定してください。

また、リードオブジェクトの読み取りサービスと書き込みサービスも設定する必要があります。 このページの下部にあるスクリーンショットを参照してください。

フォームデータモデルを作成したら、このモデルに基づいてアダプティブFormsを作成し、フォームデータモデルの送信方法を使用して SFDC でリードを作成できます。

## AEM Forms 6.4 {#aem-forms-1}

* データソースを作成

   * [データソースに移動](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * 「作成」ボタンをクリックします。
   * 意味のある値をいくつか指定します。

      * 名前：CreateLeadInSalesForce
      * タイトル：CreateLeadInSalesForce
      * サービスタイプ：RESTful サービス
   * 「次へ」をクリックします。
   * Swagger ソース：ファイル
   * 前の手順でダウンロードした Swagger ファイルを参照して選択します。
   * 認証タイプ：OAuth 2.0。次の値を指定します。
   * ClientID と Client Secret の値を指定します。
   * OAuth Url は — です **https://login.salesforce.com/services/oauth2/authorize**
   * 更新トークン URL - **https://na5.salesforce.com/services/oauth2/token**
   * アクセストークン URL **l - https://na5.salesforce.com/services/oauth2/token**
   * 認証範囲：** api chatter_api 完全 id openid refresh_token visualforce web**
   * 認証ハンドラ：認証ベアラ
   * 「OAuth に接続」ボタンをクリックします。 エラーが発生した場合は、上記の手順を確認し、すべての情報が正しく入力されていることを確認してください。


SalesForce を使用してデータソースを作成したら、作成したデータソースを使用してフォームデータ統合を作成できます。 のドキュメントリンク。 [ここ](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

SFDC でリードオブジェクトを作成するPOSTサービスを含めるようにフォームデータモデルを設定してください。

また、リードオブジェクトの読み取りサービスと書き込みサービスも設定する必要があります。 このページの下部にあるスクリーンショットを参照してください。

フォームデータモデルを作成したら、このモデルに基づいてアダプティブFormsを作成し、フォームデータモデルの送信方法を使用して SFDC でリードを作成できます。

>[!NOTE]
>
>Swagger ファイル内の url が自分の地域に対応していることを確認します。 例えば、サンプルの swagger ファイルの URL は、北米でアカウントが作成されたので、「na46.salesforce.com」です。 最も簡単な方法は、Salesforce アカウントにログインして URL を確認することです。

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
