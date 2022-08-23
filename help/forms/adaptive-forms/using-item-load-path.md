---
title: 項目読み込みパスを使用したドロップダウンリストへの入力
description: crx ノードから値を読み取るためのドロップダウンリストの設定と設定
feature: Adaptive Forms
version: 6.4,6.5
kt: 10961
topic: Development
role: Developer
level: Beginner
source-git-commit: 614db8b03a823b60846ab8ccfa8fbc29a41f7791
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 0%

---

# AEM Formsの項目読み込みプロパティ

項目読み込みパスのプロパティを使用して、ドロップダウンリストを設定および設定します。
「項目読み込みパス」フィールドを使用すると、作成者はドロップダウンリストで使用可能なオプションを読み込む URL を指定できます。
crx でこのようなノードを作成するには、次の手順に従ってください

* crx にログイン
* assets というノードを作成し（必要に応じてこのノードに名前を付けることができます）、「content」に sling:folder と入力します。
* 保存
* 新しく作成されたアセットノードをクリックし、次に示すようにプロパティを設定します。
* assettypes 型の String 型のプロパティを作成し（必要に応じて名前を付けることができます）、multivalue を指定する必要があります。 必要な値を指定して保存します。

![item-load-path](assets/item-load-path-crx.png)

これらの値をドロップダウンリストに読み込むには、項目読み込みパスのプロパティに次のパスを指定します  **/content/assets/assettypes**

サンプルパッケージは、 [ここからダウンロード](assets/item-load-path-package.zip)
