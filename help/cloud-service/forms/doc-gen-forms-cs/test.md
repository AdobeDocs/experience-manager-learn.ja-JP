---
title: ソリューションのテスト
description: Main.javaを実行してソリューションをテストする
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: アダプティブフォーム
topic: 開発
source-git-commit: f2a94910fbc29b705f82a66d8248cbcf54366874
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 1%

---


# Eclipseプロジェクトの読み込み

[zipファイル](./assets/aem-forms-doc-gen.zip)をダウンロードして解凍します。

Eclipseを起動し、プロジェクトをEclipseに読み込みます。
このプロジェクトでは、以下のファイルがresourcesフォルダーに含まれます。

* DataFile1とDataFile2 — 最終的なPDFファイルを生成するためにテンプレートとマージされるサンプルXMLデータファイル
* address.xdp - XDPテンプレート
* service_token.json — このファイルの内容を、アカウント固有の資格情報で置き換える必要があります
* options.json — このファイルで指定されるオプションは、APIで生成されたPDFファイルのプロパティを設定するために使用されます

![resources-file](./assets/resource-files.JPG)

## ソリューションのテスト

* サービスの資格情報をコピーして、プロジェクトのservice_token.jsonリソースファイルに貼り付けます。
* DocumentGeneration.javaファイルを開き、生成したPDFファイルを保存するフォルダーを指定します
* Main.javaを開きます。 変数postURLの値を、インスタンスを指すように設定します。
* Main.javaをJavaアプリケーションとして実行する

>[!NOTE]
> Javaプログラムを初めて実行すると、HTTP 403エラーが発生します。 この問題を回避するには、AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem)のテクニカルアカウントユーザーに[適切な権限を付与する必要があります。

**AEM Formsユ** ーザーは、このコースで使用した役割です。

