---
title: ソリューションをテストする
description: ExecuteAssemblerService.java を実行してソリューションをテストします。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---

# Eclipse プロジェクトを読み込む

* をダウンロードして展開します。 [zip ファイル](./assets/pdf-manipulation.zip)
* Eclipse を起動し、プロジェクトを Eclipse に読み込みます。
* このプロジェクトには、リソースフォルダー内の次のフォルダーが含まれます。
   * ddxFiles — このフォルダーには、生成する出力を記述する ddx ファイルが格納されます
   * pdffiles — このフォルダーには、アセンブリ対象の pdf ファイルが格納されます

![resources-file](./assets/resources.png)

## ソリューションをテストする

* サービスの資格情報を、プロジェクトの service_token.json リソースファイルにコピーして貼り付けます。
* AssemblePDFFiles.java ファイルを開き、生成されたPDFファイルを保存するフォルダを指定します
* ExecuteAssemblerService.java を開きます。 変数 assembleURL の値を、インスタンスを指すように設定します。
* ExecuteAssemblerService.java を Java アプリケーションとして実行します。

>[!NOTE]
> 初めて java プログラムを実行すると、HTTP 403 エラーが発生します。 これを通過するには、必ず [AEMのテクニカルアカウントユーザーに対する適切な権限](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**AEM Forms Users** 私がこのコースで使った役割です。
