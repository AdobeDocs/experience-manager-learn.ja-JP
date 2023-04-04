---
title: AEM Formsでの複数シリーズグラフ
description: 印刷チャネルと Web チャネルのドキュメントで複数系列のグラフを作成するには、適切なフォームデータモデルを作成します。
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 3%

---

# 複数系列グラフ

AEM Forms 6.5 では、複数のシリーズグラフを作成および設定する機能が導入されました。 複数系列グラフは、通常、折れ線グラフ、棒グラフ、列グラフの種類に関連して使用されます。 次のグラフは、複数系列のグラフの好例です。 このグラフは、3 つの異なるミューチュアルファンドでの 10,000 米ドルの成長を示しています。 AEM Formsでこのようなグラフを作成して使用するには、適切なフォームデータモデルを作成する必要があります。

![複数系列グラフ](assets/seriescharts.jfif)

AEM Formsで複数系列のグラフを作成するには、必要なエンティティとエンティティ間の関連付けを持つ適切なフォームデータモデルを作成する必要があります。 次のスクリーンショットは、エンティティと 3 つのエンティティ間の関連付けを示しています。 トップレベルでは、FUND エンティティと 1 対多の関連を持つ「組織」というエンティティが存在します。 次に、Fund エンティティは、Performance エンティティと 1 対多の関連を持ちます。

![フォームデータモデル](assets/formdatamodel.jfif)

## 複数系列グラフ用のフォームデータモデルの作成

>[!VIDEO](https://video.tv.adobe.com/v/26352?quality=12&learn=on)

### 折れ線グラフの設定

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=12&learn=on)

お使いのシステムでこれをテストするには、次の手順に従ってください

* [AEM Package Manager を使用して、MutualFundFactSheet.zip をダウンロードし、インポートします。](assets/mutualfundfactsheet.zip)
* [SeriesChartSampleData.json をハードドライブにダウンロードします。](assets/serieschartsampledata.json) これは、グラフにデータを入力するために使用されるサンプルデータです。
* [「 Forms and Documents 」に移動します。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 「MutualFundGrowthFactSheet」インタラクティブ通信テンプレートを優しく選択します。
* プレビューをクリック |印刷チャネル |サンプルデータをアップロードします。
* この記事の一部として提供されているサンプルデータファイルを参照します。
* 前の手順でダウンロードしたサンプルデータを使用して、「MutualFundGrowthFactSheet」インタラクティブ通信の印刷チャネルをプレビューします。
