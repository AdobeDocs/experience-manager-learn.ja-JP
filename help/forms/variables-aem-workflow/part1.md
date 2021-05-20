---
title: AEM Workflowの変数[Part1]
seo-title: AEM Workflowの変数[Part1]
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
source-wordcount: '439'
ht-degree: 0%

---


# AEMワークフローのXML変数

通常、XMLタイプの変数は、XSDベースのアダプティブフォームがあり、ワークフローでアダプティブフォームの送信から値を抽出する場合に使用されます。

次のビデオでは、String型とXML型の変数を作成し、ワークフローで使用するために必要な手順について説明します。

XML変数を使用して、アダプティブフォームを事前入力したり、アダプティブフォームの送信データをワークフローに保存したりすることができます。

文字列変数は、XML変数にXpathingを使用して設定できます。 この文字列変数は、通常、電子メールを送信コンポーネントで電子メールテンプレートのプレースホルダーを設定するために使用されます

>[!NOTE]
>
>アダプティブフォームがXSDに関連付けられていない場合、要素の値を取得するXPathは次のようになります。
>
>**/afData/afUnboundData/data/submitterName**

アダプティブフォームのデータは、上図のように、データ要素の下に保存されます。 **_上記のXPath submitterNameは、アダプティブフォームのテキストフィールドの名前です。_**

>[!NOTE]
>
>**AEM Forms 6.5.0**  - XMLタイプの変数を作成してワークフローモデルに送信されたデータを取り込む場合は、XSDを変数に関連付けないでください。これは、XSDベースのアダプティブフォームを送信すると、送信されたデータがXSDに準拠しないためです。 XSD準拠データは、 /afData/afBoundData/要素に含まれます。
>
>**AEM Forms 6.5.1**  - XML変数にXSDを関連付ける場合は、スキーマ要素を参照して変数マッピングを実行できます。スキーマ要素にバインドされていないフォームデータにはアクセスできません。 使用例が、スキーマ要素に連結されたデータと連結されていないデータにアクセスする場合は、ワークフロー内のXML変数にスキーマを連結しないでください。必要なデータを取得するには、適切なXPath式を使用する必要があります

## XML変数の作成

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### XML変数でのスキーマの使用

**XML変数とスキーマのマッピング。この機能はAEM Forms 6.5.1以降で使用します。**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### Eメール送信での変数の使用

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

システム上でアセットを動作させるには、次の手順に従います。

* [パッケージマネージャーを使用したAEMへのアセットのダウンロードと読み込み](assets/xmlandstringvariable.zip)
* [ワークフローモ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) デルを調べて、ワークフローで使用される変数を理解する
* [電子メールサービスの設定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [アダプティブフォームを開く](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* 詳細を入力し、フォームを送信します。

