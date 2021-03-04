---
title: AEM FormsでのPDFGの使用
seo-title: AEM FormsでのPDFGの使用
description: AEM Formsを使用したドラッグ&ドロップ機能によるPDF作成のデモ
seo-description: AEM Formsを使用したドラッグ&ドロップ機能によるPDF作成のデモ
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: pdf-generator
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
topic: 開発
role: デベロッパー
level: 中間
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 3%

---


# AEM FormsでのPDFGの使用{#using-pdfg-in-aem-forms}

AEM Formsを使用したドラッグ&amp;ドロップ機能によるPDF作成のデモ

PDFGは、PDF Generationの略です。 これは、様々なファイル形式をPDFに変換できることを意味します。 最も一般的なのはMicrosoft Officeドキュメントです。 PDFGは6.1以降AEM Formsに属しています。
[PDFG APIのJavadocは、ここにリスト](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

この記事に関連付けられたアセットを使用して、MS OfficeドキュメントまたはJPGファイルをHTMLページのドロップゾーンにドラッグ&amp;ドロップできます。 ドキュメントが削除されると、PDFGサービスが呼び出され、ドキュメントがPDFに変換されてAEM Serverのファイルシステムに保存されます。

デモアセットをインストールするには、次の手順を実行してください

1. このドキュメント[ここ](https://helpx.adobe.com/jp/experience-manager/6-4/forms/using/install-configure-pdf-generator.html)に記載されているようにPDFGを設定します。
1. ご使用のAEM Forms版に関する適切な文書に従ってください。
1. [パッケージマネージャーを使用して、この記事に関連するアセットを読み込んでインストールします。](assets/createpdfgdemov2.zip)
1. [CRXのpost.](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) jspinに移動します。
1. 希望に応じて保存場所を変更します（9行目）。
1. 変更を保存します。
1. 変換するファイルをドラッグ&amp;ドロップするには、[ htmlページ](http://localhost:4502/content/DocumentServices/CreatePDFG.html)を開きます。
1. Wordファイルまたはjpgをドロップゾーンにドロップします。
1. 入力ドキュメントはPDFに変換され、ポイント4で指定した場所に保存されます。

次のコードスニペットに、PDFGサービスを使用してファイルをPDFに変換する方法を示します

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

