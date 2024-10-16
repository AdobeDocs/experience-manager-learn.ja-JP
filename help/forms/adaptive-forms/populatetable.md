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
duration: 45
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '223'
ht-degree: 100%

---

# フォームデータモデルサービス呼び出しの結果をアダプティブフォームテーブルに入力する方法

[ライブフォームはここでホストされています](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
この記事では、フォームデータモデルサービスの呼び出しからデータを取得して、アダプティブフォームテーブルに入力する方法について説明します。住宅ローンの定期的な支払いを時間の経過と共に記載したテーブルの形式で償却スケジュールを作成します。償却の結果はフォームデータモデルから返されます。 スクリーンショットに示すように、フォームデータモデルのサービスは「計算」ボタンのクリックイベントで呼び出されます。 サービス呼び出しの入力パラメーターと出力パラメーターは、スクリーンショットに示すように適切にマッピングされます。 出力は、Row1 の各列にマッピングされます。
![クリックイベント](assets/amortization.PNG)

Row1 は、サービス呼び出しから返されるデータに応じて増えるように設定されています。 ここで指定している繰り返し設定に注意してください。 値が -1 の場合は、テーブルの行数が無制限であることを示しています。
![Row1](assets/rowconfiguration.PNG)

## お使いのサーバーにこれをデプロイする方法

[ここで指定しているとおりに Tomcat をインストールします](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)。
[この zip ファイルに含まれている SampleRest.war ファイルを Tomcat にデプロイします](assets/sample-rest.zip)。
AEM パッケージマネージャーを使用して[アセットをインストールします](assets/amortizationschedule.zip)。
[償却スケジュールフォームを開きます](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)。
適切な値を入力して、「計算」をクリックします。
計算結果がフォームの償却スケジュールに入力されます。
