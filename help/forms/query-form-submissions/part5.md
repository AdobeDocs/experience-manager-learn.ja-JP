---
title: サンプルのローカルサーバーへのデプロイ
description: Azure ポータルに保存されたフォーム送信のクエリに関係する手順を説明するマルチパートチュートリアル
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 44841a3c-85e0-447f-85e2-169a451d9c68
duration: 20
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 100%

---

# サンプルのローカルサーバーへのデプロイ

このユースケースをローカルサーバーで動作させるには、以下の手順に従ってください。この手順では、AEM インスタンスが localhost（4502 ポート）で実行されていると想定しています。

* パッケージマネージャーを使って[パッケージをインストール](assets/azuredemo.all-1.0.0-SNAPSHOT.zip)します。

* OSGi configMgr を使用して Azure ポータルの資格情報を指定
  ![azure-portal](assets/azure-portal-config.png)
ストレージ URI がスラッシュで終わり、SAS トークンが ? で始まることを確認します。
*  [AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo) に移動

* 次の 3 つのデータソースの認証設定を環境に合わせて編集
  ![data-sources](assets/fdm-data-sources.png)

* [ContactUs フォーム](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled)をプレビューして送信

* [フォーム送信のクエリ](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)
