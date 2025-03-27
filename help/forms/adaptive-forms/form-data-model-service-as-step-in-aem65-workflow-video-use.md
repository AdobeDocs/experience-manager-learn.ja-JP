---
title: AEM 6.5 ワークフローのステップとしてのフォームデータモデルサービスの使用
description: AEM Forms 6.5 には、AEM ワークフローで変数を作成する機能が導入されています。 この新しい機能により、AEM ワークフローで「フォームデータモデルサービスを呼び出し」を非常に簡単に使用できるようになりました。次のビデオでは、AEM ワークフローで「フォームデータモデルサービスを呼び出し」を使用する際の手順を説明します。
feature: Workflow
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
last-substantial-update: 2021-02-09T00:00:00Z
duration: 239
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '257'
ht-degree: 100%

---

# AEM 6.5 ワークフローのステップとしてのフォームデータモデルサービスの使用 {#using-form-data-model-service-as-step-in-workflow}

AEM Forms 6.4 以降では、フォームデータモデルサービスを AEM ワークフローの一部として使用できるようになりました。 次のビデオでは、AEM ワークフローでフォームデータモデルステップを設定するのに必要な手順を説明しています。

>このビデオで示される機能を使用するには、AEM Forms 6.5.1 が必要です。


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=12&learn=on)

お使いのサーバーでこの機能をテストするには、次の手順に従ってください。

*  [こちら](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)の説明に従って、SampleRest.war ファイルを使用して Tomcat を設定します。Tomcat にデプロイされた war ファイルには、申込者の信用スコアを返すコードが含まれています。信用スコアは、200 から 800 の間の乱数になります。

* [パッケージマネージャーを使用して AEM にアセットを読み込みます。](assets/aem65-loanapplication.zip)
* パッケージの内容は次のとおりです。

   * FDM ステップを使用するワークフローモデル。
   * FDM ステップで使用されるフォームデータモデル。
   * 送信時にワークフローをトリガーするためのアダプティブフォーム。
* [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled) を開きます。詳細を入力して送信します。 フォーム送信時に、[loanapplication ワークフロー](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)がトリガーされます。

![ ワークフロー ](assets/invokefdm651.PNG)
信用スコアが 500 を超える場合、ワークフローは OR 分割コンポーネントを利用して、アプリケーションを管理者にルーティングします。 信用スコアが 500 未満の場合、申し込みは cavery にルーティングされます。
