---
title: Adobe Experience Platform のタグと AEM の統合
description: Experience Platform Data Collection Tags は、アドビの次世代タグ管理ソリューションであり、Adobe Analytics、Target、Audience Manager、その他多くのソリューションをデプロイするための最適な方法です。ここでは、Adobe Experience Platform のタグの概要と、Adobe Experience Manager との推奨される統合について説明します。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5979
thumbnail: 39090.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
duration: 230
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 100%

---

# Experience Platform Data Collection Tags と AEM の統合 {#overview}

Adobe Experience Platform のタグと Adobe Experience Manager を統合する方法について説明します。

Tags は Adobe Experience Platform の次世代タグ管理テクノロジーです。Tags を使用すると、Adobe Analytics、Target、Audience Manager、その他多くのソリューションを簡単にデプロイできるようになります。ここでは、Tags の概要と Adobe Experience Manager との推奨される統合について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)

## 前提条件

Experience Platform Data Collection Tags を統合する場合は、以下が必要です。

+ AEM as a Cloud Service 環境への AEM 管理者アクセス
+ 上記の環境にデプロイされたリファレンスサイト（[WKND](https://github.com/adobe/aem-guides-wknd) など）。
+ Adobe Experience Platform Data Collection ソリューションへのアクセス
+ [Adobe Developer Console](https://developer.adobe.com/developer-console/) へのシステム管理者アクセス


## 手順の概要

+ Adobe Experience Platform Data Collection でタグプロパティを作成し、それを編集して&#x200B;_ルールを追加_&#x200B;します。次に、_ライブラリを追加_&#x200B;し、新しく追加されたルールを選択し、承認して公開します。
+ 既存（または新規）の IMS 設定を使用して AEM と Tags を接続します。
+ AEM で、タグクラウドサービス設定を作成して既存のサイトに適用し、最後に、タグプロパティとそのライブラリが公開済みサイトまたはオーサーサイトに読み込まれていることを確認します。

## 次の手順

[タグプロパティの作成](create-tag-property.md)

## その他のリソース {#additional-resources}

+ [Experience Platform アプリケーションと Experience Cloud の統合](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html?lang=ja)
+ [Tags の概要](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ja)
+ [Tags を使用した web サイトへの Experience Cloud の実装](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html?lang=ja)
