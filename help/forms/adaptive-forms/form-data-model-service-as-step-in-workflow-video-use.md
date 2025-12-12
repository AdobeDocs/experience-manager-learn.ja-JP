---
title: ワークフローのステップとしてのフォームデータモデルサービスの使用
description: AEM Forms 6.4 以降では、フォームデータモデルを AEM ワークフローの一部として使用できるようになりました。次のビデオでは、AEM ワークフローでフォームデータモデルステップを設定するのに必要な手順を説明しています。
feature: Workflow
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
duration: 205
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 93%

---

# ワークフローのステップとしてのフォームデータモデルサービスの使用 {#using-form-data-model-service-as-step-in-workflow}

AEM Forms 6.4 以降では、フォームデータモデルを AEM ワークフローの一部として使用できるようになりました。次のビデオでは、AEM ワークフローでフォームデータモデルステップを設定するのに必要な手順を説明しています。


>[!VIDEO](https://video.tv.adobe.com/v/330118?captions=jpn&quality=12&learn=on)

お使いのサーバーでこの機能をテストするには、次の手順に従ってください。

* [setvalue バンドルをダウンロードしデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。これは、メタデータプロパティを設定するカスタム OSGi バンドルです。

  >[!NOTE]
  >
  >AEM Forms 6.5 以降では、[&#x200B; ここで説明 &#x200B;](form-data-model-service-as-step-in-aem65-workflow-video-use.md) されているように、この機能は標準で提供されています。

* [こちら](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html?lang=ja)の説明に従って、SampleRest.war ファイルを使用して Tomcat をセットアップします。Tomcat にデプロイされた war ファイルには、申込者の信用スコアを返すコードが含まれています。信用スコアは 200～800 の乱数になります。

* [パッケージマネージャーを使用して、アセットを AEM に読み込みます](assets/invoke-fdm-as-service-step.zip)。パッケージには次のものが含まれています。

   * FDM ステップを使用するワークフローモデル。
   * FDM ステップで使用されるフォームデータモデル。
   * 送信時にワークフローをトリガーするためのアダプティブフォーム。
* [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled) を開きます。詳細を入力して送信します。 フォーム送信時に、[loanapplication ワークフロー](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)がトリガーされます。

![&#x200B; ワークフロー &#x200B;](assets/fdm-as-service-step-workflow.PNG).

信用スコアが 500 を超える場合、ワークフローは OR 分割コンポーネントを利用して、アプリケーションを管理者にルーティングします。 信用スコアが 500 未満の場合、申し込みは cavery にルーティングされます。
