---
title: データと XDP テンプレートの結合
description: データをテンプレートと結合した PDF ドキュメントの作成
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 6a865402-db3d-4e0e-81a0-a15dace6b7ab
duration: 15
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '80'
ht-degree: 100%

---

# データと XDP テンプレートの結合

次の手順では、XML データをテンプレートと結合して PDF を生成します。その後、この PDF は、Adobe Sign を使用して署名用に送信されます。

## OutputService を使用した PDF の生成

PDF の生成には、OutputService の [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) メソッドが使用されました。
その後、生成した PDF は、Adobe Sign REST API を使用して署名用に送信されます。
