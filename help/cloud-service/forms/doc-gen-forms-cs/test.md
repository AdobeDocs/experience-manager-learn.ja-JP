---
title: ソリューションのテスト
description: Main.java を実行してソリューションをテスト
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: f6536af2-e4b8-46ca-9b44-a0eb8f4fdca9
duration: 43
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 100%

---

# Eclipse プロジェクトの読み込み

[zip ファイル](./assets/aem-forms-cs-doc-gen.zip)をダウンロードして展開します。

Eclipse を起動し、プロジェクトを Eclipse に読み込みます。
プロジェクトのリソースフォルダーには以下のファイルが含まれます。

* DataFile1、DataFile2、DataFile3 - テンプレートと結合して最終的な PDF ファイルを生成するサンプルの xml データファイル
* custom_fonts.xdp - XDP テンプレート。
* service_token.json - このファイルの内容は、アカウントに固有の資格情報に置き換える必要があります
* options.json - このファイルで指定するオプションは、API で生成された PDF ファイルのプロパティを設定するために使用されます

![resources-file](./assets/resource-files.png)

## ソリューションのテスト

* サービスの資格情報をコピーして、プロジェクトの service_token.json リソースファイルに貼り付けます。
* DocumentGeneration.java ファイルを開き、生成された PDF ファイルを保存するフォルダーを指定します。
* Main.java を開きます。 変数 postURL の値を、インスタンスをポイントするように設定します。
* Main.java を Java アプリケーションとして実行します。

>[!NOTE]
> 初めて java プログラムを実行すると、HTTP 403 エラーが発生します。 これを解決するには、必ず [AEM のテクニカルアカウントユーザーに適切な権限](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=ja#configure-access-in-aem)を付与する必要があります。

このコースでは、**AEM Forms ユーザー**&#x200B;の役割を使用しました。
