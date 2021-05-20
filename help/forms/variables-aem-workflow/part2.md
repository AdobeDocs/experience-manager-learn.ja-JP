---
title: AEM Workflowの変数[Part2]
seo-title: AEM Workflowの変数[Part2]
description: aemワークフローでのxml,json,arraylist,document型の変数の使用
seo-description: aemワークフローでのxml,json,arraylist,document型の変数の使用
feature: ワークフロー
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# AEM WorkflowでのJSONタイプの変数

AEM Forms 6.5以降では、AEMワークフローでJSONタイプの変数を作成できるようになりました。 通常は、JSONスキーマに基づくアダプティブFormsをAEMワークフローに送信する場合や、フォームデータモデルの呼び出し操作の結果を保存する場合に、JSONタイプの変数を作成します。 次のビデオでは、AEMワークフローでJSON型の変数を作成して使用するために必要な手順について説明します

**AEM Forms 6.5.0を使用している場合**

JSON型の変数を作成してワークフローモデルに送信されたデータを取り込む場合は、JSONスキーマを変数に関連付けないでください。 これは、JSONスキーマベースのアダプティブフォームを送信すると、送信されたデータがJSONスキーマに準拠しないためです。 JSONスキーマ苦情データは、 afData.afBoundData.data要素で囲まれます。

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**AEM Forms 6.5.1以降を使用する場合**

ワークフローモデルで、スキーマをJSON型の変数にマッピングできます。 その後、スキーマブラウザーを使用して、ワークフローモデル内の文字列/数値変数にスキーマ要素をマッピングできます

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

システム上でアセットを動作させるには、次の手順に従います。

* [パッケージマネージャーを使用したAEMへのアセットのダウンロードと読み込み](assets/jsonandstringvariable.zip)
* [ワークフローモ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) デルを調べて、ワークフローで使用される変数を理解する
* [電子メールサービスの設定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [アダプティブフォームを開く](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* 詳細を入力し、フォームを送信します
