---
title: AEM Forms as a Cloud Service と Marketo の統合（第 2 部）
description: AEM Forms フォームデータモデルを使用して AEM Forms と Marketo を統合する方法を説明します
feature: Form Data Model,Integration
version: Experience Manager as a Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: 75e589fa-f7fc-4d0b-98c8-ce4d603ef2f7
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '220'
ht-degree: 100%

---

# データソースの作成

Marketo の REST API は 2-Legged OAuth 2.0 で認証されます。前の手順でダウンロードした Swagger ファイルを使用して、データソースを簡単に作成できます。

## 設定コンテナを作成

* AEM へログインします。
* 次に示すように、ツールメニューをクリックし、**設定ブラウザー**&#x200B;をクリックします。

* ![ツールメニュー](assets/datasource3.png)

* 「**作成**」をクリックし、次に示すように、意味のある名前をつけます。以下に示すように、必ず「クラウド設定」オプションを選択してください。

* ![設定コンテナ](assets/datasource4.png)

## クラウドサービスの作成

* ツールメニューに移動し、クラウドサービス／データソースをクリックします。

* ![クラウドサービス](assets/datasource5.png)

* 前の手順で作成した設定コンテナを選択し、「**作成**」をクリックして新しいデータソースを作成します。意味のある名前を入力し、サービスタイプドロップダウンリストから RESTful サービスを選択して、「**次へ**」をクリックします。
* ![new-data-source](assets/datasource6.png)

* Swagger ファイルをアップロードし、以下のスクリーンショットに示すように、Marketo インスタンスに固有の付与タイプ、クライアント ID、クライアント秘密鍵、アクセストークン URL を指定します。

* 接続をテストし、接続が成功した場合は、青色の「**作成**」ボタンをクリックして、データソースの作成プロセスを完了します。

* ![data-source-config](assets/datasource1.png)


## 次の手順

[フォームデータモデルの作成](./part3.md)
