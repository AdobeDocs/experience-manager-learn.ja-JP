---
title: との統合 [!DNL ServiceNow]
description: フォームデータモデルを使用して、すべてのインシデントを作成および表示します。
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
source-git-commit: 81a15fb0182760aaac8cb58cccbfe28de7323492
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 3%

---

# AEM Formsとの統合 [!DNL ServiceNow]

でインシデントを作成して表示します [!DNL ServiceNow] AEM Formsでフォームデータモデルを使用する。

## 前提条件

* [!DNL ServiceNow] アカウント
* 詳しい [データソースの作成](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* 詳しい [フォームデータモデル](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## サンプルアセット

この記事で提供されるサンプルアセットには、次のものが含まれます
* クラウドサービスの設定
* Swagger ファイルを使用してインシデントを作成し、すべてのインシデントを取得します
* Swagger ファイルに基づくフォームデータモデル
* 作成およびリストを作成するアダプティブフォーム [!DNL ServiceNow] インシデント

## サーバーにアセットをデプロイする

* をダウンロードします。 [サンプルアセット](assets/service-now.zip)
* を使用してアセットをAEMに読み込む [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)
* を編集します。 [CreateIncident クラウドサービス設定](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)を ServiceNow インスタンスに一致させます。
* を編集します。 [GetAllIncidents クラウドサービス設定](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) ServiceNow インスタンスに一致させる

## 統合のテスト

* [アダプティブフォームを開く](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* [ 説明とコメント ] フィールドに値を入力し、[ インシデントの作成 ] ボタンをクリックします
* 新しく作成されたインシデントのインシデント ID がテキストフィールドに入力され、以下の表にすべてのインシデントが一覧表示されます。
