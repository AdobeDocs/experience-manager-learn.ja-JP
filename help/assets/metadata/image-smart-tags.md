---
title: AEM Assets を使用した画像のスマートタグ
description: 画像のスマートタグは、画像のコンテンツに基づいて、メタデータタグを画像アセットに自動的かつインテリジェントに追加することで、AEM の検索機能を強化します。
topic: Content Management
feature: Smart Tags
role: User
level: Intermediate
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
jira: KT-645
thumbnail: 17019.jpg
last-substantial-update: 2022-06-09T00:00:00Z
doc-type: Feature Video
exl-id: c72dc489-70e6-48ca-99a8-663d4c0652ba
duration: 574
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '212'
ht-degree: 100%

---

# 画像のスマートタグ付け

画像用の AEM Assets のスマートタグは、画像アセットに派生メタデータタグを自動的に追加することで AEM Assets の検索を強化し、適切な画像を見つけやすく、迅速にオーサリングできるようにし、オーサリングエクスペリエンスを向上させます。

>[!VIDEO](https://video.tv.adobe.com/v/3444055?quality=12&learn=on&captions=jpn)

## AEM 6.x の設定{#set-up}

>[!NOTE]
> 画像用のスマートタグは、AEM as a Cloud Service 用に自動的にプロビジョニングされます。

>[!VIDEO](https://video.tv.adobe.com/v/3444166?quality=12&learn=on&captions=jpn)

Adobe I/O で統合を作成してスマートコンテンツサービスを使用する前に、以下の事項を確認します。

* 組織の管理者権限を持つ Adobe ID アカウント。
* 組織でスマートコンテンツサービスが有効化されていること。

このビデオでは、スマートタグ画像に使用する Adobe I/O スマートコンテンツサービスを設定するために必要な次のタスクについて詳しく説明します。

* AEM でスマートコンテンツサービス設定を作成して、公開鍵を生成します。OAuth 統合用の公開証明書を取得します。
* Adobe I/O で統合を作成し、生成した公開鍵をアップロードします。
* API キーやその他の Adobe I/O の資格情報を使用して AEM インスタンスを設定します。
* （オプション）アセットアップロード時の自動タグ付けを有効化します。

## その他のリソース

* [AEM Assets のスマートタグドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/smart-tags.html?lang=ja)
