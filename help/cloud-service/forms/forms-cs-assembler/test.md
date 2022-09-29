---
title: Forms Assembler ソリューションのテスト
description: ExecuteAssemblerService.java を実行してソリューションをテストします。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 3%

---

# Eclipse プロジェクトを読み込む

* をダウンロードして展開します。 [zip ファイル](./assets/pdf-manipulation.zip)
* Eclipse を起動し、プロジェクトを Eclipse に読み込みます。
* このプロジェクトには、リソースフォルダー内の次のフォルダーが含まれます。
   * ddxFiles — このフォルダーには、生成する出力を記述する ddx ファイルが格納されます
   * pdffiles — このフォルダーには、アセンブリする pdf ファイルと、PDFA ユーティリティをテストする pdf ファイルが含まれます
   * credentials — このフォルダーには pdfa-options.json ファイルが含まれています

![resources-file](./assets/resources.png)

## アセンブリPDFファイルのテスト

* サービスの資格情報を、プロジェクトの service_token.json リソースファイルにコピーして貼り付けます。
* AssemblePDFFiles.java ファイルを開き、生成されたPDFファイルを保存するフォルダを指定します
* ExecuteAssemblerService.java を開きます。 変数の値を設定 _AEM_FORMS_CS_ をクリックして、インスタンスを指定します。
* 適切な行のコメントを解除して、2 つ以上のPDF・ファイルの組み立てをテストします
* ExecuteAssemblerService.java を Java アプリケーションとして実行します。

### PDFA ユーティリティをテスト

* サービスの資格情報を、プロジェクトの service_token.json リソースファイルにコピーして貼り付けます。
* PDFAUtilities.java ファイルを開き、生成されたフォルダーファイルを保存するPDFーを指定します。
* ExecuteAssemblerService.java を開きます。 変数の値を設定 _AEM_FORMS_CS_ をクリックして、インスタンスを指定します。
* 適切な行のコメントを解除して、PDFA 操作をテストします。
* ExecuteAssemblerService.java を Java アプリケーションとして実行します。



>[!NOTE]
> 初めて java プログラムを実行すると、HTTP 403 エラーが発生します。 これを通過するには、必ず [AEMのテクニカルアカウントユーザーに対する適切な権限](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=ja#aemでのアクセスの設定).

**AEM Forms Users** 私がこのコースで使った役割です。
