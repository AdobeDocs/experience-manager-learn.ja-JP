---
title: AEM Forms と Marketo の連携（第 2 部）
description: AEM Forms フォームデータモデルを使用した AEM Forms と Marketo の統合に関するチュートリアル
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: f8ba3d5c-0b9f-4eb7-8609-3e540341d5c2
duration: 137
source-git-commit: 7e0d7e87d72aa1e4450649afa6a962099ceb2db4
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 15%

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
