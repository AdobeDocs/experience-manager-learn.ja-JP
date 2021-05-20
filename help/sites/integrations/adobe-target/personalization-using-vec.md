---
title: Visual Experience Composerを使用したパーソナライゼーション
description: Visual Experience Composerを使用してAdobe Targetアクティビティを作成する方法を説明します。
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: 統合
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '519'
ht-degree: 2%

---


# Visual Experience Composer {#personalization-vec}を使用したパーソナライゼーション

Visual Experience Composer(VEC)を使用してA/BテストTargetアクティビティを作成する方法を説明します。

## 前提条件

AEM WebサイトでVECを使用するには、次の設定を行う必要があります。

1. [AEM WebサイトへのAdobe Targetの追加](./add-target-launch-extension.md)
1. [LaunchからのAdobe Target呼び出しのトリガー](./load-and-fire-target.md)

## シナリオの概要

WKNDサイトのホームページには、ローカルアクティビティまたは情報カードの形式で都市の周りを回避する最善の方法が表示されます。 マーケターは、アドベンチャーセクションのティーザーにテキストを変更し、コンバージョンの改善方法を理解することで、ホームページを変更するタスクを割り当てられています。

## Visual Experience Composer(VEC)を使用したA/Bテストの作成手順

1. [Adobe Experience Cloud](https://experience.adobe.com/)にログインし、__Target__&#x200B;をタップし、「__アクティビティ__」タブに移動します。

   + Experience Cloudダッシュボードに&#x200B;__Target__&#x200B;が表示されない場合は、右上の組織切り替えボタンで正しいAdobe組織が選択され、[Adobe Admin Console](https://adminconsole.adobe.com/)でTargetへのアクセス権が付与されていることを確認します。

1. 「**アクティビティを作成**」ボタンをクリックし、「**A/Bテスト**」アクティビティを選択します

   ![A/Bアクティビティ](assets/ab-target-activity.png)

1. 「**Visual Experience Composer**」オプションを選択し、アクティビティURLを指定して、「**次へ**」をクリックします

   ![アクティビティURL](assets/ab-test-url.png)

1. Visual Experience Composerでは、新しいアクティビティを作成した後、左側に2つのタブが表示されます。*エクスペリエンスA*&#x200B;と&#x200B;*エクスペリエンスB*。 リストからエクスペリエンスを選択します。 「**エクスペリエンスを追加**」ボタンを使用して、新しいエクスペリエンスをリストに追加できます。

   ![エクスペリエンスA](assets/experience.png)

1. 変更を開始するページ上の画像またはテキストを選択するか、コードエディターを使用してHTML要素を選択します。

   ![エレメント](assets/select-element.png)

1. テキストを&#x200B;*西オーストラリアでのキャンプ*&#x200B;から&#x200B;*オーストラリアの冒険*&#x200B;に変更します。 エクスペリエンスに追加された変更のリストが「変更」の下に表示されます。 変更した項目をクリックして編集し、CSSセレクターとその項目に追加された新しいコンテンツを表示できます。

   ![冒険](assets/adventures.png)

1. *エクスペリエンスA*&#x200B;を&#x200B;*アドベンチャー*&#x200B;に名前変更します。
1. 同様に、*エクスペリエンスB*&#x200B;のテキストを&#x200B;*西オーストラリアのキャンプ*&#x200B;から&#x200B;*オーストラリアの自然*&#x200B;に更新します。

   ![参照](assets/explore.png)

1. 「**次へ**」をクリックして「ターゲティング」に移動し、2つのエクスペリエンス間のトラフィックの手動配分を50 ～ 50に保ちます。

   ![ターゲット設定](assets/targeting.png)

1. 目標と設定については、「レポートソース」として「 Adobe Target 」を選択し、「目標指標」として「ページビューアクションによるコンバージョン」を選択します。

   ![ゴール](assets/goals.png)

1. アクティビティの名前を指定し、保存します。
1. 保存したアクティビティをアクティブ化して、変更をライブにプッシュします。

   ![ゴール](assets/activate.png)

1. 新しいタブでサイトページ（手順3のアクティビティURL）を開き、A/Bテストアクティビティからエクスペリエンス（アドベンチャーまたはエクスプローラ）を表示できるようにします。

   ![ゴール](assets/publish.png)

## 概要

この章では、テストを実行するコードを変更することなく、Webページのレイアウトとコンテンツをドラッグ&amp;ドロップ、入れ替え、変更することで、Visual Experience Composerを使用したエクスペリエンスを作成できました。

## サポートリンク

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
