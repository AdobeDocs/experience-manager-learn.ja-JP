---
title: AEM Formsでの PDFG の使用
description: AEM Formsを使用してPDFを作成するドラッグ&ドロップ機能のデモ
feature: PDF Generator
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 3%

---

# AEM Formsでの PDFG の使用{#using-pdfg-in-aem-forms}

AEM Formsを使用してPDFを作成するドラッグ&amp;ドロップ機能のデモ

PDFG は、PDF生成を表します。 つまり、様々なファイル形式をPDFに変換できます。 最も一般的なものはMicrosoft Office ドキュメントです。 PDFG は 6.1 以降、AEM Formsに含まれています。
[PDFG API の Javadoc は次のとおりです](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

この記事に関連付けられたJPGでは、MS Office ドキュメントまたはアセットファイルをHTMLページのドロップゾーンにドラッグ&amp;ドロップできます。 ドキュメントがドロップされると、PDFG サービスが呼び出され、ドキュメントがPDFに変換されて、AEM Server のファイルシステムに保存されます。

デモアセットをインストールするには、次の手順を実行してください

1. このドキュメントで説明するように PDFG を設定 [ここ](https://helpx.adobe.com/jp/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. ご使用のAEM Formsバージョンに関連する適切なドキュメントに従ってください。
1. [パッケージマネージャーを使用して、この記事に関連するアセットを読み込んでインストールします。](assets/createpdfgdemov2.zip)
1. [post.jsp に移動します。](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) CRX 内
1. 保存場所を好みに応じて変更します（9 行目）。
1. 変更を保存します。
1. を開きます。 [html ページ](http://localhost:4502/content/DocumentServices/CreatePDFG.html) をクリックして、変換するファイルをドラッグ&amp;ドロップします。
1. Word ファイルまたは jpg をドロップゾーンにドロップします。
1. 入力ドキュメントはPDFに変換され、ポイント 4 で指定した場所に保存されます。

次のコードスニペットに、ファイルをPDFに変換する PDFG サービスの使用方法を示します

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
