---
title: AEMワークフロー内の変数[Part1]
seo-title: AEMワークフロー内の変数[Part1]
description: aemワークフローでのxml,json,arraylist,ドキュメント型の変数の使用
seo-description: aemワークフローでのxml,json,arraylist,ドキュメント型の変数の使用
feature: ワークフロー
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: 開発
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---


# AEMワークフローのXML変数

XML型の変数は、通常、XSDベースのアダプティブフォームがあり、ワークフローでアダプティブフォーム送信から値を抽出する場合に使用されます。

次のビデオでは、String型の変数とXML型の変数を作成し、ワークフローで使用するために必要な手順について説明します。

XML変数は、アダプティブフォームの事前入力や、アダプティブフォームの送信データのワークフローへの保存に使用できます。

文字列変数は、Xpathingを使用してXML変数に入力できます。 次に、この文字列変数は、通常、「電子メールの送信」コンポーネント内の電子メールテンプレートのプレースホルダーを設定するために使用されます

>[!NOTE]
>
>アダプティブフォームがXSDと関連付けられていない場合、要素の値を取得するXPathは次のようになります。
>
>**/afData/afUnboundData/data/submitterName**

アダプティブフォームのデータは、上に示すように、データ要素の下に保存されます。 **_上記のXPathでは、submitterNameはアダプティブフォームのテキストフィールドの名前です。_**

>[!NOTE]
>
>**AEM Forms6.5.0**  — ワークフローモデルで送信されたデータを取り込むためにXML型の変数を作成する場合は、XSDを変数に関連付けないでください。これは、XSDベースのアダプティブフォームを送信する場合、送信されたデータがXSDに準拠していないためです。 XSD準拠データは、/afData/afBoundData/要素に含まれます。
>
>**AEM Forms6.5.1** - XML変数にXSDを関連付ける場合は、スキーマ要素を参照して、変数のマッピングを行うことができます。スキーマ要素に連結されていないフォームデータにはアクセスできません。 スキーマ要素に連結されたデータと連結されていないデータにアクセスする場合は、スキーマをワークフロー内のXML変数に連結しないでください。必要なデータを取得するには、適切なXPath式を使用する必要があります

## XML変数の作成

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### XML変数でのスキーマの使用

**XML変数とスキーマのマッピングこの機能はAEM Forms6.5.1以降**&#x200B;で使用

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### 電子メールの送信での変数の使用

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

システム上でアセットを動作させるには、次の手順に従います。

* [パッケージマネージャーを使用して、アセットをダウンロードし、AEMに読み込みます](assets/xmlandstringvariable.zip)
* [ワークフロー](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) モデルを調べ、ワークフローで使用される変数を理解します。
* [電子メールサービスの設定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [アダプティブフォームを開く](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* 詳細を入力し、フォームを送信します。

