---
title: AEM FormsでのPDFGの使用
seo-title: AEM FormsでのPDFGの使用
description: AEM Formsを使用したPDF作成のドラッグ&ドロップ機能のデモ
seo-description: AEM Formsを使用したPDF作成のドラッグ&ドロップ機能のデモ
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: pdf-generator
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
topic: 開発
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 3%

---


# AEM Forms{#using-pdfg-in-aem-forms}でのPDFGの使用

AEM Formsを使用したPDF作成のドラッグ&amp;ドロップ機能のデモ

PDFGは、PDF生成の略です。 これは、様々なファイル形式をPDFに変換できることを意味します。 最も一般的なドキュメントはMicrosoft Officeドキュメントです。 PDFGは6.1以降AEM Formsに含まれています。
[PDFG APIのJavadocは、次のリストに記載されています。](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

この記事に関連付けられたアセットを使用して、MS OfficeドキュメントまたはJPGファイルをHTMLページのドロップゾーンにドラッグ&amp;ドロップできます。 ドキュメントがドロップされると、PDFGサービスが呼び出され、ドキュメントがPDFに変換されてAEM Serverのファイルシステムに保存されます。

デモアセットをインストールするには、次の手順を実行します

1. このドキュメント[ここ](https://helpx.adobe.com/jp/experience-manager/6-4/forms/using/install-configure-pdf-generator.html)で説明するようにPDFGを設定します。
1. ご使用のAEM Formsバージョンに関連する適切なドキュメントに従ってください。
1. [パッケージマネージャーを使用して、この記事に関連するアセットを読み込み、インストールします。](assets/createpdfgdemov2.zip)
1. [CRXのpost.jspinに移](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) 動します。
1. 保存場所を好みに応じて変更します（9行目）。
1. 変更を保存します。
1. [ htmlページ](http://localhost:4502/content/DocumentServices/CreatePDFG.html)を開き、変換用のファイルをドラッグ&amp;ドロップします。
1. Wordファイルまたはjpgをドロップゾーンにドロップします。
1. 入力ドキュメントはPDFに変換され、ポイント4で指定した場所に保存されます。

次のコードスニペットは、ファイルをPDFに変換するためのPDFGサービスの使用方法を示しています

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

