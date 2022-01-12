---
title: ソリューションをテストする
description: Main.java を実行してソリューションをテストします。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: f712e86600ed18aee43187a5fb105324b14b7b89
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---


# Eclipse プロジェクトを読み込む

をダウンロードして展開します。 [zip ファイル](./assets/aem-forms-cs-doc-gen.zip)

Eclipse を起動し、プロジェクトを Eclipse に読み込みます。プロジェクトには、以下のファイルが resources フォルダーに含まれます。

* DataFile1、DataFile2、および DataFile3 — テンプレートと結合して最終的なPDFファイルを生成するサンプルの xml データファイル
* custom_fonts.xdp - XDP テンプレート。
* service_token.json — このファイルの内容を、アカウント固有の資格情報に置き換える必要があります
* options.json — このファイルで指定するオプションは、API で生成されたPDFファイルのプロパティを設定するために使用されます

![resources-file](./assets/resource-files.png)

## ソリューションをテストする

* サービスの資格情報を、プロジェクトの service_token.json リソースファイルにコピーして貼り付けます。
* DocumentGeneration.java ファイルを開き、生成されたフォルダーファイルを保存するPDFーを指定します
* Main.java を開きます。 変数 postURL の値を、インスタンスを指すように設定します。
* Main.java を Java アプリケーションとして実行します。

>[!NOTE]
> 初めて java プログラムを実行すると、HTTP 403 エラーが発生します。 これを通過するには、必ず [AEMのテクニカルアカウントユーザーに対する適切な権限](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**AEM Forms Users** 私がこのコースで使った役割です。

