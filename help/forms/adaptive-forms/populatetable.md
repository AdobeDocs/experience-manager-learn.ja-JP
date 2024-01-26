---
title: アダプティブフォームテーブルへの入力
description: フォームデータモデルサービス呼び出しの結果をアダプティブフォームテーブルに入力する方法
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
duration: 57
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 79%

---

# フォームデータモデルサービス呼び出しの結果をアダプティブフォームテーブルに入力する方法

[ライブフォームはここでホストされています](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
この記事では、フォームデータモデルサービスの呼び出しからデータを取得して、アダプティブフォームテーブルに入力する方法について説明します。住宅ローンの定期的な支払いを時間の経過と共に記載したテーブルの形式で償却スケジュールを作成します。償却の結果はフォームデータモデルから返されます。 スクリーンショットに示すように、フォームデータモデルのサービスは「計算」ボタンのクリックイベントで呼び出されます。 サービス呼び出しの入力パラメーターと出力パラメーターは、スクリーンショットに示すように適切にマッピングされます。 出力は、Row1 の各列にマッピングされます。
![クリックイベント](assets/amortization.PNG)

Row1 は、サービス呼び出しから返されるデータに応じて増えるように設定されています。 ここで指定している繰り返し設定に注意してください。 値が -1 の場合は、テーブルの行数が無制限であることを示しています。
![Row1](assets/rowconfiguration.PNG)

## お使いのサーバーにこれをデプロイする方法

[ここで指定した Tomcat をインストールします。](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[この zip ファイルに含まれる SampleRest.war ファイルを Tomcat にデプロイします。](assets/sample-rest.zip)
[アセットのインストール](assets/amortizationschedule.zip) AEM package manager の使用
[償却スケジュール・フォームを開きます。](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
適切な値を入力し、フォームに入力する「償却スケジュールの計算」をクリックします
