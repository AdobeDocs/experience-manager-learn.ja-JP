---
title: AEM Workflow の変数 [Part1]
description: AEMワークフローでの XML、JSON、ArrayList、Document 型の変数の使用
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f9782684-3a74-4080-9680-589d3f901617
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 0%

---

# AEM Workflow の XML 変数

XML 型の変数は、通常、XSD ベースのアダプティブフォームがあり、ワークフロー内のアダプティブフォーム送信から値を抽出する場合に使用されます。

次のビデオでは、String 型と XML 型の変数を作成し、ワークフローで使用するために必要な手順について説明します。

XML 変数を使用して、アダプティブフォームに事前にデータを設定したり、アダプティブフォームの送信データをワークフローに保存したりすることができます。

文字列変数は、Xpathing を使用して XML 変数に入力できます。 その後、この文字列変数は通常、「電子メールを送信」コンポーネントで電子メールテンプレートのプレースホルダーを設定するために使用されます

>[!NOTE]
>
>アダプティブフォームが XSD と関連付けられていない場合、要素の値を取得するための XPath は次のようになります。
>
>**/afData/afUnboundData/data/submitterName**

アダプティブフォームのデータは、上記のようにデータ要素の下に保存されます。 **_上記の XPath submitterName は、アダプティブフォームのテキストフィールドの名前です。_**

>[!NOTE]
>
>**AEM Forms 6.5.0**  — ワークフローモデルで送信されたデータを取り込むために XML 型の変数を作成する場合は、XSD を変数に関連付けないでください。 これは、XSD ベースのアダプティブフォームを送信すると、送信されたデータが XSD に準拠していないためです。 XSD 苦情データは、 /afData/afBoundData/要素で囲まれます。
>
>**AEM Forms 6.5.1** - XSD を XML 変数に関連付ける場合、スキーマ要素を参照して変数マッピングを実行できます。 スキーマ要素にバインドされていないフォームデータにはアクセスできません。 スキーマ要素に連結されたデータと連結されていないデータにアクセスする場合は、ワークフロー内でスキーマを XML 変数に連結しないでください。必要なデータを取得するには、適切な XPath 式を使用する必要があります

## XML 変数の作成

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### XML 変数でのスキーマの使用

**XML 変数とスキーマのマッピング。 この機能はAEM Forms 6.5.1 以降で使用します。**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=12&learn=on)

#### 送信メールでの変数の使用

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

お使いのシステムでアセットを動作させるには、次の手順に従ってください。

* [パッケージマネージャーを使用して、アセットをダウンロードし、AEMに読み込みます](assets/xmlandstringvariable.zip)
* [ワークフローモデルを参照](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) ワークフローで使用される変数を理解するには
* [電子メールサービスの設定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [アダプティブフォームを開く](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* 詳細を入力し、フォームを送信します。
