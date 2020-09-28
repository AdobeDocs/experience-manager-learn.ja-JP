---
title: 投資混合パネルの設定
seo-title: 投資混合パネルの設定
description: これは、最初の対話型通信ドキュメントの作成に関するマルチステップチュートリアルのパート11です。このパートでは、現在とモデルの投資ミックスを表示する円グラフを追加します。
seo-description: これは、最初の対話型通信ドキュメントの作成に関するマルチステップチュートリアルのパート11です。このパートでは、現在とモデルの投資ミックスを表示する円グラフを追加します。
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 0%

---


# 投資混合パネルの設定

この部分では、円グラフを追加して、現在の投資とモデルの投資の組み合わせを表示します。

* AEM Formsにログインし、Adobe Experience Manager/Forms/Forms&amp;ドキュメントに移動します。

* 401KStatementフォルダーを開きます。

* 401KStatementを編集モードで開きます。

* アカウント保持者の現在とモデルの投資の組み合わせを表す円グラフを2つ追加します。

## 現在のアセットの混在 {#current-asset-mix}

* 右側の「現在のアセットミックス」パネルをタップし、「+」アイコンを選択してテキストコンポーネントを挿入します。 デフォルトのテキストを「現在のアセットミックス」に変更します。

* 「現在のアセットミックス」パネルをタップし、「+」アイコンを選択して、グラフコンポーネントを挿入します。 新しく挿入されたグラフコンポーネントをタップし、「レンチ」アイコンをクリックして、グラフの設定プロパティシートを開きます。

* 次の画像に示すように、プロパティを設定します。 グラフの種類が円グラフであることを確認します。

* X軸とY軸に連結されたデータモデルオブジェクトに注目してください。 フォームデータモデルのルート要素を選択し、ドリルダウンして適切な要素を選択する必要があります。

* ![currentassetmix](assets/currentassetmixchart.png)

## モデルアセットミックス {#model-asset-mix}

* 右側の「RecommendedAssetMix」パネルをタップし、「+」アイコンを選択してテキストコンポーネントを挿入します。 デフォルトのテキストを「Model Asset Mix」に変更します。

* 「RecommendedAssetMix」パネルをタップし、「+」アイコンを選択して、グラフコンポーネントを挿入します。 新しく挿入されたグラフコンポーネントをタップし、「レンチ」アイコンをクリックして、グラフの設定プロパティシートを開きます。

* 次の画像に示すように、プロパティを設定します。 グラフの種類が円グラフであることを確認します。

* X軸とY軸に連結されたデータモデルオブジェクトに注目してください。 フォームデータモデルのルート要素を選択し、ドリルダウンして適切な要素を選択する必要があります。

* ![assettype](assets/modelassettypechart.png)

