---
title: AEM Forms Cloud ServiceとMarketoの統合（パート 2）
description: AEM Forms フォームデータモデルを使用して AEM Forms と Marketo を統合する方法を説明します
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: 75e589fa-f7fc-4d0b-98c8-ce4d603ef2f7
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 13%

---

# データソースの作成

Marketoの REST API は、2-legged OAuth 2.0 で認証されます。前の手順でダウンロードした Swagger ファイルを使用して、データソースを簡単に作成できます

## 設定コンテナの作成

* AEMにログインします。
* 次に示すように、「ツール」メニューをクリックし、**設定ブラウザー** をクリックします

* ![ ツールメニュー ](assets/datasource3.png)

* 「**作成**」をクリックし、次に示すように、意味のある名前を指定します。 必ず「クラウド設定」オプションを選択してください（下図を参照）

* ![ 設定コンテナ ](assets/datasource4.png)

## クラウドサービスの作成

* ツール メニューに移動し、クラウドサービス / データソースをクリックします。

* ![ クラウドサービス ](assets/datasource5.png)

* 前の手順で作成した設定コンテナを選択し、「**作成**」をクリックして新しいデータソースを作成します。意味のある名前を指定し、「サービスタイプ」ドロップダウンリストから「RESTful サービス」を選択して、「次へ **をクリックし** す。
* ![new-data-source](assets/datasource6.png)

* Swagger ファイルをアップロードし、以下のスクリーンショットに示すように、Marketo インスタンスに固有の付与タイプ、クライアント ID、クライアントシークレット、アクセストークン URL を指定します。

* 接続をテストし、接続が成功したら、青い **作成** ボタンをクリックして、データソースの作成プロセスを完了します。

* ![data-source-config](assets/datasource1.png)


## 次の手順

[フォームデータモデルの作成](./part3.md)
