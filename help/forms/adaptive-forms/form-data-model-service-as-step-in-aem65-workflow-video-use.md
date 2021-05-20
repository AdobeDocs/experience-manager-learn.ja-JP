---
title: AEM 6.5ワークフローでのフォームデータモデルサービスの手順としての使用
seo-title: AEM 6.5ワークフローでのフォームデータモデルサービスの手順としての使用
description: AEM Forms 6.5では、AEMワークフローで変数を作成する機能が導入されました。 この新しい機能により、AEMワークフローで「フォームデータモデルサービスを起動」を使用することが非常に簡単になりました。 次のビデオでは、AEMワークフローで「フォームデータモデルを起動」サービスを使用する手順について説明します。
seo-description: AEM Forms 6.5では、AEMワークフローで変数を作成する機能が導入されました。 この新しい機能により、AEMワークフローで「フォームデータモデルサービスを起動」を使用することが非常に簡単になりました。 次のビデオでは、AEMワークフローで「フォームデータモデルを起動」サービスを使用する手順について説明します。
feature: ワークフロー
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
topic: 開発
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---


# AEM 6.5 Workflow {#using-form-data-model-service-as-step-in-workflow}でのフォームデータモデルサービスの手順としての使用

AEM Forms 6.4以降では、フォームデータモデルサービスをAEM Workflowの一部として使用できるようになりました。 次のビデオでは、AEM Workflowでフォームデータモデルを設定するために必要な手順について説明します

>!![NOTE]このビデオで示される機能には、AEM Forms 6.5.1が必要です。


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

この機能をサーバーでテストするには、次の手順に従います

* [ここ](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)で説明するように、SampleRest.warファイルを使用してtomcatを設定します。Tomcatにデプロイされたwarファイルには、申込者のクレジットスコアを返すコードが含まれています。クレジットスコアは200から800の間の乱数です

* [ パッケージマネージャーを使用したAEMへのアセットの読み込み](assets/aem65-loanapplication.zip)
* パッケージには次の内容が含まれます。

   * FDMステップを使用するワークフローモデル。
   * FDM手順で使用されるフォームデータモデル。
   * 送信時にワークフローをトリガーするアダプティブフォーム。
* [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)を開きます。 詳細を入力し、送信します。 フォームの送信時に、[loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)がトリガーされます。

![ ワークフロー ](assets/invokefdm651.PNG).
クレジットスコアが500を超える場合、ワークフローは、 OR分割コンポーネントを使用してアプリケーションを管理者にルーティングします。 クレジットスコアが500未満の場合、申し込みはキャバリにルーティングされます。
