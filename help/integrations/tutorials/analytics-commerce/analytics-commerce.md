---
title: Analytics とコマースの統合チュートリアル
description: Analytics をコマースと統合する方法を説明します。
solution: Analytics, Commerce
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
index: true
hidefromtoc: true
kt: null
thumbnail: null
last-substantial-update: 2023-04-11T00:00:00Z
badgeIntegration: label="統合" type="positive"
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---


# Analytics とコマースの統合

* アカウントがAdobe Analyticsにアクセスできることを確認します。

* Adobe Analyticsでプロジェクトを作成します。

* スキーマを作成します。
   * これは、後の手順で選択するオプションから選択する必要があります。 スキーマを作成するには、左の列の「Data Management」の下を見て、「スキーマ」を見つけます。 左上で、「スキーマを作成」をクリックします。 「XDM ExperienceEvent」を選択します。
   * 左側の [ フィールドの検索 ] グループで、[ 追加 ] をクリックします。
      * 検索では、 `ExperienceEvent Commerce`
      * を探す `Adobe Analytics ExperienceEvent Commerce` 「 」チェックボックスをオンにします。
      * 必ず `Add field groups` 右上で保存して続行する
* データセットを作成する場合は、次に「DataStream」を設定する際に必要です。
   * データセットは、左の列「Data Management」の下にあり、「データセット」を探します。
   * 次に、右上の「データセットを作成」をクリックします。 スキーマからデータセットを作成します。
   * 前に作成したスキーマを検索して使用します。
* データストリームの作成. 「左の列のデータ収集」を使用し、「データストリーム」を検索することで取得できます。
* パネルとセグメントを含むテーブルを作成する。 このチュートリアルを複雑にするには、経験豊富な Analytics 担当者がサポートを受ける必要があります。


レポートを表示するには、 experience.adobe.com に移動して、ワークスペースプロジェクトを見つけ、表示するプロジェクトのリンクをクリックすると、次の画像のように表示されます

![一部のコマースデータの Analytics のスクリーンショット](./assets/analytics-commerce/analytics-screenshot-commerce-items.png)