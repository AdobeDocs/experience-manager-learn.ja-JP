---
title: AEM ワークフローの変数（第 2 部）
description: AEM ワークフローでの XML、JSON、ArrayList、Document タイプの変数の使用
version: 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: e7d3e0be-5194-47c2-a668-ce78e727986e
duration: 354
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '262'
ht-degree: 100%

---

# AEM ワークフローでの JSON タイプの変数

AEM Forms 6.5 以降では、AEM ワークフローで JSON タイプの変数を作成できるようになりました。JSON スキーマに基づくアダプティブフォームを AEM ワークフローに送信する場合や、フォームデータモデルの呼び出し操作の結果を保存する場合は、通常、JSON タイプの変数を作成します。 次のビデオでは、AEM ワークフローで JSON タイプの変数を作成して使用するために必要な手順について説明します

**AEM Forms 6.5.0 を使用する場合**

送信されたデータをワークフローモデルに取り込むために JSON タイプの変数を作成する場合は、その変数に JSON スキーマを関連付けないでください。 これは、JSON スキーマベースのアダプティブフォームを送信する場合、送信されたデータが JSON スキーマに準拠していないからです。 JSON スキーマ準拠のデータは、 afData.afBoundData.data 要素で囲まれます。

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**AEM Forms 6.5.1 以上を使用する場合**

スキーマをワークフローモデルの JSON タイプの変数にマッピングできます。その後、スキーマブラウザーを使用して、スキーマ要素をワークフローモデルの文字列／数値変数にマッピングできます。

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

お使いのシステムでアセットを動作させるには、次の手順に従ってください。

* [パッケージマネージャーを使用して、アセットをダウンロードし AEM に読み込みます。](assets/jsonandstringvariable.zip)
* [ワークフローモデルを調べて](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html)、ワークフローで使用されている変数を理解します。
* [メールサービスを設定します](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html?lang=ja#ConfiguringtheMailService)。
* [アダプティブフォームを開きます](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)。
* 詳細を入力し、フォームを送信します。
