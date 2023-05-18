---
title: Experience Platformデータ収集タグ (Launch) とAEMの統合
description: Experience Platformデータ収集のタグは、Adobeの次世代タグ管理ソリューションであり、Adobe Analytics、Target、Audience Managerなどの多くのソリューションを導入する最善の方法です。 タグ（旧称 Launch）の概要と、Adobe Experience Managerとの推奨される統合について説明します。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5979
thumbnail: 39090.jpg
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 8%

---

# Experience Platformデータ収集タグとAEMの統合 {#overview}

Experience Platform _データ収集タグ_ （以前の Launch）をAdobe Experience Managerと共に使用します。

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 次を参照してください。 [文書](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) 用語の変更を統合的に参照する場合。


タグは、Adobe Experience Platformが提供する次世代のタグ管理テクノロジーです。 タグは、Adobe Analytics、Target、Audience Manager、その他多くのソリューションをデプロイする最も簡単な方法を提供します。 タグの概要と、Adobe Experience Managerとの推奨される統合について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)


## 前提条件

データ収集タグを統合する場合は、次のExperience Platformが必要です。

+ AEM as a Cloud Service環境へのAEM管理者アクセス
+ のようなリファレンスサイト [WKND](https://github.com/adobe/aem-guides-wknd) にデプロイされました。
+ Adobe Experience Platform Data Collection Solution へのアクセス
+ へのシステム管理者のアクセス権 [Adobe Developer Console](https://developer.adobe.com/developer-console/)


## 概要レベルの手順

+ Adobe Experience Platform Data Collection で、タグプロパティを作成し、次のように編集します。 _ルールを追加_. 次に、 _ライブラリを追加_、新しく追加されたルールを選択し、承認して公開します。
+ 既存（または新規）の IMS 設定を使用したAEMとタグの接続
+ AEMで、Launch クラウドサービス設定を作成し、既存のサイトに適用して、最後にタグプロパティとそのライブラリが公開済みサイトまたは作成者サイトに読み込まれていることを確認します。

## 次の手順

[タグプロパティの作成](create-tag-property.md)

## その他のリソース {#additional-resources}

+ [Experience Platform アプリケーションと Experience Cloud の統合](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html?lang=ja)
+ [タグの概要](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ja)
+ [タグ付き Web サイトでのExperience Cloudの実装](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
