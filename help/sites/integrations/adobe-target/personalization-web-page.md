---
title: 完全な web ページエクスペリエンスのパーソナライゼーション
description: Adobe Target を使用して AEM web サイトのページを新しいページにリダイレクトする Target アクティビティの作成方法を説明します。
jira: KT-6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2d201b48-c0fb-4bb4-a7d8-da9f4702e9ff
duration: 124
source-git-commit: 970093bb54046fee49e2ac209f1588e70582ab67
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 98%

---

# 完全な web ページエクスペリエンスのパーソナライゼーション {#personalization-fpe}

Adobe Target を使用して、AEMでホストされているサイトのページを新しいページにリダイレクトするアクティビティの作成方法を説明します。

## 前提条件

AEM web サイトのすべてのページをパーソナライズするには、次の設定を行う必要があります。

1. [AEM web サイトへの Adobe Target の追加](./add-target-launch-extension.md)
1. [トリガーとAdobe Targetの呼び出し](./load-and-fire-target.md)

## シナリオの概要

WKND サイトはホームページのデザインを一新し、現在のホームページの訪問者を新しいホームページにリダイレクトしたいと考えています。 同時に、再設計されたホームページがユーザーエンゲージメントと収益の改善にどのように役立つかも把握する必要があります。 マーケターは、訪問者を新しいホームページにリダイレクトするアクティビティを作成するタスクを割り当てられています。 WKND サイトのホームページを参照し、Adobe Target を使用してアクティビティを作成する方法を確認していきましょう。

## Visual Experience Composer（VEC）を使用した A/B テストの作成手順

1. Adobe Target にログインし、「アクティビティ」タブに移動します。
1. 「**アクティビティを作成**」ボタンをクリックし、「**A/B テスト**」アクティビティを選択します

   ![A/B アクティビティ](assets/ab-target-activity.png)

1. 「**Visual Experience Composer**」オプションを選択し、アクティビティ URL を指定して「**次へ**」をクリックします。

   ![アクティビティ URL](assets/ab-test-url.png)

1. Visual Experience Composer では、アクティビティを作成した後、左側に「*エクスペリエンス A*」および 「*エクスペリエンス B*」の 2 つのタブが表示されます。リストからエクスペリエンスを選択します。 新しいエクスペリエンスをリストに追加するには、「**エクスペリエンスを追加**」ボタンをクリックします。

   ![エクスペリエンスオプション](assets/experience-options.png)

1. エクスペリエンス A で使用可能なオプションを表示して「**URL にリダイレクト**」オプションを選択し、新しい WKND サイトホームページの URL を指定します。

   ![リダイレクト URL](assets/redirect-url.png)

1. 名前を変更 *エクスペリエンス A* から *新しい WKND ホームページ* および *エクスペリエンス B* から *WKND ホームページ*

   ![Adventures](assets/new-experiences.png)

1. 「**次へ**」をクリックしてターゲティングに移動し、両方のエクスペリエンスで手動のトラフィック配分を 50 に設定します。

   ![ターゲティング](assets/targeting.png)

1. 「目標と設定」では、「レポートソース」として「Adobe Target」を選択し、「目標指標」として「ページビューアクションによるコンバージョン」を選択します。

   ![目標](assets/goals.png)

1. アクティビティの名前を指定して保存します。
1. 保存したアクティビティをアクティブ化して、変更をライブにプッシュします。

   ![ゴール](assets/activate.png)

1. 新しいタブでサイトページ（手順 3 のアクティビティ URL）を開くと、A/B テストアクティビティからいずれかのエクスペリエンス（WKND ホームページまたは新しい WKND ホームページ）を表示できるはずです。 `us/en.html` は `us/home.html` にリダイレクトされます。

   ![ゴール](assets/redirect-test.png)

## 概要

マーケターが Adobe Target を使用して、AEM でホストされるサイトページを新しいページにリダイレクトするアクティビティを作成することができました。

## サポートリンク

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
