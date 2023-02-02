---
title: アセットをローカルにデプロイ
description: ローカルのAEMインスタンスにチュートリアルアセットをデプロイします。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: ddef90067d3ae4a3c6a705b5e109e474bab34f6d
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 5%

---

# システムにデプロイ

以下に示す手順に従って、ローカルのAEMインスタンスでこのユースケースを機能させてください。

* [DevelopingWithServiceUser バンドルのデプロイ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) zip ファイルに含まれています。

* Apache Sling Service User Mapper Service に次のエントリを追加します。 **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** の使用 [configMgr](http://localhost:4502/system/console/configMgr).

* [ニュースレターバンドルをデプロイします](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). このバンドルには、フォルダーのコンテンツをリストし、選択したニュースレターをアセンブルするためのコードが含まれています。

* [パッケージマネージャーを使用してパッケージを読み込みます](assets/newsletter.zip). このパッケージには、ソリューションをテストするクライアントライブラリとサンプル pdf ファイルが含まれています。

* [サンプルのアダプティブフォームを読み込む](assets/sample-adaptive-form.zip). このフォームは、選択可能なニュースレターのリストを表示します。

* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
ダウンロードするニュースレターを 2 つ選択します。選択したニュースレターは 1 つの PDF に結合され、返されます。




