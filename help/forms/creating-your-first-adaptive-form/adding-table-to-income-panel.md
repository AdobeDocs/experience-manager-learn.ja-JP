---
title: 所得パネルへのコンポーネントの追加
description: 所得パネルにテーブルを追加します。 テーブルの行を設定し、ルールエディターを使用して総計を計算します。
feature: Adaptive Forms
version: 6.4,6.5
thumbnail: 22198.jpg
kt: 4211
topic: Development
role: Developer
level: Beginner
exl-id: e7674c46-259f-4dbd-96db-c40369534911
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# 所得パネルへのコンポーネントの追加 {#adding-components-to-income-panel}

所得パネルにテーブルを追加します。 テーブルの行を設定し、ルールエディターを使用して総計を計算します。

**テーブルコンポーネントの追加と設定**

>[!VIDEO](https://video.tv.adobe.com/v/22198?quality=12&learn=on)



## 収入テーブルを動的にする {#make-the-income-table-dynamic}

**編集モードになっていることを確認します。 編集ボタンは、ブラウザーの右上にあります。**

* デフォルトでは、アダプティブフォームに表を挿入すると、表は動的ではなく、実行時に表に新しい行を追加できません。

* ブラウザーを更新します。

* コンテンツ階層で「 Row1 」を選択します。

* レンチアイコンをクリックして、プロパティシートを開きます。

* 繰り返し設定の下の「最小」と「最大」のカウントを 1 と 5 に設定し、青いチェックマークアイコンをクリックして変更を保存します。 つまり、テーブルの行数は最大 5 行までです。 行の数を無限に設定するには、最大数を —1 に設定します。

## 総計を計算するルールを作成 {#create-rule-to-calculate-grand-total}


>[!VIDEO](https://video.tv.adobe.com/v/22197?quality=12&learn=on)
