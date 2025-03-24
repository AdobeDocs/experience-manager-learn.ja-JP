---
title: Forms Assembler ソリューションのテスト
description: ExecuteAssemblerService.java の実行とソリューションのテスト
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
duration: 55
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 100%

---

# Eclipse プロジェクトの読み込み

* [zip ファイル](./assets/pdf-manipulation.zip)をダウンロードして展開します。
* Eclipse を起動し、プロジェクトを Eclipse に読み込みます。
* このプロジェクトには、リソースフォルダー内の次のフォルダーが含まれます。
   * ddxFiles - このフォルダーには、生成する出力を記述する ddx ファイルが格納されます
   * pdffiles - このフォルダーには、アセンブリする PDF ファイルと、PDFA ユーティリティをテストする PDF ファイルが含まれます
   * credentials - このフォルダーには pdfa-options.json ファイルが含まれます

![resources-file](./assets/resources.png)

## PDF ファイルのアセンブリテスト

* サービスの資格情報をコピーして、プロジェクトの service_token.json リソースファイルに貼り付けます。
* AssemblePDFFiles.java ファイルを開き、生成された PDF ファイルを保存するフォルダーを指定します。
* ExecuteAssemblerService.java を開きます。 変数 _AEM_FORMS_CS_ の値を設定して、インスタンスを指定します。
* 適切な行のコメントを解除して、2 つ以上の PDF ファイルのアセンブリをテストします
* ExecuteAssemblerService.java を Java アプリケーションとして実行します。

### PDFA ユーティリティのテスト

* サービスの資格情報をコピーして、プロジェクトの service_token.json リソースファイルに貼り付けます。
* PDFAUtilities.java ファイルを開き、生成された PDF ファイルを保存するフォルダーを指定します。
* ExecuteAssemblerService.java を開きます。 変数 _AEM_FORMS_CS_ の値を設定して、インスタンスを指定します。
* 適切な行のコメントを解除して、PDFA 操作をテストします。
* ExecuteAssemblerService.java を Java アプリケーションとして実行します。



>[!NOTE]
> 初めて java プログラムを実行すると、HTTP 403 エラーが発生します。 これを解決するには、必ず [AEM のテクニカルアカウントユーザーに適切な権限](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=ja#configure-access-in-aem)を付与する必要があります。

このコースでは、**AEM Forms ユーザー**&#x200B;の役割を使用しました。
