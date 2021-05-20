---
title: アダプティブFormsの概要
seo-title: アダプティブFormsの概要
description: 'このチュートリアルでは、複数のタブを持つアダプティブフォームを作成する手順について説明します。 テーブル、アコーディオンレイアウト、ルールエディターを使用して、ビジネスルールを作成する方法を学習します。 '
seo-description: 'このチュートリアルでは、複数のタブを持つアダプティブフォームを作成する手順について説明します。 テーブル、アコーディオンレイアウト、ルールエディターを使用して、ビジネスルールを作成する方法を学習します。 '
uuid: 6f73cb1c-94e2-4ac7-89e5-a72141a06bbe
feature: アダプティブフォーム
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.3,6.4,6.5
discoiquuid: b6863d3d-8528-4a96-ae37-c8d1aa62d443
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 34%

---


# アダプティブFormsの概要{#getting-started-with-adaptive-forms}

このチュートリアルでは、複数のタブを持つアダプティブフォームを作成する手順について説明します。 テーブル、アコーディオンレイアウト、ルールエディターを使用して、ビジネスルールを作成する方法を学習します。

アダプティブフォームを使用すると、動的で柔軟で応答の速い、魅力的なフォームを作成することができます。AEM Forms には、アダプティブフォームを作成して操作するための直感的なユーザーインターフェースと、すぐに使用できる各種のコンポーネントが用意されています。フォームモデルやスキーマをベースとしてアダプティブフォームを作成することも、フォームモデルを使用せずにアダプティブフォームを作成することもできます。フォームモデルを選択する場合、そのモデルが業務上の要件を満たしているかどうかだけでなく、インフラに対する現在の投資や既存のアセットを拡張できるかどうかについても慎重に検討することが重要です。

このチュートリアルでは、アダプティブフォームの作成時にフォームモデルを使用しません。

## 前提条件 {#prerequisites}

以下が必要です。

* フォームアドオンパッケージをインストールしたAEMの動作中のインスタンス

* **localhost:4502上でAEM Formsバージョン6.4以降を実行していると想定されます。**

* [client-libs-and-logoとgetting-started-](assets/client-libs-and-logo.zip) フラグメ [ントをハードドラ](assets/getting-started-fragment.zip) イブにダウンロードします。

* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して、zipファイルをAEMに読み込みます。


