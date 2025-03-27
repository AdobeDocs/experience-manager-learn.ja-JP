---
title: クラウドサービス設定の作成
description: OAuth 資格情報を使用して Salesforce に接続するデータソースの作成
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms, Integrations
jira: KT-7148
thumbnail: 331755.jpg
exl-id: e2d56e91-c13e-4787-a97f-255938b5d290
duration: 173
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '75'
ht-degree: 100%

---

# データソースの作成

前の手順で作成した Swagger ファイルを使用して、REST ベースのデータソースを作成します。

>[!VIDEO](https://video.tv.adobe.com/v/331755?quality=12&learn=on)

| 設定 | 値 |
|---------------------|-----------------------------------------------------------------|
| OAuth URL | https://login.salesforce.com/services/oauth2/authorize |
| 認証範囲 | api chatter_api full id openid refresh_token visualforce web |
| 更新トークン URL | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |
| トークン URL にアクセス | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |


**更新およびアクセストークンの URL のドメイン名は、Salesforce アカウントの設定に合わせて変更する必要があります。**