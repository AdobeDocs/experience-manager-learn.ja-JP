---
title: Visual Experience Composer を使用したパーソナライズ機能
description: Visual Experience Composer を使用して Adobe Target Activity を作成する方法を説明します。
version: Cloud Service
jira: KT-6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
duration: 141
source-git-commit: 970093bb54046fee49e2ac209f1588e70582ab67
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 100%

---

# Visual Experience Composer を使用したパーソナライズ機能 {#personalization-vec}

Visual Experience Composer（VEC）を使用して Target アクティビティの A/B テスト を作成する方法を説明します。

## 前提条件

AEM web サイトで VEC を使用するには、次の設定を行う必要があります。

1. [AEM web サイトへの Adobe Target の追加](./add-target-launch-extension.md)
1. [タグからの Adobe Target 呼び出しのトリガー](./load-and-fire-target.md)

## シナリオの概要

WKND サイトのホームページには、情報カードの形式で、ある都市のローカルのアクティビティや付近の人気スポットが表示されます。あなたはマーケターとして、「Adventure」セクションのティーザーのテキストを変更し、コンバージョンがどのように向上したかを理解することで、ホームページを変更するタスクを割り当てられました。

## Visual Experience Composer（VEC）を使用した A/B テストの作成手順

1.  [Adobe Experience Cloud](https://experience.adobe.com/) にログインして「__ターゲット__」をタップし、「__アクティビティ__」タブに移動します。

   + Experience Cloud ダッシュボードで「__ターゲット__」が表示されない場合は、右上の組織切り替えボタンで正しい Adobe 組織が選択され、[Adobe Admin Console](https://adminconsole.adobe.com/) でユーザーにターゲットへのアクセス権が付与されていることを確認します。

1. 「**アクティビティを作成**」ボタンをクリックし、「**A/B テスト**」アクティビティを選択します。

   ![A/B アクティビティ](assets/ab-target-activity.png)

1. 「**Visual Experience Composer**」オプションを選択し、アクティビティ URL を指定して「**次へ**」をクリックします。

   ![アクティビティ URL](assets/ab-test-url.png)

1. Visual Experience Composer では、アクティビティを作成した後、左側に「*エクスペリエンス A*」および 「*エクスペリエンス B*」の 2 つのタブが表示されます。リストからエクスペリエンスを選択します。 新しいエクスペリエンスをリストに追加するには、「**エクスペリエンスを追加**」ボタンをクリックします。

   ![エクスペリエンス A](assets/experience.png)

1. 変更を開始するには、ページ上の画像またはテキストを選択するか、コードエディターを使用して HTML 要素を選択できます。

   ![要素](assets/select-element.png)

1. テキストを「*Camping in Western Australia*」から「*Adventures of Australia*」に変更します。エクスペリエンスに追加された変更のリストは、「変更」の下に表示されます。 変更した項目をクリックして編集し、CSS セレクターと、その項目に追加された新しいコンテンツを表示することができます。

   ![Adventures](assets/adventures.png)

1. 「*Experience A*」から「*Adventure*」に名前を変更します。
1. 同様に、「*エクスペリエンス B*」のテキストを「*西オーストラリアでのキャンプ*」から「*オーストラリアの荒野を探検する*」に変更します。

   ![探索](assets/explore.png)

1. 「**次へ**」をクリック して「ターゲティング」に移動し、2 つのエクスペリエンスの間の手動のトラフィック配分を 50/50 の範囲に保ちます。

   ![ターゲティング](assets/targeting.png)

1. 「目標と設定」では、「レポートソース」として「Adobe Target」を選択し、「目標指標」として「ページビューアクションによるコンバージョン」を選択します。

   ![目標](assets/goals.png)

1. アクティビティの名前を指定して保存します。
1. 保存したアクティビティをアクティブ化して、変更をライブにプッシュします。

   ![目標](assets/activate.png)

1. サイトページ（手順 3 のアクティビティ URL）を新しいタブで開くと、A/B テストアクティビティからエクスペリエンス（探検または探索）を表示できるようになります。

   ![目標](assets/publish.png)

## 概要

この章では、テストを実行するコードを変更することなく、web ページのレイアウトとコンテンツをドラッグ＆ドロップ、入れ替え、変更することで、Visual Experience Composer を使用してエクスペリエンスを作成できました。

## サポートリンク

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
