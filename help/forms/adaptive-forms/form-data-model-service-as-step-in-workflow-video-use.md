---
title: ワークフローでのフォームデータモデルサービスの手順としての使用
description: AEM Forms 6.4以降では、フォームデータモデルをAEM Workflowの一部として使用できるようになりました。 次のビデオでは、AEM Workflowでフォームデータモデルを設定するために必要な手順について説明します。
feature: ワークフロー
type: Tutorial
version: 6.4,6.5
topic: 開発
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# ワークフローでのフォームデータモデルサービスの手順としての使用 {#using-form-data-model-service-as-step-in-workflow}

AEM Forms 6.4以降では、フォームデータモデルをAEM Workflowの一部として使用できるようになりました。 次のビデオでは、AEM Workflowでフォームデータモデルを設定するために必要な手順について説明します


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

この機能をサーバーでテストするには、次の手順に従います
* [setvalueバンドルをダウンロードしてデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。これは、メタデータプロパティを設定するカスタムOSGIバンドルです。
>!![NOTE]AEM Forms 6.5以降では、この機能は、次に説明するように標準で使用で [きます。](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* [ここ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)で説明するように、SampleRest.warファイルを使用してtomcatを設定します。Tomcatにデプロイされたwarファイルには、申込者のクレジットスコアを返すコードが含まれています。 クレジットスコアは、200 ～ 800の乱数です

* [パッケージマネージャーを使用してAEMにアセットを読み込みます](assets/invoke-fdm-as-service-step.zip)。パッケージには次の内容が含まれます。

   * FDMステップを使用するワークフローモデル。
   * FDM手順で使用されるフォームデータモデル。
   * 送信時にワークフローをトリガーするアダプティブフォーム。
* [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)を開きます。 詳細を入力し、送信します。 フォームの送信時に、[loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)がトリガーされます。

![ ワークフロー ](assets/fdm-as-service-step-workflow.PNG).
クレジットスコアが500を超える場合、ワークフローは、 OR分割コンポーネントを使用してアプリケーションを管理者にルーティングします。 クレジットスコアが500未満の場合、申し込みはキャバリにルーティングされます
