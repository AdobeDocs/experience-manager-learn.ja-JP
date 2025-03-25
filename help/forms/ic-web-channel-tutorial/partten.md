---
title: 退職後収入見通しパネルの設定
description: これは、最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの第 10 部です。ここでは、テキストおよびグラフコンポーネントを追加して、定年後の収入見通しのパネルを設定します。
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
exl-id: 0dd8a430-9a4e-4dc7-ad75-6ad2490430f2
duration: 71
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 100%

---

# 退職後収入見通しパネルの設定{#configuring-retirement-outlook-panel}

* これは、最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの第 10 部です。ここでは、テキストおよびグラフコンポーネントを追加して、定年後の収入見通しのパネルを設定します。

* AEM Forms にログインし、Adobe Experience Manager／Forms／フォームとドキュメントに移動します。

* 401KStatement フォルダーを開きます。

* 401KStatement ドキュメントを編集モードで開きます。

**LeftPanel のターゲット領域の設定**

* 右側の LeftPanel のターゲット領域をタップし、「+」アイコンをクリックして、コンポーネントを挿入ダイアログボックスを表示します。

* テキストコンポーネントを挿入します。

* 新しく追加されたテキストコンポーネントを軽くタップして、コンポーネントツールバーを表示します

* 「鉛筆」アイコンを選択して、デフォルトのテキストを編集します。

* デフォルトのテキストを「**退職所得見通し**」に置き換えます。

**RightPanel のターゲット領域の設定**

* 右側の RightPanel のターゲット領域をタップし、「+」アイコンをクリックして、コンポーネントを挿入ダイアログボックスを表示します。

* テキストコンポーネントを挿入します。

* 新しく追加されたテキストコンポーネントを軽くタップして、コンポーネントツールバーを表示します。

* 「鉛筆」アイコンを選択して、デフォルトのテキストを編集します。

* デフォルトのテキストを「**推定月次退職所得**」に置き換えます。

## 退職所得見通しドキュメントフラグメントの追加 {#add-retirement-income-outlook-document-fragment}

* アセットアイコンをクリックし、フィルターを適用して「ドキュメントフラグメント」タイプのアセットを表示します。RetieramentIncomeOutlook ドキュメントフラグメントを左パネルのターゲット領域にドラッグ＆ドロップします。

* コンテンツエリアにドキュメントフラグメントを追加する方法については、[こちらのページ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html?lang=ja)を参照してください。

## 推定月次所得グラフの追加 {#adding-estimated-monthly-income-chart}

* 右側の RightPanel ターゲット領域をクリックします。「+」アイコンをクリックして、グラフコンポーネントを挿入します。列グラフを使用して、推定月次所得を表示します。新しく挿入したグラフコンポーネントを軽くタップします。「レンチ」アイコンを選択して、設定プロパティシートを開きます。次のプロパティを使用してグラフを設定します（以下のスクリーンショットを参照）。

**AEM Forms 6.4 - 推定月次所得列グラフの設定**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 - 推定月次所得列グラフの設定**

![forms65](assets/estimatedmonthlyincomechart65.PNG)

## 次の手順

[円グラフの設定](./parteleven.md)
