---
title: Visual Experience Composerを使用したパーソナライゼーション
description: Visual Experience Composerを使用したAdobe Targetアクティビティの作成方法を説明します。
feature: targeting
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 1%

---


# Visual Experience Composerを使用したパーソナライゼーション {#personalization-vec}

Visual Experience Composer(VEC)を使用してA/Bテストターゲットアクティビティを作成する方法を説明します。


## シナリオの概要

WKNDサイトのホームページは、地域のアクティビティや、情報カードの形で都市周辺で行う最善の方法を表示します。 マーケティング担当者には、アドベンチャーセクションのテーザーにテキストを変更し、コンバージョンの向上を理解することで、ホームページを変更するタスクが割り当てられています。

## Visual Experience Composer(VEC)を使用したA/Bテストの作成手順

1. Adobe Targetにログインし、「アクティビティ」タブに移動します
2. 「 **アクティビティを作成** 」ボタンをクリックし、「 **A/Bテスト** アクティビティ」を選択します

   ![A/Bアクティビティ](assets/ab-target-activity.png)

3. 「 **Visual Experience Composer** 」オプションを選択し、アクティビティURLを指定して、「 **次へ」をクリックします**

   ![アクティビティURL](assets/ab-test-url.png)

4. 新しいアクティビティを作成すると、Visual Experience Composerの左側に2つのタブが表示されます。 *エクスペリエンスA* と *エクスペリエンスB*。 リストからエクスペリエンスを選択します。 「エクスペリエンス **」ボタンを使用して、リストに新しいエクスペリエンスを追加でき** ます。

   ![エクスペリエンスA](assets/experience.png)

5. 開始が変更を加えるためのページ上の画像またはテキストを選択するか、またはを使用する場合は、コードエディターを使用してHTML要素を選択できます。

   ![エレメント](assets/select-element.png)

6. テキストを「 *キャンピング（西オーストラリア）」から* 「オーストラリア *の冒険*」に変更します。 エクスペリエンスに追加された変更のリストは、「変更」の下に表示されます。 変更したアイテムをクリックして編集し、CSSセレクターと新しいコンテンツを表示できます。

   ![冒険](assets/adventures.png)

7. エク *スペリエンスAの名前を* Adventureに変更 **
8. 同様に、 *エクスペリエンスB* のテキストを、西オーストラリアの *キャンピングのテキストに更新して、オーストラリアの自然* を探検します **。

   ![エクスプローラ](assets/explore.png)

9. 「 **次へ** 」をクリックして「ターゲット設定」に移動し、2つのエクスペリエンス間で手動のトラフィック配分を50 ～ 50に保ちます。

   ![ターゲット設定](assets/targeting.png)

10. 「目標と設定」で、レポートソースをAdobe Targetとして選択し、目標指標をページ表示アクションを使用したコンバージョンとして選択します。

   ![ゴール](assets/goals.png)

11. アクティビティの名前を入力し、「保存」をクリックします。
12. 保存したアクティビティをアクティブ化して、変更をライブにプッシュします。

   ![ゴール](assets/activate.png)

13. サイトページ(手順3のアクティビティURL)を新しいタブで開き、A/Bテストアクティビティからエクスペリエンス（アドベンチャーまたはエクスプローラ）のいずれかを表示できるようにします。

   ![ゴール](assets/publish.png)

## 概要

この章では、マーケティング担当者が、テストを実行するコードを変更することなく、Webページのレイアウトとコンテンツをドラッグ&amp;ドロップ、入れ替え、変更することで、Visual Experience Composerを使用してエクスペリエンスを作成できました。

## サポートリンク

* [Adobe Experience Cloudデバッガー — Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloudデバッガ — Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)