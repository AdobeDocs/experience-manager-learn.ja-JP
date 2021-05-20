---
title: 完全なWebページエクスペリエンスのパーソナライズ
description: Adobe Targetを使用してAEM Webサイトのページを新しいページにリダイレクトするTargetアクティビティの作成方法を説明します。
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
topic: 統合
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 1%

---


# 完全なWebページエクスペリエンスのパーソナライズ{#personalization-fpe}

Adobe Targetを使用して、AEM上でホストされているサイトページを新しいページにリダイレクトするアクティビティの作成方法を説明します。

## 前提条件

AEM Webサイトのすべてのページをパーソナライズするには、次の設定を行う必要があります。

1. [AEM WebサイトへのAdobe Targetの追加](./add-target-launch-extension.md)
1. [LaunchからのAdobe Target呼び出しのトリガー](./load-and-fire-target.md)

## シナリオの概要

WKNDサイトのデザインが変更され、現在のホームページの訪問者を新しいホームページにリダイレクトしたいと考えています。 同時に、再設計されたホームページがユーザーエンゲージメントと売上高を向上させる方法も理解します。 マーケターには、訪問者を新しいホームページにリダイレクトするアクティビティを作成するタスクが割り当てられています。 WKNDサイトのホームページを参照し、Adobe Targetを使用してアクティビティを作成する方法を学びます。

## Visual Experience Composer(VEC)を使用したA/Bテストの作成手順

1. Adobe Targetにログインし、「アクティビティ」タブに移動します。
1. 「**アクティビティを作成**」ボタンをクリックし、「**A/Bテスト**」アクティビティを選択します

   ![A/Bアクティビティ](assets/ab-target-activity.png)

1. 「**Visual Experience Composer**」オプションを選択し、アクティビティURLを指定して、「**次へ**」をクリックします

   ![アクティビティURL](assets/ab-test-url.png)

1. Visual Experience Composerでは、新しいアクティビティを作成した後、左側に2つのタブが表示されます。*エクスペリエンスA*&#x200B;と&#x200B;*エクスペリエンスB*。 リストからエクスペリエンスを選択します。 「**エクスペリエンスを追加**」ボタンを使用して、新しいエクスペリエンスをリストに追加できます。

   ![エクスペリエンスオプション](assets/experience-options.png)

1. エクスペリエンスAで使用可能なオプションを表示し、「URLにリダイレクト&#x200B;**」オプションを選択して、新しいWKNDサイトのホームページのURLを指定します。**

   ![リダイレクトURL](assets/redirect-url.png)

1. *エクスペリエンスA*&#x200B;を&#x200B;*新しいWKNDホームページ*&#x200B;および&#x200B;*エクスペリエンスB*&#x200B;を&#x200B;*WKNDホームページ*&#x200B;に変更します。

   ![冒険](assets/new-experiences.png)

1. 「**次へ**」をクリックして「ターゲティング」に移動し、2つのエクスペリエンス間の手動のトラフィック配分は50 ～ 50に保ちます。

   ![ターゲット設定](assets/targeting.png)

1. 目標と設定については、「レポートソース」として「 Adobe Target 」を選択し、「目標指標」として「ページビューアクションによるコンバージョン」を選択します。

   ![ゴール](assets/goals.png)

1. アクティビティの名前を指定し、保存します。
1. 保存したアクティビティをアクティブ化して、変更をライブにプッシュします。

   ![ゴール](assets/activate.png)

1. 新しいタブでサイトページ（手順3のアクティビティURL）を開き、A/Bテストアクティビティからエクスペリエンス（WKNDホームページまたは新しいWKNDホームページ）を表示できるようにします。 `us/en.html` にリダイレクトしま `us/home.html`す。

   ![ゴール](assets/redirect-test.png)

## 概要

マーケターは、Adobe Targetを使用して、AEM上でホストされるサイトページを新しいページにリダイレクトするアクティビティを作成できました。

## サポートリンク

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)

