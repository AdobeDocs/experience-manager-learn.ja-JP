---
title: 'アダプティブフォームの表への入力 '
description: フォームデータモデルサービスの呼び出しの結果をアダプティブフォームの表に入力する
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
source-git-commit: 2b7f0f6c34803672cc57425811db89146b38a70a
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---


# フォームデータモデルサービスの呼び出しの結果をアダプティブフォームの表に入力する

[ライブフォームはここでホ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
ストされています。この記事では、フォームデータモデルサービスの呼び出しからデータを取得してアダプティブフォームのテーブルに入力する方法について説明します。定期的な住宅ローンの支払いを時間の経過と共に記載した表に、償却スケジュールを作成します。 償却の結果は、フォームデータモデルから返されます。 フォームデータモデルのサービスは、スクリーンショットに示すように、「 click 」イベント「 calculate 」ボタンをクリックすると呼び出されます。 サービス呼び出しの入力パラメーターと出力パラメーターは、スクリーンショットに示すように適切にマッピングされます。 出力は、Row1の列にマッピングされます
![clickevent](assets/amortization.PNG)

Row1は、サービス呼び出しで返されるデータに応じて拡大するように設定されます。 ここで指定する繰り返し設定に注意してください。 値を —1に設定すると、テーブルの行数は無制限になります
![Row1](assets/rowconfiguration.PNG)

## サーバーにデプロイします

[ここで指定したTomcatをインス](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[トールこのzipファイルに含まれるSampleRest.warファイルをTomcatにデプロイしま](assets/sample-rest.zip)
[す。 AEMパッケージマネージャーを使用してアセットをインストールしま ](assets/amortizationschedule.zip) 
[](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
す。

