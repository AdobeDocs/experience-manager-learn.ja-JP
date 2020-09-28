---
title: AEMワークフロー内の変数[パート2]
seo-title: AEMワークフロー内の変数[パート2]
description: aemワークフローでのxml,json,arraylist,ドキュメント型の変数の使用
seo-description: aemワークフローでのxml,json,arraylist,ドキュメント型の変数の使用
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# AEMワークフローのJSON型の変数

AEM Forms6.5以降、AEMワークフローでJSON型の変数を作成できるようになりました。 AEMワークフローにJSONスキーマに基づくアダプティブFormsを送信する場合、またはフォームデータモデルの呼び出し操作の結果を保存する場合は、通常、JSON型の変数を作成します。 次のビデオでは、AEMワークフローでJSON型の変数を作成して使用するために必要な手順について説明します
>[!NOTE]

**AEM Forms6.5.0を使用している場合**

ワークフローモデルで送信されたデータを取り込むためのJSON型の変数を作成する場合は、JSONスキーマを変数に関連付けないでください。 これは、JSONスキーマベースのアダプティブフォームを送信する場合、送信されたデータがJSONスキーマに準拠していないためです。 JSONスキーマの準拠データは、afData.afBoundData.data要素に含まれています。

**AEM Forms6.5.1以降を使用する場合**

ワークフローモデルでJSON型の変数を使用してスキーマをマッピングできます。 その後、スキーマブラウザーを使用して、ワークフローモデル内の文字列/数値変数でスキーマ要素をマッピングできます

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)

**スキーマ要素をドリルダウンし、スキーマ要素をワークフロー変数にマップする機能は、AEM Forms6.5.1以降でのみ使用できます。**

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

システム上でアセットを動作させるには、次の手順に従います。

* [パッケージマネージャーを使用して、アセットをダウンロードし、AEMに読み込みます](assets/jsonandstringvariable.zip)
* [ワークフローモデルを調べ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) 、ワークフローで使用される変数を理解します。
* [電子メールサービスの設定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [アダプティブフォームを開く](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* 詳細を入力し、フォームを送信します
