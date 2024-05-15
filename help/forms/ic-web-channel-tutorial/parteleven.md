---
title: 投資ミックスパネルの設定
description: これは、初めてのインタラクティブなコミュニケーションドキュメントを作成するための、複数の手順からなるチュートリアルの第 11 部です。この部分では、現在の投資ミックスとモデルの投資ミックスを表示する円グラフを追加します。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
duration: 69
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 100%

---

# 投資ミックスパネルの設定

ここでは、現在およびモデルのアセットミックスを表示する円グラフを追加します。

* AEM Forms にログインし、Adobe Experience Manager／Forms／フォームとドキュメントに移動します。

* 401KStatement フォルダーを開きます。

* 401KStatement を編集モードで開きます。

* 2 つの円グラフを追加して、口座保有者の現在およびモデルのアセットミックスを表します。

## 現在のアセットミックス {#current-asset-mix}

* 右側の CurrentAssetMix パネルをタップし、「+」アイコンを選択してテキストコンポーネントを挿入します。 デフォルトのテキストを「現在のアセットミックス」に変更します。

* CurrentAssetMix パネルをタップし、「+」アイコンを選択してグラフコンポーネントを挿入します。新しく挿入されたグラフコンポーネントをタップし、レンチアイコンをクリックして、グラフの設定プロパティシートを開きます。

* 次の画像に示すようにプロパティを設定します。グラフの種類が円グラフであることを確認します。

* X 軸と Y 軸に連結されたデータモデルオブジェクトに注意してください。 フォームデータモデルのルート要素を選択し、ドリルダウンして適切な要素を選択する必要があります。

* ![currentassetmix](assets/currentassetmixchart.png)

## モデルアセットミックス {#model-asset-mix}

* 右側の RecommendedAssetMix パネルをタップし、「+」アイコンを選択してテキストコンポーネントを挿入します。 デフォルトのテキストを「モデルアセットミックス」に変更します。

* RecommendedAssetMix パネルをタップし、「+」アイコンを選択してグラフコンポーネントを挿入します。 新しく挿入されたグラフコンポーネントをタップし、レンチアイコンをクリックして、グラフの設定プロパティシートを開きます。

* 次の画像に示すようにプロパティを設定します。グラフの種類が円グラフであることを確認します。

* X 軸と Y 軸に連結されたデータモデルオブジェクトに注意してください。 フォームデータモデルのルート要素を選択し、ドリルダウンして適切な要素を選択する必要があります。

* ![assettype](assets/modelassettypechart.png)

## 次の手順

[Web チャネルドキュメントの配信準備](./parttwelve.md)
