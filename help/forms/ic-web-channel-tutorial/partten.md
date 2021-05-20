---
title: リタイアメントの[Outlook]パネルの構成
seo-title: リタイアメントの[Outlook]パネルの構成
description: これは、最初のインタラクティブ通信ドキュメントを作成するための複数手順のチュートリアルの10部です。 ここでは、テキストとグラフのコンポーネントを追加して、リタイアメントの[Outlookパネル]を構成します。
seo-description: これは、最初のインタラクティブ通信ドキュメントを作成するための複数手順のチュートリアルの10部です。 ここでは、テキストとグラフのコンポーネントを追加して、リタイアメントの[Outlookパネル]を構成します。
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 1%

---


# リタイアメントのOutlookパネルの構成{#configuring-retirement-outlook-panel}

* これは、最初のインタラクティブ通信ドキュメントを作成するための複数手順のチュートリアルの10部です。 ここでは、テキストとグラフのコンポーネントを追加して、リタイアメントの[Outlookパネル]を構成します。

* AEM Formsにログインし、 Adobe Experience Manager / Forms / Forms &amp; Documentsに移動します。

* 401KStatementフォルダーを開きます。

* 401KStatementドキュメントを編集モードで開きます。

**LeftPanelのターゲット領域の設定**

* 右側のLeftPanelのターゲット領域をタップし、「+」アイコンをクリックしてコンポーネントの挿入ダイアログボックスを表示します。

* テキストを挿入コンポーネント。

* 新しく追加されたテキストコンポーネントを軽くタップして、コンポーネントツールバーを表示します。

* 「鉛筆」アイコンを選択して、デフォルトのテキストを編集します。

* デフォルトのテキストを「**Your Retiarment Income Outlook」に置き換えます。**

**RightPanelのターゲット領域の設定**

* 右側のRightPanelのターゲット領域をタップし、「+」アイコンをクリックしてコンポーネントの挿入ダイアログボックスを表示します。

* テキストを挿入コンポーネント。

* 新しく追加されたテキストコンポーネントを軽くタップして、コンポーネントツールバーを表示します。

* 「鉛筆」アイコンを選択して、デフォルトのテキストを編集します。

* デフォルトのテキストを「**Estimated Monthly Retieration Income」**&#x200B;に置き換えます。

## 除・売却所得のOutlookドキュメント・フラグメントの追加{#add-retirement-income-outlook-document-fragment}

* アセットアイコンをクリックし、「ドキュメントフラグメント」タイプのアセットを表示するフィルターを適用します。 RetiermentIncomeOutlookドキュメントフラグメントを[左パネル]のターゲット領域にドラッグ&amp;ドロップします。

* コンテンツ領域にドキュメントフラグメントを追加する場合は、このページ[を参照してください。](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/9.html)

## 月次予測収入チャートの追加{#adding-estimated-monthly-income-chart}

* 右側のRightPanelターゲット領域をクリックします。 「+」アイコンをクリックして、グラフコンポーネントを挿入します。 列グラフを使用して、推定月収を表示します。 新しく挿入したグラフコンポーネントをそっとタップします。 「レンチ」アイコンを選択して、設定プロパティシートを開きます。下のスクリーンショットに示すように、次のプロパティを使用してグラフを設定します。

**AEM Forms 6.4 — 推定月間所得列グラフの設定**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 — 推定月間所得列グラフの設定**

![forms65](assets/estimatedmonthlyincomechart65.PNG)




