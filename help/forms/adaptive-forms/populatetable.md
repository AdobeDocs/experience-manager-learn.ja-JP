---
title: 'アダプティブフォームの表の入力 '
seo-title: アダプティブフォームの表の入力
description: アダプティブフォームの表へのForm Data Model Service呼び出しの結果の入力
seo-description: アダプティブフォームの表へのForm Data Model Service呼び出しの結果の入力
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---


# Form Data Model Service Invocationの結果をアダプティブフォームの表に入力する

[ライブフォームはここでホストされ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)ます。この記事では、フォームデータモデルサービスの呼び出しからデータを取得して、アダプティブフォームのテーブルに入力する方法について説明します。 定期的な住宅ローンの支払いを時間の経過と共にリストする表に、償却スケジュールを作成します。 償却結果は、フォームデータモデルによって返されます。 スクリーンショットに示すように、calculateボタンのclickイベント時にForm Data Modelのサービスが呼び出されます。 サービス呼び出しの入力パラメーターと出力パラメーターは、スクリーンショットに示すように適切にマッピングされます。 出力は、Row1![clickeventの列にマップされます](assets/amortization.PNG)

Row1は、サービス呼び出しによって返されるデータに応じて拡大されるように設定されます。 ここで繰り返しの設定を指定します。 -1を指定すると、テーブル![Row1の行数に制限はありません](assets/rowconfiguration.PNG)

## これをサーバーに展開する

[ここで指定したTomcatのインストールSampleRest](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md).warファイルの[デプロイAEM package managerを使用してアセットを](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)[インストールするアモリゼーションスケジュールフォームを ](assets/amortizationschedule.zip) 開く適切な値を[](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)入力し、「calculateAmortization Schedule」をクリックして、フォームに入力します。

