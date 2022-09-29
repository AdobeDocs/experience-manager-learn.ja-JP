---
title: Visual Experience Composer を使用したパーソナライゼーション
description: Visual Experience Composer を使用してAdobe Targetアクティビティを作成する方法を説明します。
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 2%

---

# Visual Experience Composer を使用したパーソナライゼーション {#personalization-vec}

Visual Experience Composer(VEC) を使用して A/B テスト Target アクティビティを作成する方法を説明します。

## 前提条件

AEM Web サイトで VEC を使用するには、次の設定を行う必要があります。

1. [AEM Web サイトにAdobe Targetを追加する](./add-target-launch-extension.md)
1. [Launch からのAdobe Target呼び出しのトリガー](./load-and-fire-target.md)

## シナリオの概要

WKND サイトのホームページには、ローカルのアクティビティや、情報カードの形式で都市の周りを取り巻く最適な操作が表示されます。 マーケターは、アドベンチャーセクションのティーザーにテキストを変更し、コンバージョンをどのように改善するかを理解することで、ホームページを変更するタスクを割り当てられました。

## Visual Experience Composer(VEC) を使用した A/B テストの作成手順

1. ログイン先 [Adobe Experience Cloud](https://experience.adobe.com/)、をタップします。 __ターゲット__&#x200B;をクリックし、 __アクティビティ__ タブ

   + 表示されない __ターゲット__ Experience Cloudダッシュボードで、右上の組織切り替えボタンで正しいAdobe組織が選択され、ユーザーに Target へのアクセス権が付与されていることを確認します ( [Adobe Admin Console](https://adminconsole.adobe.com/).

1. クリック **アクティビティを作成** ボタンをクリックし、 **A/B テスト** アクティビティ

   ![A/B アクティビティ](assets/ab-target-activity.png)

1. を選択します。 **Visual Experience Composer** 」オプションを選択し、アクティビティ URL を指定して、 **次へ**

   ![アクティビティ URL](assets/ab-test-url.png)

1. Visual Experience Composer では、新しいアクティビティを作成した後、左側に 2 つのタブが表示されます。 *エクスペリエンス A* および *エクスペリエンス B*. リストからエクスペリエンスを選択します。 新しいエクスペリエンスをリストに追加するには、 **エクスペリエンスを追加** 」ボタンをクリックします。

   ![エクスペリエンス A](assets/experience.png)

1. 変更を開始するには、ページ上の画像またはテキストを選択するか、コードエディターを使用して要素を選択し、HTMLできます。

   ![要素](assets/select-element.png)

1. 次のテキストを変更： *西オーストラリアのキャンピング* から *オーストラリアの冒険*. エクスペリエンスに追加された変更のリストは、「変更」の下に表示されます。 変更した項目をクリックして編集し、CSS セレクターと、その項目に追加された新しいコンテンツを表示することができます。

   ![冒険](assets/adventures.png)

1. 名前を変更 *エクスペリエンス A* から *冒険*
1. 同様に、 *エクスペリエンス B* から *西オーストラリアのキャンピング* から *オーストラリアの荒野を探検する*.

   ![参照](assets/explore.png)

1. クリック **次へ** 「ターゲティング」に移行する際に、2 つのエクスペリエンスの間の手動のトラフィック配分を 50 ～ 50 の範囲に保ちます。

   ![ターゲット設定](assets/targeting.png)

1. 「目標と設定」では、「レポートソース」として「 Adobe Target 」を選択し、「目標指標」として「ページビューアクションによるコンバージョン」を選択します。

   ![ゴール](assets/goals.png)

1. アクティビティの名前を指定し、保存します。
1. 保存したアクティビティをアクティブ化して、変更をライブにプッシュします。

   ![ゴール](assets/activate.png)

1. サイトページ（手順 3 のアクティビティ URL）を新しいタブで開き、A/B テストアクティビティからエクスペリエンス（アドベンチャーまたは探索）を表示できるようにします。

   ![ゴール](assets/publish.png)

## 概要

この章では、Web ページのレイアウトとコンテンツを、テストを実行するコードを変更することなくドラッグ&amp;ドロップ、入れ替え、変更することで、Visual Experience Composer を使用してエクスペリエンスを作成できました。

## サポートリンク

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
