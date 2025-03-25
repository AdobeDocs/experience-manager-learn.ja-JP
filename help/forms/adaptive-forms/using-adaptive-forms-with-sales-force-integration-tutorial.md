---
title: AEM Forms 6.3 および 6.4 での Salesforce を使用したデータソースの設定
description: フォームデータモデルを使用した AEM Forms と Salesforce の統合
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7a4fd109-514a-41a8-a3fe-53c1de32cb6d
last-substantial-update: 2020-02-14T00:00:00Z
duration: 175
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 100%

---

# AEM Forms 6.3 および 6.4 での Salesforce を使用したデータソースの設定{#configuring-datasource-with-salesforce-in-aem-forms-and}

## 前提条件 {#prerequisites}

この記事では、Salesforce を使用したデータソースの作成プロセスを順を追って説明します。

このチュートリアルの前提条件は次のとおりです。

* このページの下部までスクロールし、Swagger ファイルをダウンロードして、ハードドライブに保存します。
* SSL が有効な AEM Forms

   * [AEM 6.3 での SSL の有効化に関する公式ドキュメント](https://helpx.adobe.com/jp/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [AEM 6.4 での SSL の有効化に関する公式ドキュメント](https://helpx.adobe.com/jp/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Salesforce アカウントが必要です。
* 連携されたアプリを作成する必要があります。アプリ作成用の Salesforce 公式ドキュメントフォームについては、[こちら](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0)を参照してください。
* アプリに適した OAuth 範囲を指定します（ここでは、テスト用に、使用可能なすべての OAuth 範囲が選択されています）。
* コールバック URL を指定します。ここでは、コールバック URL は次のとおりです。

   * **AEM Forms 6.3** を使用する場合は、https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html。この URL の createlead はフォームデータモデルの名前です。

   * **AEM Forms 6.4** を使用する場合は、https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html。

この例の gbedekar -w7-1:6443 は、AEM が動作しているサーバーの名前とポートです。

連携されたアプリを作成したら、**Consumer Key と秘密鍵**&#x200B;をメモします。AEM Forms でデータソースを作成する際に必要になります。

連携されたアプリを作成したので、次は、Salesforce で実行する必要がある操作の Swagger ファイルを作成する必要があります。ダウンロード可能なアセットの一部として、サンプルの Swagger ファイルが含まれています。この Swagger ファイルを使用すると、アダプティブフォームの送信時に「リード」オブジェクトを作成できます。詳しくは、この Swagger ファイルを調べてみてください。

次の手順では、AEM Forms でデータソースを作成します。お使いの AEM Forms のバージョンに応じて、次の手順に従ってください。

## AEM Forms 6.3 {#aem-forms}

* https プロトコルを使用して AEM Forms にログインします。
* 「https://&lt;servername>:&lt;serverport> /etc/cloudservices.html, For example, https://gbedekar-w7-1:6443/etc/cloudservices.html」と入力して Cloud Services に移動します。
* 「フォームデータモデル」まで下にスクロールします。
* 「設定を表示」をクリックします。
* 「+」をクリックして、新しい設定を追加します。
* 「フルサービスをリセット」を選択します。設定に、意味のあるタイトルと名前を付けます。次に例を示します。

   * 名前：CreateLeadInSalesForce
   * タイトル：CreateLeadInSalesForce

* 「作成」をクリックします。

**次の画面で以下を行います。**

* Swagger ソースファイルのオプションとして「ファイル」を選択します。前にダウンロードしたファイルを参照します。
* 「認証タイプ」として OAuth2.0 を選択します。
* 「クライアント ID」と「クライアント秘密鍵」の値を指定します。
* OAuth URL：**https://login.salesforce.com/services/oauth2/authorize**
* 更新トークン URL：**https://na5.salesforce.com/services/oauth2/token**
* **アクセストークン URL：https://na5.salesforce.com/services/oauth2/token**
* 認証範囲：** api chatter_api full id openid refresh_token visualforce web**
* 認証ハンドラ：認証ベアラー
* 「OAuth に接続」をクリックします。問題がなければ、エラーは表示されません。

Salesforce を使用するフォームデータモデルを作成したら、作成したデータソースを使用してフォームデータ統合を作成します。フォームデータ統合の作成に関する公式ドキュメントについては、[こちら](https://helpx.adobe.com/jp/aem-forms/6-3/data-integration.html)を参照してください。

SFDC でリードオブジェクトを作成する POST サービスを含めるようにフォームデータモデルを必ず設定してください。

また、リードオブジェクトの読み取りサービスと書き込みサービスも設定する必要があります。このページの下部にあるスクリーンショットを参照してください。

フォームデータモデルを作成したら、このモデルに基づいてアダプティブフォームを作成し、フォームデータモデルの送信方法を使用して SFDC にリードを作成できます。

## AEM Forms 6.4 {#aem-forms-1}

* データソースの作成

   * [データソースに移動します](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)。

   * 「作成」ボタンをクリックします。
   * 意味のある値を指定します。

      * 名前：CreateLeadInSalesForce
      * タイトル：CreateLeadInSalesForce
      * サービスタイプ：RESTful サービス

   * 「次へ」をクリックします。
   * Swagger ソース：ファイル
   * 前の手順でダウンロードした Swagger ファイルを参照して選択します。
   * 認証タイプ：OAuth 2.0。次の値を指定します。
   * 「クライアント ID」と「クライアント秘密鍵」の値を指定します。
   * OAuth URL：**https://login.salesforce.com/services/oauth2/authorize**
   * 更新トークン URL：**https://na5.salesforce.com/services/oauth2/token**
   * アクセストークン URL：**https://na5.salesforce.com/services/oauth2/token**
   * 認証範囲：** api chatter_api full id openid refresh_token visualforce web**
   * 認証ハンドラ：認証ベアラー
   * 「OAuth に接続」ボタンをクリックします。エラーが発生した場合は、上記の手順ですべての情報が正しく入力されていることを確認してください。

SalesForce を使用するデータソースを作成したら、作成したデータソースを使用してフォームデータ統合を作成できます。これに関するドキュメントリンクについては、[こちら](https://helpx.adobe.com/jp/experience-manager/6-4/forms/using/create-form-data-models.html)を参照してください。

SFDC でリードオブジェクトを作成する POST サービスを含めるようにフォームデータモデルを必ず設定してください。

また、リードオブジェクトの読み取りサービスと書き込みサービスも設定する必要があります。このページの下部にあるスクリーンショットを参照してください。

フォームデータモデルを作成したら、このモデルに基づいてアダプティブフォームを作成し、フォームデータモデルの送信方法を使用して SFDC にリードを作成できます。

>[!NOTE]
>
>Swagger ファイル内の URL が自分の地域に対応していることを確認します。例えば、サンプルの Swagger ファイルの URL は、アカウントが北米で作成されたので、「na46.salesforce.com」です。最も簡単な方法は、Salesforce アカウントにログインして URL を確認することです。

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
