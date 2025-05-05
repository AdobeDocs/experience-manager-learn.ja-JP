---
title: AEM Forms での複数系列グラフ
description: 印刷チャネルと web チャネルのドキュメントで複数系列のグラフを作成するには、適切なフォームデータモデルを作成します。
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
last-substantial-update: 2019-07-07T00:00:00Z
duration: 430
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '279'
ht-degree: 100%

---

# 複数系列グラフ

AEM Forms 6.5 では、複数系列グラフを作成および設定する機能が導入されました。複数系列グラフは、通常、折れ線グラフ、棒グラフ、列グラフの種類に関連して使用されます。次のグラフは、複数系列グラフの良い例です。このグラフは、10,000 米ドルを 3 種類の投資信託に投資した場合の、一定期間の成長を示しています。AEM Formsでこのようなグラフを作成して使用するには、適切なフォームデータモデルを作成する必要があります。

![複数系列グラフ](assets/series_charts.png)

AEM Forms で複数系列のグラフを作成するには、必要なエンティティとエンティティ間の関連付けを持つ適切なフォームデータモデルを作成する必要があります。次のスクリーンショットは、エンティティと 3 つのエンティティ間の関連付けを示しています。トップレベルでは、Fund エンティティと 1 対多の関連を持つ「組織」というエンティティがあります。次に、Fund エンティティは Performance エンティティと 1 対多の関連があります。

![フォームデータモデル](assets/form_data_model.png)

## 複数系列グラフ用のフォームデータモデルの作成

>[!VIDEO](https://video.tv.adobe.com/v/35442?quality=12&learn=on&captions=jpn)

### 線系列グラフの設定

>[!VIDEO](https://video.tv.adobe.com/v/35443?quality=12&learn=on&captions=jpn)

お使いのシステムでこれをテストするには、次の手順に従ってください。

* [AEM Package Manager を使用して、MutualFundFactSheet.zip をダウンロードし、読み込みます。](assets/mutualfundfactsheet.zip)
* [SeriesChartSampleData.json をハードドライブにダウンロードします。](assets/serieschartsampledata.json)これは、グラフにデータを入力するために使用されるサンプルデータです。
* [「フォームとドキュメント」に移動します。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 「MutualFundGrowthFactSheet」インタラクティブ通信テンプレートを選択します。
* プレビュー／印刷チャネル／サンプルデータをアップロードをクリックします。
* この記事の一部として提供されているサンプルデータファイルを参照します。
* 前の手順でダウンロードしたサンプルデータを使用して、「MutualFundGrowthFactSheet」インタラクティブ通信の印刷チャネルをプレビューします。
