---
title: ' [!DNL ServiceNow] との統合'
description: フォームデータモデルを使用して、すべてのインシデントを作成および表示します。
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
duration: 47
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 100%

---

# AEM Forms と [!DNL ServiceNow] の統合

AEM Forms のフォームデータモデルを使用して、[!DNL ServiceNow] でインシデントを作成および表示します。

## 前提条件

* [!DNL ServiceNow] アカウント。
* [データソースの作成](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=ja)に精通している
* [フォームデータモデル](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=ja)に精通している

## サンプルアセット

この記事で提供されるサンプルアセットには、次のものが含まれます。

* クラウドサービス設定
* インシデントの作成とすべてのインシデントの取得に使用する Swagger ファイル
* Swagger ファイルに基づくフォームデータモデル
* [!DNL ServiceNow] インシデントの作成とリストに使用するアダプティブフォーム

## サーバーにアセットをデプロイ

* [サンプルアセット](assets/service-now.zip)をダウンロードします。
* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用してアセットを AEM に読み込みます。
* この統合に使用される Swagger ファイルは、crx リポジトリ内の ```/conf/9957/settings/cloudconfigs/fdm``` フォルダーにあります。
* ServiceNow インスタンスと一致するように [CreateIncident クラウドサービス設定](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)を編集します。
* ServiceNow インスタンスと一致するように [GetAllIncidents クラウドサービス設定](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents)を編集します。 ServiceNow インスタンスの資格情報と一致するように、ホスト、ユーザー名、パスワードを変更する必要があります。

## ServiceNow インスタンスの資格情報にアクセス

* ユーザープロファイルをクリックします。
  ![ユーザープロファイルをクリックします](assets/snow-1.png)

* 「インスタンスのパスワードの管理」をクリックします。
* インスタンスの詳細が次のように表示されます。
  ![インスタンスの詳細](assets/snow-3.png)

## 統合のテスト

* [アダプティブフォームを開きます](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* 「説明とコメント」フィールドに値を入力し、「インシデントの作成」ボタンをクリックします。
* 新しく作成されたインシデントのインシデント ID がテキストフィールドに入力され、以下の表にすべてのインシデントが一覧表示されます。
