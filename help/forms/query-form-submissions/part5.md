---
title: ローカルサーバーにサンプルをデプロイします。
description: Azure ポータルに保存されたフォーム送信のクエリに関する手順を説明するマルチパートチュートリアル
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# ローカルサーバーにサンプルをデプロイします。

このユースケースをローカルサーバーで動作させるには、以下の手順に従ってください。AEMインスタンスが localhost（4502 ポート）で実行されていると想定されます。

* [パッケージのインストール](assets/azuredemo.all-1.0.0-SNAPSHOT.zip) パッケージマネージャーを使用します。

* OSGi configMgr を使用して Azure ポータルの資格情報を指定します
  ![azure-portal](assets/azure-portal-config.png)
ストレージ URI がスラッシュで終わり、SAS トークンが？で始まることを確認します。
* に移動します。 [AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo)

* 環境に合わせて次の 3 つのデータソースの認証設定を編集します
  ![data-sources](assets/fdm-data-sources.png)

* プレビューして送信 [ContactUs フォーム](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled)

* [フォーム送信のクエリ](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)

