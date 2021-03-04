---
title: 'アダプティブフォームの表の入力 '
seo-title: アダプティブフォームの表の入力
description: アダプティブフォームの表へのForm Data Model Service呼び出しの結果の入力
seo-description: アダプティブフォームの表へのForm Data Model Service呼び出しの結果の入力
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---


# Form Data Model Service Invocationの結果をアダプティブフォームの表に入力する

[ライブフォームは](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
ここでホストされています。この記事では、フォームデータモデルサービスの呼び出しからデータを取得し、アダプティブフォームのテーブルに入力する方法について説明します。定期的な住宅ローンの支払いを時間の経過と共にリストする表に、償却スケジュールを作成します。 償却結果は、フォームデータモデルによって返されます。 スクリーンショットに示すように、calculateボタンのclickイベント時にForm Data Modelのサービスが呼び出されます。 サービス呼び出しの入力パラメーターと出力パラメーターは、スクリーンショットに示すように適切にマッピングされます。 出力はRow1の列にマップされます
![clickevent](assets/amortization.PNG)

Row1は、サービス呼び出しによって返されるデータに応じて拡大されるように設定されます。 ここで繰り返しの設定を指定します。 -1を指定すると、テーブルの行数に制限はありません
![Row1](assets/rowconfiguration.PNG)

## これをサーバーに展開する

[Tomcatのインストール](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[ここで指定するSampleRest.war](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
[ファイルのデプロイAEMパッケージマネージャを ](assets/amortizationschedule.zip) 使用してアセットをインストールする償却スケジュールフォームを
[](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
開く適切な値を入力し、「計算計画」をクリックしてフォームに入力します

