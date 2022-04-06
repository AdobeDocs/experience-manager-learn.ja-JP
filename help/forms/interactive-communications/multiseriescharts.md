---
title: AEM Formsでの複数シリーズグラフ
seo-title: Multi Series Charts in AEM Forms
description: 印刷チャネルと Web チャネルのドキュメントで複数系列のグラフを作成するには、適切なフォームデータモデルを作成します。
seo-description: Create appropriate Form Data Model to create multi series charts in print and web channel documents.
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
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 2%

---

# 複数系列グラフ

AEM Forms 6.5 では、複数のシリーズグラフを作成および設定する機能が導入されました。 複数系列グラフは、通常、折れ線グラフ、棒グラフ、列グラフの種類に関連して使用されます。 次のグラフは、複数系列のグラフの好例です。 このグラフは、3 つの異なるミューチュアルファンドでの 10,000 米ドルの成長を示しています。 AEM Formsでこのようなグラフを作成して使用するには、適切なフォームデータモデルを作成する必要があります。

![多系列](assets/seriescharts.jfif)

AEM Formsで複数系列のグラフを作成するには、必要なエンティティとエンティティ間の関連付けを持つ適切なフォームデータモデルを作成する必要があります。 次のスクリーンショットは、エンティティと 3 つのエンティティ間の関連付けを示しています。 トップレベルでは、FUND エンティティと 1 対多の関連を持つ「組織」というエンティティが存在します。 次に、Fund エンティティは、Performance エンティティと 1 対多の関連を持ちます。

![フォームデータモデル](assets/formdatamodel.jfif)


## 複数系列グラフ用のフォームデータモデルの作成

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### 折れ線グラフの設定

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


お使いのシステムでこれをテストするには、次の手順に従ってください

* [AEM Package Manager を使用して、MutualFundFactSheet.zip をダウンロードし、インポートします。](assets/mutualfundfactsheet.zip)
* [SeriesChartSampleData.json をハードドライブにダウンロードします。](assets/serieschartsampledata.json) これは、グラフの入力に使用されるサンプルデータです。
* [「 Forms and Documents 」に移動します。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 「MutualFundGrowthFactSheet」インタラクティブ通信テンプレートを優しく選択します。
* プレビューをクリック |印刷チャネル |サンプルデータをアップロードします。
* この記事の一部として提供されているサンプルデータファイルを参照します。
* 前の手順でダウンロードしたサンプルデータを使用して、「MutualFundGrowthFactSheet」インタラクティブ通信の印刷チャネルをプレビューします。
