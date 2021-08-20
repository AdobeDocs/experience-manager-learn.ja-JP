---
title: 所得パネルへのコンポーネントの追加
description: 所得パネルにテーブルを追加します。 テーブル行を設定し、ルールエディターを使用して総計を計算します。
feature: アダプティブフォーム
version: 6.4,6.5
thumbnail: 22198.jpg
kt: 4211
topic: 開発
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 1%

---


# 所得パネルへのコンポーネントの追加 {#adding-components-to-income-panel}

所得パネルにテーブルを追加します。 テーブル行を設定し、ルールエディターを使用して総計を計算します。

**表コンポーネントの追加と設定**

>[!VIDEO](https://video.tv.adobe.com/v/22198?quality=9&learn=on)



## 所得テーブルを動的にする {#make-the-income-table-dynamic}

**編集モードであることを確認します。編集ボタンは、ブラウザーの右上に表示されます。**

* デフォルトでは、アダプティブフォームに表を挿入すると、表は動的ではなく、実行時に表に新しい行を追加することはできません。

* ブラウザーを更新します。

* コンテンツ階層の「 Row1 」を選択します。

* レンチアイコンをクリックして、プロパティシートを開きます。

* 繰り返し設定の下の「最小値」と「最大値」を1と5に設定し、青いチェックマークアイコンをクリックして変更を保存します。 つまり、テーブルの行数は最大5行までです。 不特定の数の行を設定するには、最大数を —1に設定します。

## 総計を計算するルールを作成する {#create-rule-to-calculate-grand-total}


>[!VIDEO](https://video.tv.adobe.com/v/22197?quality=9&learn=on)


