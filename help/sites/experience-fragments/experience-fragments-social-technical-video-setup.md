---
title: AEMエクスペリエンスフラグメントを使用したソーシャル投稿の設定
description: エクスペリエンスフラグメントを使用すると、マーケターはAEMで作成したエクスペリエンスをソーシャルメディアプラットフォームに投稿できます。 以下のビデオでは、エクスペリエンスフラグメントをFacebookとPinterestに公開するために必要な設定と設定について詳しく説明しています。
sub-product: サイト、content services
feature: Experience Fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
topic: Content Management
role: Administrator, Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 2%

---


# エクスペリエンスフラグメントを使用したソーシャル投稿の設定{#set-up-social-posting-with-experience-fragments}

エクスペリエンスフラグメントを使用すると、マーケターはAEMで作成したエクスペリエンスをソーシャルメディアプラットフォームに投稿できます。 以下のビデオでは、エクスペリエンスフラグメントをFacebookとPinterestに公開するために必要な設定と設定について詳しく説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[エクスペリエンスフラグメント] -FacebookとPinterestへのソーシャル投稿のセットアップと設定*

## facebookおよびPinterestに投稿するエクスペリエンスフラグメントの設定のチェックリスト

1. AEM作成者インスタンスがHTTPSで実行されている
2. Facebookアカウント+Facebook開発版App
3. Pinterestアカウント+Pinterest開発版App
4. [!UICONTROL AEM Cloud ] Services設定 —Facebook
5. [!UICONTROL AEM Cloud ] Services設定 —Pinterest
6. FACEBOOK+PinterestのCloud Servicesを含むAEMエクスペリエンスフラグメント
7. facebookテンプレートを使用したエクスペリエンスフラグメントのバリエーション
8. pinterestテンプレートを使用したエクスペリエンスフラグメントのバリエーション

## エクスペリエンスフラグメントリダイレクトURI

このURIは、Oauthフローの一部として、FacebookおよびPinterestのアプリに使用されます。

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

