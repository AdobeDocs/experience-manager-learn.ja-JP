---
title: ワークフローでのフォームデータモデルサービスのステップとしての使用
description: AEM Forms 6.4 以降では、フォームデータモデルをAEM Workflow の一部として使用できるようになりました。 次のビデオでは、AEM Workflow でフォームデータモデルの手順を設定するために必要な手順について説明します。
feature: Workflow
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# ワークフローでのフォームデータモデルサービスのステップとしての使用 {#using-form-data-model-service-as-step-in-workflow}

AEM Forms 6.4 以降では、フォームデータモデルをAEM Workflow の一部として使用できるようになりました。 次のビデオでは、AEM Workflow でフォームデータモデルの設定手順に必要な手順について説明します


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

ご使用のサーバーでこの機能をテストするには、次の手順に従ってください。
* [setvalue バンドルをダウンロードしてデプロイします。](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). これは、メタデータプロパティを設定するカスタム OSGi バンドルです。
>!![NOTE]AEM Forms 6.5 以降では、この機能は、 [ここで説明する](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* 説明に従って、SampleRest.war ファイルを使用して Tomcat を設定します。 [ここ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)Tomcat にデプロイされた war ファイルには、申込者のクレジットスコアを返すコードが含まれています。 クレジットスコアは 200 ～ 800 の乱数です

* [パッケージマネージャーを使用してAEMにアセットを読み込む](assets/invoke-fdm-as-service-step.zip)パッケージには、次の内容が含まれます。

   * FDM ステップを使用するワークフローモデル。
   * FDM ステップで使用されるフォームデータモデル。
   * 送信時にワークフローをトリガーするアダプティブフォーム。
* を開きます。 [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). 詳細を入力し、送信します。 フォーム送信時に、 [loanapplication ワークフロー](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) がトリガーされます。

![ ワークフロー ](assets/fdm-as-service-step-workflow.PNG).
クレジットスコアが 500 を超える場合、ワークフローは OR 分割コンポーネントを使用して、アプリケーションを管理者にルーティングします。 クレジットスコアが 500 未満の場合、申し込みはキャバリにルーティングされます
