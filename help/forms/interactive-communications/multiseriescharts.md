---
title: AEM Formsの複数シリーズグラフ
seo-title: AEM Formsの複数シリーズグラフ
description: 印刷およびWebチャネルードキュメントで複数シリーズのグラフを作成する場合は、適切なForm Data Modelを作成します。
seo-description: 印刷およびWebチャネルードキュメントで複数シリーズのグラフを作成する場合は、適切なForm Data Modelを作成します。
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: 開発
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 1%

---


# 複数シリーズのグラフ

AEM Forms6.5では、複数のシリーズグラフを作成および設定する機能が導入されました。 通常、複数のシリーズグラフは、折れ線グラフ、棒グラフ、列グラフの種類と関連付けて使用されます。 次のグラフは、複数系列のグラフの良い例です。 グラフは、ある期間の3つの異なるミューチュアルファンドでの$10,000 USDの増加を示しています。 AEM Formsでこのようなグラフを作成して使用するには、適切なForm Data Modelを作成する必要があります。

![多系列](assets/seriescharts.jfif)

AEM Formsで複数シリーズのグラフを作成するには、必要なエンティティとエンティティ間の関連付けを持つ適切なフォームデータモデルを作成する必要があります。 次のスクリーンショットは、エンティティと3つのエンティティ間の関連付けを示しています。 トップレベルでは、「組織」というエンティティがあり、このエンティティとIMFのエンティティとの間に1対多の関連があります。 同様に、FNDエンティティは、Performanceエンティティと1対多の関連付けを持ちます。

![フォームデータモデル](assets/formdatamodel.jfif)


## 複数シリーズグラフのフォームデータモデルの作成

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### 線系列グラフの設定

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


お使いのシステムでテストするには、次の手順に従ってください

* [AEMパッケージマネージャーを使用して、MutualFundFactSheet.zipをダウンロードし、インポートします。](assets/mutualfundfactsheet.zip)
* [SeriesChartSampleData.jsonをハードドライブにダウンロードします。](assets/serieschartsampledata.json) これは、グラフの入力に使用されるサンプルデータです。
* [Formsとドキュメントに移動します。](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* 「MutualFundGrowthFactSheet」対話型通信テンプレートをそっと選択します。
* プレビューをクリック |サンプルデータのアップロードを参照してください。
* この記事の一部として提供されているサンプルデータファイルを参照します。
* 「MutualFundGrowthFactSheet」対話型通信と前の手順でダウンロードしたサンプルデータの印刷チャネルをプレビューします。
