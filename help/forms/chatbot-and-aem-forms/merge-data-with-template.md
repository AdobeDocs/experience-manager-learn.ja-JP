---
title: データと XDP テンプレートの結合
description: データをテンプレートと結合してPDFドキュメントを作成する
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
source-git-commit: eb4463ae0270725c5b0bd96e9799bada25b06303
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 15%

---

# データと XDP テンプレートの結合

次の手順では、XML データをテンプレートと結合してPDFを生成します。 その後、このPDFは、Adobe Signを使用した署名用に送信されます。

## OutputService を使用したPDFの生成

この [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) PDFの生成には、OutputService のメソッドが使用されました。
その後、生成されたPDFは、Adobe Sign REST API を使用して署名用に送信されます。

