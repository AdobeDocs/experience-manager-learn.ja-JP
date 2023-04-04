---
title: AEM 6.5 ワークフローでの手順としてのフォームデータモデルサービスの使用
description: AEM Forms 6.5 では、AEMワークフローで変数を作成する機能が導入されました。 この新しい機能により、AEM Workflow で「フォームデータモデルサービスを起動」を使用することが非常に簡単になりました。 次のビデオでは、AEM Workflow での「フォームデータモデルサービスを起動」の使用に関する手順について説明します。
feature: Workflow
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
last-substantial-update: 2021-02-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# AEM 6.5 ワークフローでの手順としてのフォームデータモデルサービスの使用 {#using-form-data-model-service-as-step-in-workflow}

AEM Forms 6.4 以降では、フォームデータモデルサービスをAEM Workflow の一部として使用できるようになりました。 次のビデオでは、AEM Workflow でフォームデータモデルの設定手順に必要な手順について説明します

>!![NOTE]このビデオで示される機能には、AEM Forms 6.5.1 が必要です。


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=12&learn=on)

ご使用のサーバーでこの機能をテストするには、次の手順に従ってください。

* 説明に従って、SampleRest.war ファイルを使用して Tomcat を設定します。 [ここ](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)Tomcat にデプロイされた war ファイルには、申込者のクレジットスコアを返すコードが含まれています。クレジットスコアは、200 から 800 の間の乱数です。

* [ パッケージマネージャーを使用してAEMにアセットを読み込む](assets/aem65-loanapplication.zip)
* パッケージには次の内容が含まれます。

   * FDM ステップを使用するワークフローモデル。
   * FDM ステップで使用されるフォームデータモデル。
   * 送信時にワークフローをトリガーするアダプティブフォーム。
* を開きます。 [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). 詳細を入力し、送信します。 フォーム送信時に、 [loanapplication ワークフロー](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) がトリガーされます。

![ ワークフロー ](assets/invokefdm651.PNG).
クレジットスコアが 500 を超える場合、ワークフローは OR 分割コンポーネントを使用して、アプリケーションを管理者にルーティングします。 クレジットスコアが 500 未満の場合、申し込みはキャバリにルーティングされます。
