---
title: 収入パネルへのコンポーネントの追加
description: 収入パネルにテーブルを追加します。テーブルの行を設定し、ルールエディターを使用して総計を計算します。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
thumbnail: 22198.jpg
jira: KT-4211
topic: Development
role: Developer
level: Beginner
exl-id: e7674c46-259f-4dbd-96db-c40369534911
duration: 352
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '193'
ht-degree: 100%

---

# 収入パネルへのコンポーネントの追加 {#adding-components-to-income-panel}

収入パネルにテーブルを追加します。テーブルの行を設定し、ルールエディターを使用して総計を計算します。

**テーブルコンポーネントの追加と設定**

>[!VIDEO](https://video.tv.adobe.com/v/326904?quality=12&learn=on&captions=jpn)



## 収入テーブルを動的にする {#make-the-income-table-dynamic}

**編集モードになっていることを確認します。編集ボタンは、ブラウザーの右上にあります。**

* デフォルトでは、アダプティブフォームにテーブルを挿入すると、テーブルは動的ではないため、実行時にテーブルに新しい行を追加できません。

* ブラウザーを更新します。

* コンテンツ階層で「Row1」を選択します。

* レンチアイコンをクリックして、プロパティシートを開きます。

* 繰り返し設定の下の「最小」と「最大」のカウントを 1 と 5 に設定し、青いチェックマークアイコンをクリックして変更を保存します。つまり、テーブルの行数は最大 5 行までです。行の数を無限に設定するには、最大カウントを -1 に設定します。

## 総計を計算するルールを作成する {#create-rule-to-calculate-grand-total}


>[!VIDEO](https://video.tv.adobe.com/v/326893?quality=12&learn=on&captions=jpn)

## 次の手順

[アセットパネルの設定](./configuring-assets-panel.md)
