---
title: 「国の作成」ドロップダウンリストコンポーネント
description: Aem forms コアドロップダウンコンポーネントに基づいて、国ドロップダウンリストコンポーネントを作成します。
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 3%

---

# ドロップダウンコンポーネントに基づく国ドロップダウンコンポーネントの作成

Adobe Experience Manager（AEM）での新しいコアコンポーネントの作成は、コンポーネント構造の定義、ダイアログのカスタマイズ、動的機能用の Sling モデルの実装など、いくつかの手順を伴うエキサイティングなプロセスです。

このチュートリアルを終了すると、次の方法を習得できます。

* Sling モデルを作成および使用してデータを動的に取得します。
* 新しいフィールドを追加し、他のフィールドを非表示にすることで、cq-dialog をカスタマイズします。
* 再利用のためにカスタマイズされた堅牢なコンポーネント構造を定義します。

「国」という名前のコンポーネントを使用すると、ユーザーは大陸を選択し、選択した大陸に対応する国をドロップダウンに動的に入力できます。 これは、標準のドロップダウンリストコンポーネントの基盤に基づいて構築され、この特定のユースケース向けに強化されます。

この動的で強力なコンポーネントに取り組んで作成しましょう。

## 前提条件

Adobe Experience Manager（AEM）で新しいコアコンポーネントを作成するには、スムーズな開発プロセスを実現するためにいくつかの前提条件を満たす必要があります。 始める前に必要な操作は次のとおりです：

* AEM Development Environment: ローカルで実行される機能的なクラウド対応インストール
* Visual Studio Code や IntelliJ などのAEM開発ツールへのアクセス
* 最新のアーキタイプを使用したセットアップとAEM プロジェクトの準備
* AEMの概念に関する基本知識
* Git リポジトリ、適切なバージョンの JDK などの基本的なツールと設定


## 次の手順

[コンポーネント構造の作成](./component.md)
