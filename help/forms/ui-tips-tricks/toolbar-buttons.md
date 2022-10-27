---
title: ツールバーの次のボタンと前のボタンの間にスペースを設定する
description: ツールバーのボタンの間隔
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 4%

---

# ツールバーボタンの間隔

AEM Formsのツールバーに「次へ」ボタンと「前へ」ボタンを追加すると、デフォルトでは、ボタンが隣りに配置されます。 左側の前へ/戻るボタンを保持したまま、「次へ」ボタンをツールバーの右端に押すことができます

![toolbar-spacing](assets/toolbar-spacing.png)


## ツールバーのスタイル設定

上記の使用例は、スタイルエディターを使用することで簡単に実行できます。 ツールバーに「前へ」/「次へ」ボタンを追加したら、必ず編集メニューからスタイルレイヤーを選択します。 スタイルモードを選択した状態で、ツールバーを選択してスタイルプロパティシートを開きます。 「Dimensionと位置」セクションを展開し、すべてのプロパティが表示されていることを確認します。 次のプロパティを設定します。
* 寸法と位置
   * 幅: 100%
   * 位置：相対

変更を保存します

## 「次へ」ボタンのスタイル設定

「次へ」ボタンを選択し、（次のボタンのテキストではなく）次のボタンのスタイルプロパティシートを開くようにします。 次のプロパティを設定します。
* 寸法と位置
   * 位置：絶対上 1px 右 1px
* 境界線
   * 境界線の半径：4px(Top,Right,Bottom,Left)

変更を保存します
