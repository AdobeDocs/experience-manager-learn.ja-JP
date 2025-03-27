---
title: 国ドロップダウンリストコンポーネントの作成
description: AEM Forms コアドロップダウンコンポーネントに基づいて、国ドロップダウンリストコンポーネントを作成します。
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: aef151bc-daf1-4abd-914a-6299f3fb58e4
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '245'
ht-degree: 100%

---

# ドロップダウンコンポーネントに基づく国ドロップダウンコンポーネントの作成

Adobe Experience Manager（AEM）での新しいコアコンポーネントの作成は、コンポーネント構造の定義、ダイアログのカスタマイズ、動的な機能用の Sling モデルの実装など、いくつかの手順を伴う興味深いプロセスです。

このチュートリアルの終わりまでに、次の方法を習得することができます。

* Sling モデルを作成および使用して、データを動的に取得します。
* 新しいフィールドを追加し、他のフィールドを非表示にして、cq-dialog をカスタマイズします。
* 再利用のために調整された堅牢なコンポーネント構造を定義します。

「国」という名前のコンポーネントを使用すると、ユーザーは大陸を選択し、選択した大陸に対応する国をドロップダウンに動的に入力できます。これは、標準のドロップダウンリストコンポーネントの基盤として作成され、この特定のユースケース向けに強化されます。

この動的で強力なコンポーネントを今すぐ作成しましょう。

## 前提条件

Adobe Experience Manager（AEM）での新しいコアコンポーネントの作成には、スムーズな開発プロセスを実現するためにいくつかの前提条件を満たす必要があります。開始する前の必要な操作は次のとおりです。

* AEM開発環境：ローカルで実行可能なクラウド対応機能インストール
* Visual Studio Code や IntelliJ などの AEM 開発ツールへのアクセス
* 最新のアーキタイプを使用した Maven 設定と AEM プロジェクト
* AEM の概念の基本知識
* Git リポジトリ、適切なバージョンの JDK などの基本的なツールと設定


## 次の手順

[コンポーネント構造の作成](./component.md)
