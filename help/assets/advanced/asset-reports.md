---
title: AEM Assetsの資産レポート
description: 'AEM Assetsは、直観的なユーザー操作により、大規模なリポジトリに適したレポートフレームワークを提供します。 '
feature: reports
topics: authoring, operations, performance, metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
kt: 648
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---


# アセットレポート{#using-reports-in-aem-assets}

AEM Assetsは、直観的なユーザー操作により、大規模なリポジトリに適したレポートフレームワークを提供します。

>[!VIDEO](https://video.tv.adobe.com/v/22140/?quality=12&learn=on)

## Microsoft Excelの数式{#excel-formulas}

Microsoft Excelでアセットのサイズ別グラフを生成するには、ビデオで次の数式を使用します。

### アセットサイズのバイトへの正規化{#asset-size-normalization-to-bytes}

```
=IF(RIGHT(D2,2)="KB",
      LEFT(D2,(LEN(D2)-2))*1024,
  IF(RIGHT(D2,2)="MB",
      LEFT(D2,(LEN(D2)-2))*1024*1024,
  IF(RIGHT(D2,2)="GB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024,
  IF(RIGHT(D2,2)="TB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024*1024, 0))))
```

### サイズ別のアセット数{#asset-count-by-size}

#### 200 KB未満{#less-than-kb}

```
=COUNTIFS(E2:E1000,"< 200000")
```

#### 200 KB ～ 500 KB {#kb-to-kb}

```
=COUNTIFS(E2:E1000,">= 200000", E2:E1000,"<= 500000")
```

#### 500 KBを超える{#greater-than-kb}

```
=COUNTIFS(E2:E1000,"> 500000")
```

## その他のリソース{#additional-resources}

[グラフ](./assets/asset-reports/all-assets.xlsx)を含むすべてのアセットのExcelファイルをダウンロード
