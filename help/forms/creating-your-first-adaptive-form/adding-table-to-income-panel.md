---
title: 所得パネルへのコンポーネントの追加
seo-title: 所得パネルへのコンポーネントの追加
description: 所得パネルにテーブルを追加します。 テーブル行を設定し、ルールエディターを使用して総計を計算します。
seo-description: 所得パネルにテーブルを追加します。 テーブル行を設定し、ルールエディターを使用して総計を計算します。
uuid: d5c98561-c559-4624-976a-7a1486da7e69
feature: アダプティブフォーム
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.4,6.5
thumbnail: 22198.jpg
kt: 4211
discoiquuid: fa483260-38ff-40d8-96a7-1de11d8b792b
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 1%

---


# 所得パネル{#adding-components-to-income-panel}へのコンポーネントの追加

所得パネルにテーブルを追加します。 テーブル行を設定し、ルールエディターを使用して総計を計算します。

**表コンポーネントの追加と設定**

>[!VIDEO](https://video.tv.adobe.com/v/22198?quality=9&learn=on)



## 収入テーブルを動的にする{#make-the-income-table-dynamic}

**編集モードであることを確認します。編集ボタンは、ブラウザーの右上に表示されます。**

* デフォルトでは、アダプティブフォームに表を挿入すると、表は動的ではなく、実行時に表に新しい行を追加することはできません。

* ブラウザーを更新します。

* コンテンツ階層の「 Row1 」を選択します。

* レンチアイコンをクリックして、プロパティシートを開きます。

* 繰り返し設定の下の「最小値」と「最大値」を1と5に設定し、青いチェックマークアイコンをクリックして変更を保存します。 つまり、テーブルの行数は最大5行までです。 不特定の数の行を設定するには、最大数を —1に設定します。

## 総計を計算するルールを作成する{#create-rule-to-calculate-grand-total}


>[!VIDEO](https://video.tv.adobe.com/v/22197?quality=9&learn=on)


