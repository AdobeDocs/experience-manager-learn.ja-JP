---
title: アダプティブFormsの概要
description: このチュートリアルでは、複数のタブを持つアダプティブフォームを作成する手順を説明します。 テーブル、アコーディオンレイアウト、ルールエディターを使用して、ビジネスルールを作成する方法を学習します。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 8c90fe1c-0c83-4287-9766-08d806b8815a
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 38%

---

# アダプティブFormsの概要 {#getting-started-with-adaptive-forms}

このチュートリアルでは、複数のタブを持つアダプティブフォームを作成する手順を説明します。 テーブル、アコーディオンレイアウト、ルールエディターを使用して、ビジネスルールを作成する方法を学習します。

アダプティブフォームを使用すると、動的で柔軟で応答の速い、魅力的なフォームを作成することができます。AEM Forms には、アダプティブフォームを作成して操作するための直感的なユーザーインターフェースと、すぐに使用できる各種のコンポーネントが用意されています。フォームモデルやスキーマをベースとしてアダプティブフォームを作成することも、フォームモデルを使用せずにアダプティブフォームを作成することもできます。フォームモデルを選択する場合、そのモデルが業務上の要件を満たしているかどうかだけでなく、インフラに対する現在の投資や既存のアセットを拡張できるかどうかについても慎重に検討することが重要です。

このチュートリアルでは、アダプティブフォームの作成時にフォームモデルを使用しません。

## 前提条件 {#prerequisites}

以下が必要です。

* フォームアドオンパッケージがインストールされたAEMの作業用インスタンス

* **localhost:4502 上でAEM Formsバージョン 6.4 以降を実行していると想定されます。**

* [client-libs-and-logo をダウンロードします。](assets/client-libs-and-logo.zip) および [getting-started-fragment](assets/getting-started-fragment.zip) ハードドライブに接続します。

* を使用して zip ファイルをAEMに読み込みます。 [パッケージマネージャー ](http://localhost:4502/crx/packmgr/index.jsp)
