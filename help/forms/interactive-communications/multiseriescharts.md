---
title: AEM Formsでの複数シリーズグラフ
seo-title: Multi Series Charts in AEM Forms
description: 適切なフォームデータモデルを作成し、印刷チャネルとWebチャネルのドキュメントで複数系列グラフを作成します。
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
source-git-commit: 0049c9fd864bd4dd4f8c33b1e40e94aad3ffc5b9
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 2%

---


# 複数系列グラフ

AEM Forms 6.5では、複数のシリーズグラフを作成および設定する機能が導入されました。 複数系列グラフは、通常、折れ線グラフ、棒グラフ、縦棒グラフの種類に関連して使用されます。 次のグラフは、複数系列グラフの好例です。 グラフは、3つの異なるミューチュアルファンドで10,000ドルの成長を示しています。 AEM Formsでこの種のグラフを作成して使用するには、適切なフォームデータモデルを作成する必要があります。

![多系列](assets/seriescharts.jfif)

AEM Formsで複数シリーズグラフを作成するには、必要なエンティティとエンティティ間の関連付けを持つ適切なフォームデータモデルを作成する必要があります。 次のスクリーンショットは、エンティティと3つのエンティティ間の関連付けを示しています。 トップレベルでは、FUNDと1対多の関係を持つ「組織」と呼ばれるエンティティが存在します。 次に、FMDエンティティは、Performanceエンティティと1対多の関連を持ちます。

![フォームデータモデル](assets/formdatamodel.jfif)


## 複数シリーズグラフ用のフォームデータモデルの作成

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### 折れ線グラフの設定

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


お使いのシステムでこれをテストするには、次の手順に従ってください。

* [AEMパッケージマネージャーを使用して、MutualFundFactSheet.zipをダウンロードし、インポートします。](assets/mutualfundfactsheet.zip)
* [SeriesChartSampleData.jsonをハードドライブにダウンロードします。](assets/serieschartsampledata.json) これは、グラフの入力に使用されるサンプルデータです。
* [「Formsとドキュメント」に移動します。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 「MutualFundGrowthFactSheet」インタラクティブコミュニケーションテンプレートを緩やかに選択します。
* 「プレビュー」をクリックします。 |サンプルデータをアップロードします。
* この記事の一部として提供されているサンプルデータファイルを参照します。
* 前の手順でダウンロードしたサンプルデータを使用して、「MutualFundGrowthFactSheet」インタラクティブ通信の印刷チャネルをプレビューします。
