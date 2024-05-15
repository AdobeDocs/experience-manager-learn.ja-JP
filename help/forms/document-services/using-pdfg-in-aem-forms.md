---
title: AEM Forms での PDFG の使用
description: AEM Forms を使用して PDF を作成するドラッグ＆ドロップ機能のデモ
feature: PDF Generator
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
last-substantial-update: 2020-07-07T00:00:00Z
duration: 52
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 100%

---

# AEM Forms での PDFG の使用{#using-pdfg-in-aem-forms}

AEM Forms を使用して PDF を作成するドラッグ＆ドロップ機能のデモ

PDFG は、「PDF 生成」を表し、様々なファイル形式を PDF に変換できることを意味します。最も一般的な例は、Microsoft Office ドキュメントです。PDFG は 6.1 以降、AEM Forms に含まれています。
[PDFG API の javadoc はこちらに一覧表示されています](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html?lang=ja)

この記事に関連するアセットを使用すると、MS Office ドキュメントまたは JPG ファイルを HTML ページのドロップゾーンにドラッグ＆ドロップできます。ドキュメントがドロップされると、PDFG サービスが呼び出され、ドキュメントが PDF に変換されて、AEM サーバーのファイルシステムに保存されます。

デモアセットをインストールするには、次の手順を実行します

1. [こちら](https://helpx.adobe.com/jp/experience-manager/6-4/forms/using/install-configure-pdf-generator.html)のドキュメントで説明されているように PDFG を設定します。
1. ご使用の AEM Forms バージョンに関連する適切なドキュメントに従ってください。
1. [パッケージマネージャーを使用して、この記事に関連するアセットを読み込んでインストールします。](assets/createpdfgdemov2.zip)
1. CRX で [post.jsp に移動します](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp)
1. 保存場所を好みに応じて変更します（9 行目）
1. 変更を保存します。
1. [html ページ](http://localhost:4502/content/DocumentServices/CreatePDFG.html)を開き、ファイルをドラッグ＆ドロップして変換します。
1. Word ファイルまたは jpg をドロップゾーンにドロップします。
1. 入力ドキュメントが PDF に変換され、ポイント 4 で指定した場所に保存されます。

次のコードスニペットは、PDFG サービスを使用してファイルを PDF に変換する方法を示しています

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
