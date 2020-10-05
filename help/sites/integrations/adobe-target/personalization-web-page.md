---
title: 完全なWebページエクスペリエンスのパーソナライズ
description: AEM上でホストされているサイトページを、Adobe Targetを使用して新しいページにリダイレクトするアクティビティを作成する方法を説明します。
feature: targeting
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 1%

---


# 完全なWebページエクスペリエンスのパーソナライズ {#personalization-fpe}

AEM上でホストされているサイトページを、Adobe Targetを使用して新しいページにリダイレクトするアクティビティを作成する方法を説明します。

## シナリオの概要

WKNDサイトはホームページを再設計し、現在のホームページ訪問者を新しいホームページにリダイレクトしたいと考えています。 同時に、再設計されたホームページがユーザーの関与と売上高を改善する方法についても理解します。 マーケティング担当者には、訪問者を新しいホームページにリダイレクトするアクティビティを作成するタスクが割り当てられています。 WKNDのサイトホームページを調べ、Adobe Targetを使用したアクティビティの作成方法を学びましょう。

## Visual Experience Composer(VEC)を使用したA/Bテストの作成手順

1. Adobe Targetにログインし、「アクティビティ」タブに移動します
1. 「 **アクティビティを作成** 」ボタンをクリックし、「 **A/Bテスト** アクティビティ」を選択します

   ![A/Bアクティビティ](assets/ab-target-activity.png)

1. 「 **Visual Experience Composer** 」オプションを選択し、アクティビティURLを指定して、「 **次へ」をクリックします**

   ![アクティビティURL](assets/ab-test-url.png)

1. 新しいアクティビティを作成すると、Visual Experience Composerの左側に2つのタブが表示されます。 *エクスペリエンスA* と *エクスペリエンスB*。 リストからエクスペリエンスを選択します。 「エクスペリエンス **」ボタンを使用して、リストに新しいエクスペリエンスを追加でき** ます。

   ![エクスペリエンスオプション](assets/experience-options.png)

1. エクスペリエンスAで使用できる表示オプションを選択し、「URLに **リダイレクト** 」オプションを選択して、新しいWKNDサイトホームページのURLを指定します。

   ![リダイレクトURL](assets/redirect-url.png)

1. エ *クスペリエンスAの* 名前を新しいWKNDホームページ *、* エクスペリエンスBの名前を ***WKNDホームページに変更*

   ![冒険](assets/new-experiences.png)

1. 「 **次へ** 」をクリックして「ターゲット」に移動し、2つのエクスペリエンス間で手動のトラフィック配分を50 ～ 50に維持します。

   ![ターゲット設定](assets/targeting.png)

1. 「目標と設定」で、レポートソースをAdobe Targetとして選択し、目標指標をページ表示アクションを使用したコンバージョンとして選択します。

   ![ゴール](assets/goals.png)

1. アクティビティの名前を入力し、「保存」をクリックします。
1. 保存したアクティビティをアクティブ化して、変更をライブにプッシュします。

   ![ゴール](assets/activate.png)

1. サイトページ(手順3のアクティビティURL)を新しいタブで開き、A/Bテストアクティビティからエクスペリエンス(WKNDホームページまたは新しいWKNDホームページ)のいずれかを表示できるようにします。 `us/en.html` リダイレクト先 `us/home.html`:

   ![ゴール](assets/redirect-test.png)

## 概要

マーケティング担当者は、AEMでホストされるサイトページを、Adobe Targetを使用して新しいページにリダイレクトするアクティビティを作成できました。

## サポートリンク

* [Adobe Experience Cloudデバッガー — Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloudデバッガ — Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)

