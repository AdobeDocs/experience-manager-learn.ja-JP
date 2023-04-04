---
title: AEM Assetsを使用した画像のスマートタグ
description: 画像のスマートタグは、画像のコンテンツに基づいて、メタデータタグを画像アセットに自動的かつインテリジェントに追加することで、AEMの検索機能を強化します。
topic: Content Management
feature: Smart Tags
role: User
level: Intermediate
version: 6.4, 6.5, Cloud Service
kt: 645
thumbnail: 17019.jpg
last-substantial-update: 2022-06-09T00:00:00Z
exl-id: c72dc489-70e6-48ca-99a8-663d4c0652ba
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 6%

---

# 画像のスマートタグ

画像用の AEM Assets のスマートタグは、画像アセットに派生メタデータタグを自動的に追加することで AEM Assets の検索を強化し、適切な画像を見つけやすく、迅速にオーサリングできるようにし、オーサリングエクスペリエンスを向上させます。

>[!VIDEO](https://video.tv.adobe.com/v/17019?quality=12&learn=on)

## AEM 6.x のセットアップ{#set-up}

>[!NOTE]
> 画像用のスマートタグは、AEM as a Cloud Service用に自動的にプロビジョニングされます。

>[!VIDEO](https://video.tv.adobe.com/v/17023?quality=12&learn=on)

スマートコンテンツサービスを使用する前に、以下の点を確認して、統合のAdobe I/Oを作成します。

* 組織の管理者権限を持つAdobe IDアカウント
* 組織でスマートコンテンツサービスが有効になっている

このビデオでは、スマートタグ画像に使用するAdobe I/Oスマートコンテンツサービスを設定するために必要な次のタスクについて詳しく説明します。

* 公開鍵を生成するスマートコンテンツサービス設定をAEMで作成します。 OAuth 統合用の公開証明書を取得します。
* 統合を「Adobe I/O」で作成し、生成した公開鍵をアップロードします。
* API キーやAEMのその他の資格情報を使用して、API インスタンスをAdobe I/Oします。
* （オプション）アセットアップロード時の自動タグ付けを有効化します。

## その他のリソース

* [AEM Assetsスマートタグドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/manage/smart-tags.html)
