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
workflow-type: ht
source-wordcount: '211'
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
