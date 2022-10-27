---
title: アダプティブフォームテーブルの入力
description: フォームデータモデルサービスの呼び出しの結果をアダプティブフォームの表に入力する
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# フォームデータモデルサービスの呼び出しの結果をアダプティブフォームの表に入力する

[ライブフォームはここでホストされます](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
この記事では、フォームデータモデルサービスの呼び出しからデータを取得して、アダプティブフォームのテーブルにデータを入力する方法について説明します。 定期的な住宅ローンの支払いを時間の経過と共に記載した表で償却スケジュールを作成します。 償却の結果はフォームデータモデルから返されます。 フォームデータモデルのサービスは、スクリーンショットに示すように、click イベントの calculate ボタンで呼び出されます。 サービス呼び出しの入力パラメーターと出力パラメーターは、スクリーンショットに示すように適切にマッピングされます。 出力は、Row1 の列にマッピングされます
![clickevent](assets/amortization.PNG)

Row1 は、サービス呼び出しによって返されるデータに応じて増えるように設定されます。 ここで指定した繰り返し設定に注意してください。 -1 の値は、テーブルの行数を無制限に示します。
![Row1](assets/rowconfiguration.PNG)

## サーバーにデプロイ

[ここで指定した Tomcat をインストールします](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[この zip ファイルに含まれる SampleRest.war ファイルを Tomcat にデプロイします。](assets/sample-rest.zip)
[アセットのインストール ](assets/amortizationschedule.zip) AEM package manager の使用
[償却スケジュール・フォームを開きます。](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
適切な値を入力し、フォームに入力する「償却スケジュールの計算」をクリックします
