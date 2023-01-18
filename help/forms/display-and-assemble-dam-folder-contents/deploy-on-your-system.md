---
title: アセットをローカルにデプロイ
description: ローカルのAEMインスタンスにチュートリアルアセットをデプロイします。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: 7815b1a78949c433f2c53ff752bf39dd55f9ac94
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 2%

---

# システムにデプロイ

以下に示す手順に従って、ローカルのAEMインスタンスでこのユースケースを機能させてください。

* [この記事に記載されている手順に従って、fd-service ユーザーを使用するようにを設定します。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en). DevelopingWithServiceUser バンドルがデプロイされていることを確認します。

* [ニュースレターバンドルをデプロイします](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). このバンドルには、フォルダーのコンテンツをリストし、選択したニュースレターをアセンブルするためのコードが含まれています。

* [パッケージマネージャーを使用してパッケージを読み込みます](assets/newsletter.zip). このパッケージには、ソリューションをテストするクライアントライブラリとサンプル pdf ファイルが含まれています。

* [サンプルのアダプティブフォームを読み込む](assets/sample-adaptive-form.zip). このフォームは、選択可能なニュースレターのリストを表示します。

* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
ダウンロードするニュースレターを 2 つ選択します。選択したニュースレターは 1 つの PDF に結合され、返されます。




