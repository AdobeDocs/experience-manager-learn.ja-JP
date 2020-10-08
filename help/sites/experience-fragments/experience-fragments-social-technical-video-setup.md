---
title: AEMエクスペリエンスフラグメントを使用したソーシャル投稿の設定
description: エクスペリエンスフラグメントを使用すると、マーケターはAEMで作成したエクスペリエンスをソーシャルメディアプラットフォームに投稿できます。 以下のビデオでは、エクスペリエンスフラグメントをFacebookやPinterestに公開するために必要な設定と設定について詳しく説明しています。
sub-product: サイト、content services
feature: experience-fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---


# エクスペリエンスフラグメントを使用したソーシャル投稿の設定 {#set-up-social-posting-with-experience-fragments}

エクスペリエンスフラグメントを使用すると、マーケターはAEMで作成したエクスペリエンスをソーシャルメディアプラットフォームに投稿できます。 以下のビデオでは、エクスペリエンスフラグメントをFacebookやPinterestに公開するために必要な設定と設定について詳しく説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[エクスペリエンスフラグメント]- FacebookやPinterestへのソーシャル投稿のセットアップと設定*

## FacebookおよびPinterestに投稿するエクスペリエンスフラグメントの設定のチェックリスト

1. AEM作成者インスタンスがHTTPSで実行されている
2. Facebookアカウント+ Facebook開発者アプリ
3. Pinterestアカウント+ Pinterest開発版アプリ
4. [!UICONTROL AEMクラウドサービス] - Facebook
5. [!UICONTROL AEMクラウドサービス] - Pinterest
6. Facebook + PinterestのCloud Servicesを含むAEMエクスペリエンスフラグメント
7. Facebookテンプレートを使用したエクスペリエンスフラグメントのバリエーション
8. Pinterestテンプレートを使用したエクスペリエンスフラグメントのバリエーション

## エクスペリエンスフラグメントリダイレクトURI

このURIは、Oauthフローの一部としてFacebookアプリとPinterestアプリに使用されます。

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

