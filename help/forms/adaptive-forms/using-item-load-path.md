---
title: 項目読み込みパスを使用したドロップダウンリストへの入力
description: crx ノードから値を読み取るためのドロップダウンリストの設定と入力
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-10961
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-20T00:00:00Z
thumbnail: item-load.jpg
exl-id: 89c486c8-95c3-4cd4-bf8e-a1b3558f17d6
duration: 34
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 100%

---

# AEM Forms の項目読み込みプロパティ

項目読み込みパスのプロパティを使用して、ドロップダウンリストの設定と入力を行います。
「項目読み込みパス」フィールドを使用すると、作成者はドロップダウンリストで使用可能なオプションを読み込む URL を指定できます。
crx でこのようなノードを作成するには、次の手順に従います。
* crx にログインします
* 「アセット」というノードを作成し（要件に応じてこのノードに名前を付けることができます）、「コンテンツ」に sling:folder と入力します。
* 保存
* 新しく作成したアセットノードをクリックし、次に示すようにプロパティを設定します。
* assettypes という文字列型のプロパティを作成します（必要に応じて名前を付けることができます）。プロパティが複数値であることを確認します。必要な値を指定して保存します。
  ![item-load-path](assets/item-load-path-crx.png)

これらの値をドロップダウンリストに読み込むには、項目読み込みパスのプロパティに **/content/assets/assettypes** というパスを指定します。

サンプルパッケージは、[こちらからダウンロード](assets/item-load-path-package.zip)できます。
