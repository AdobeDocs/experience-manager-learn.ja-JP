---
title: AEM 6.5ワークフローでの手順としてのForm Data Model Serviceの使用
seo-title: AEM 6.5ワークフローでの手順としてのForm Data Model Serviceの使用
description: AEM Forms6.5では、AEMワークフローで変数を作成する機能が導入されました。 AEMワークフローの「Invoke Form Data Model Service」を使用するこの新しい機能により、非常に簡単になりました。 次のビデオでは、AEMワークフローのInvoke Form Data Model Serviceの使用に関する手順を説明します。
seo-description: AEM Forms6.5では、AEMワークフローで変数を作成する機能が導入されました。 AEMワークフローの「Invoke Form Data Model Service」を使用するこの新しい機能により、非常に簡単になりました。 次のビデオでは、AEMワークフローのInvoke Form Data Model Serviceの使用に関する手順を説明します。
feature: workflow.
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---


# AEM 6.5ワークフローでの手順としてのForm Data Model Serviceの使用 {#using-form-data-model-service-as-step-in-workflow}

AEM Forms6.4以降では、AEMワークフローの一部としてForm Data Model Serviceを使用できるようになりました。 次のビデオでは、AEMワークフローでフォームデータモデルを設定する手順に必要な手順について説明します

>!![NOTE]このビデオで示す機能には、AEM Forms6.5.1が必要です。


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

この機能をサーバーでテストするには、次の手順に従ってください

* SampleRest.warファイルを使用してTomcatを [設定します](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)。 Tomcatにデプロイされたwarファイルには、申込者のクレジットスコアを返すコードが含まれています。クレジットスコアは、200 ～ 800の乱数です

* [ パッケージマネージャーを使用して、AEMにアセットを読み込みます](assets/aem65-loanapplication.zip)
* パッケージには次の内容が含まれます。

   * FDM手順を使用するワークフロー・モデル。
   * FDMの手順で使用するフォーム・データ・モデル。
   * 送信時にワークフローをトリガーするアダプティブフォーム。
* MortgageApplicationFormを開き [ます](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)。 詳細を入力し、送信します。 フォームの送信時に、 [申込みワークフロー](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) がトリガーされます。

![ ワークフロー ](assets/invokefdm651.PNG).
ワークフローは、Or Splitコンポーネントを使用して、クレジットスコアが500を超える場合は、アプリをadminにルーティングします。 クレジットスコアが500未満の場合、アプリはキャバリにルーティングされます。
