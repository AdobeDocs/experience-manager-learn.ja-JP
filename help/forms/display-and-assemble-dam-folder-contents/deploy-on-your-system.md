---
title: アセットをローカルにデプロイ
description: ローカルの AEM インスタンスへのチュートリアルアセットのデプロイ
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
duration: 44
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 100%

---

# システムにデプロイ

ローカルの AEM インスタンスでこのユースケースを機能させるには、以下に示す手順に従ってください。

* zip ファイルに含まれている [DevelopingWithServiceUser バンドルをデプロイします](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)。

* [configMgr](http://localhost:4502/system/console/configMgr)を使用して、Apache Sling Service User Mapper Service にエントリ **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** を追加します。

* [ニュースレターバンドルをデプロイします](assets/Newsletters.core-1.0.0-SNAPSHOT.jar)。このバンドルには、フォルダーのコンテンツをリストし、選択したニュースレターをアセンブルするためのコードが含まれています。

* [パッケージマネージャーを使用してパッケージを読み込みます](assets/newsletter.zip)。このパッケージには、ソリューションをテストするクライアントライブラリとサンプル PDF ファイルが含まれています。

* [サンプルアダプティブフォームを読み込みます](assets/sample-adaptive-form.zip)。このフォームは、選択可能なニュースレターのリストを表示します。

* [フォームをプレビューします](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled)。
ダウンロードするニュースレターを 2 つ選択します。選択したニュースレターは 1 つの PDF に結合されて、返されます。
