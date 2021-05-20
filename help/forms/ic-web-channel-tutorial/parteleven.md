---
title: 投資ミックスパネルの構成
seo-title: 投資ミックスパネルの構成
description: これは、最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの11部です。この部分では、現在とモデルの投資ミックスを表示する円グラフを追加します。
seo-description: これは、最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの11部です。この部分では、現在とモデルの投資ミックスを表示する円グラフを追加します。
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---


# 投資ミックスパネルの構成

ここでは、現在とモデルの投資ミックスを表示する円グラフを追加します。

* AEM Formsにログインし、 Adobe Experience Manager / Forms / Forms &amp; Documentsに移動します。

* 401KStatementフォルダーを開きます。

* 401KStatementを編集モードで開きます。

* 2つの円グラフを追加して、アカウント保有者の現在とモデルの投資ミックスを表します。

## 現在のアセットミックス{#current-asset-mix}

* 右側の「CurrentAssetMix」パネルをタップし、「+」アイコンを選択してテキストコンポーネントを挿入します。 デフォルトのテキストを「Current Asset Mix」に変更します。

* 「CurrentAssetMix」パネルをタップし、「+」アイコンを選択してグラフコンポーネントを挿入します。 新しく挿入されたグラフコンポーネントをタップし、「レンチ」アイコンをクリックして、グラフの設定プロパティシートを開きます。

* 次の画像に示すように、プロパティを設定します。 グラフの種類が円グラフであることを確認します。

* X軸とY軸に連結されたデータモデルオブジェクトに注意してください。 フォームデータモデルのルート要素を選択し、ドリルダウンして適切な要素を選択する必要があります。

* ![currentassetmix](assets/currentassetmixchart.png)

## モデルアセットミックス{#model-asset-mix}

* 右側の「RecommendedAssetMix」パネルをタップし、「+」アイコンを選択してテキストコンポーネントを挿入します。 デフォルトのテキストを「Model Asset Mix」に変更します。

* 「RecommendedAssetMix」パネルをタップし、「+」アイコンを選択してグラフコンポーネントを挿入します。 新しく挿入されたグラフコンポーネントをタップし、「レンチ」アイコンをクリックして、グラフの設定プロパティシートを開きます。

* 次の画像に示すように、プロパティを設定します。 グラフの種類が円グラフであることを確認します。

* X軸とY軸に連結されたデータモデルオブジェクトに注意してください。 フォームデータモデルのルート要素を選択し、ドリルダウンして適切な要素を選択する必要があります。

* ![assettype](assets/modelassettypechart.png)

