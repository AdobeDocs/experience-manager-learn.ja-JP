---
title: AEM 6.5ワークフローでの手順としてのForm Data Model Serviceの使用
seo-title: AEM 6.5ワークフローでの手順としてのForm Data Model Serviceの使用
description: AEM Forms6.5では、AEMワークフローで変数を作成する機能が導入されました。 AEMワークフローの「Invoke Form Data Model Service」を使用するこの新しい機能により、非常に簡単になりました。 次のビデオでは、AEMワークフローのInvoke Form Data Model Serviceの使用に関する手順を説明します。
seo-description: AEM Forms6.5では、AEMワークフローで変数を作成する機能が導入されました。 AEMワークフローの「Invoke Form Data Model Service」を使用するこの新しい機能により、非常に簡単になりました。 次のビデオでは、AEMワークフローのInvoke Form Data Model Serviceの使用に関する手順を説明します。
feature: ワークフロー
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
topic: 開発
role: デベロッパー
level: 中間
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 1%

---


# AEM 6.5ワークフロー{#using-form-data-model-service-as-step-in-workflow}での手順としてのForm Data Model Serviceの使用

AEM Forms6.4以降では、AEMワークフローの一部としてForm Data Model Serviceを使用できるようになりました。 次のビデオでは、AEMワークフローでフォームデータモデルを設定する手順に必要な手順について説明します

>!![NOTE]このビデオで示す機能には、AEM Forms6.5.1が必要です。


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

この機能をサーバーでテストするには、次の手順に従ってください

* [ここ](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)で説明したようにSampleRest.warファイルを使用してTomcatを設定します。 Tomcatにデプロイされたwarファイルには、申込者のクレジットスコアを返すコードが含まれます。クレジットスコアは200 ～ 800の乱数です

* [ パッケージマネージャーを使用して、AEMにアセットを読み込みます](assets/aem65-loanapplication.zip)
* パッケージには次の内容が含まれます。

   * FDM手順を使用するワークフロー・モデル。
   * FDMの手順で使用するフォーム・データ・モデル。
   * 送信時のワークフローをトリガーするアダプティブフォーム。
* [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)を開きます。 詳細を入力し、送信します。 フォームの送信時に、[アプリケーションワークフロー](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)がトリガされます。

![ ワークフロー ](assets/invokefdm651.PNG).
ワークフローは、Or Splitコンポーネントを使用して、クレジットスコアが500を超える場合は、アプリをadminにルーティングします。 クレジットスコアが500未満の場合、アプリはキャバリにルーティングされます。
