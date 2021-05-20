---
title: AEMエクスペリエンスフラグメントを使用したソーシャル投稿の設定
description: エクスペリエンスフラグメントを使用すると、マーケターはAEMで作成したエクスペリエンスをソーシャルメディアプラットフォームに投稿できます。 次のビデオでは、エクスペリエンスフラグメントをFacebookとPinterestに公開するために必要な設定と設定について詳しく説明します。
sub-product: サイト、コンテンツサービス
feature: エクスペリエンスフラグメント
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
topic: コンテンツ管理
role: Administrator, Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 2%

---


# エクスペリエンスフラグメントを使用したソーシャル投稿の設定{#set-up-social-posting-with-experience-fragments}

エクスペリエンスフラグメントを使用すると、マーケターはAEMで作成したエクスペリエンスをソーシャルメディアプラットフォームに投稿できます。 次のビデオでは、エクスペリエンスフラグメントをFacebookとPinterestに公開するために必要な設定と設定について詳しく説明します。

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[エクスペリエンスフラグメント]  - FacebookおよびPinterestへのソーシャル投稿のセットアップと設定*

## facebookおよびPinterestに投稿するエクスペリエンスフラグメントの設定のチェックリスト

1. AEMオーサーインスタンスがHTTPSで動作している
2. Facebookアカウント+ Facebook Developer App
3. Pinterestアカウント+ Pinterest Developer App
4. [!UICONTROL AEM Cloud ] ServicesConfiguration - Facebook
5. [!UICONTROL AEM Cloud ] ServicesConfiguration - Pinterest
6. FACEBOOK + Pinterest用のCloud Servicesを含むAEMエクスペリエンスフラグメント
7. facebookテンプレートを使用したエクスペリエンスフラグメントのバリエーション
8. pinterestテンプレートを使用したエクスペリエンスフラグメントのバリエーション

## エクスペリエンスフラグメントのリダイレクトURI

このURIは、OAuthフローの一部としてFacebookおよびPinterestアプリで使用されます。

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

