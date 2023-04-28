---
title: アダプティブフォームの基本を学ぶ
description: このチュートリアルでは、複数のタブを持つアダプティブフォームを作成する手順を説明します。 テーブル、アコーディオンレイアウトおよびルールエディターを使用して、ビジネスルールを作成する方法を学習します。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 8c90fe1c-0c83-4287-9766-08d806b8815a
last-substantial-update: 2020-02-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '207'
ht-degree: 100%

---

# アダプティブフォームの基本を学ぶ {#getting-started-with-adaptive-forms}

このチュートリアルでは、複数のタブを持つアダプティブフォームを作成する手順を説明します。 テーブル、アコーディオンレイアウトおよびルールエディターを使用して、ビジネスルールを作成する方法を学習します。

アダプティブフォームを使用すると、動的で柔軟で応答の速い、魅力的なフォームを作成することができます。AEM Forms には、アダプティブフォームを作成して操作するための直感的なユーザーインターフェースと、すぐに使用できる各種のコンポーネントが用意されています。フォームモデルやスキーマをベースとしてアダプティブフォームを作成することも、フォームモデルを使用せずにアダプティブフォームを作成することもできます。フォームモデルを選択する場合、そのモデルが業務上の要件を満たしているかどうかだけでなく、インフラに対する現在の投資や既存のアセットを拡張できるかどうかについても慎重に検討することが重要です。

このチュートリアルでは、アダプティブフォームの作成時にフォームモデルを使用しません。

## 前提条件 {#prerequisites}

以下が必要です。

* フォームアドオンパッケージがインストールされた AEM の実用的なインスタンス

* **localhost:4502 上で AEM Forms バージョン 6.4 以降が動作していると想定します。**

* [client-libs-and-logo](assets/client-libs-and-logo.zip) および [getting-started-fragment](assets/getting-started-fragment.zip) をハードドライブにダウンロードします。

* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して zip ファイルを AEM に読み込みます。
