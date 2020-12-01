---
title: ワークフローのステップとしてのForm Data Modelサービスの使用
seo-title: ワークフローのステップとしてのForm Data Modelサービスの使用
description: AEM Forms6.4以降では、AEMワークフローの一部としてForm Data Modelを使用できるようになりました。 次のビデオでは、AEMワークフローでフォームデータモデルを設定する手順に必要な手順について説明します。
seo-description: AEM Forms6.4以降では、AEMワークフローの一部としてForm Data Modelを使用できるようになりました。 次のビデオでは、AEMワークフローでフォームデータモデルを設定する手順に必要な手順について説明します。
uuid: ecd5d5aa-01eb-48fb-872f-66c656ae14df.
feature: workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: c442f439-1e5d-4f96-85df-b818c28389ff
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---


# ワークフロー{#using-form-data-model-service-as-step-in-workflow}の手順としてのForm Data Modelサービスの使用

AEM Forms6.4以降では、AEMワークフローの一部としてForm Data Modelを使用できるようになりました。 次のビデオでは、AEMワークフローでフォームデータモデルを設定する手順に必要な手順について説明します


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

この機能をサーバーでテストするには、次の手順に従ってください
* [setvalueバンドルをダウンロードしてデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。メタデータプロパティを設定するカスタムOSGIバンドルです。
>!![NOTE]AEM Forms6.5以降では、以下に [説明するように、この機能をすぐに使用できます。](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* [ここ](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)で説明するようにSampleRest.warファイルを使用してTomcatを設定します。 Tomcatにデプロイされたwarファイルには、申込者のクレジットスコアを返すコードが含まれています。 クレジットスコアは、200 ～ 800の乱数です

* [パッケージマネージャーを使用して、AEMにアセットを読み込みます](assets/invoke-fdm-as-service-step.zip)。パッケージには次の内容が含まれます。

   * FDM手順を使用するワークフロー・モデル。
   * FDMの手順で使用するフォーム・データ・モデル。
   * 送信時にワークフローをトリガーするアダプティブフォーム。
* [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)を開きます。 詳細を入力し、送信します。 フォームの送信時に、[アプリケーションワークフロー](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)がトリガされます。

![ ワークフロー ](assets/fdm-as-service-step-workflow.PNG).
ワークフローは、Or Splitコンポーネントを使用して、クレジットスコアが500を超える場合は、アプリをadminにルーティングします。 クレジットスコアが500未満の場合、アプリはキャバリにルーティングされます
