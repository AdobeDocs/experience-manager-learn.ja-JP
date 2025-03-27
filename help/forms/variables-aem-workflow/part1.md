---
title: 'AEM ワークフローの変数 [第 1 部] '
description: AEM ワークフローでの XML、JSON、ArrayList、Document タイプの変数の使用
feature: Adaptive Forms, Workflow
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f9782684-3a74-4080-9680-589d3f901617
duration: 561
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '404'
ht-degree: 100%

---

# AEM ワークフローの XML 変数

XML タイプの変数を使用するのは、通常、XSD ベースのアダプティブフォームがあり、ワークフローでそのアダプティブフォームの送信から値を抽出する場合です。

次のビデオでは、文字列タイプと XML タイプの変数を作成してワークフローで使用するために必要な手順を説明します。

XML 変数を使用すると、アダプティブフォームにデータを事前に入力したり、ワークフロー内でアダプティブフォームの送信データを保存したりすることができます。

文字列変数は、Xpathing を使用して XML 変数に入力できます。通常、この文字列変数を使用して、メール送信コンポーネントのメールテンプレートプレースホルダーに値を入力します。

>[!NOTE]
>
>アダプティブフォームが XSD と関連付けられていない場合、要素の値を取得するための XPath は次のようになります。
>
>**/afData/afUnboundData/data/submitterName**

アダプティブフォームのデータは、上記のデータ要素に保存されます。**_上記の XPath で、submitterName はアダプティブフォームのテキストフィールドの名前です。_**

>[!NOTE]
>
>**AEM Forms 6.5.0** - XML タイプの変数を作成して、送信されたデータをワークフローモデルで取り込む場合は、その変数に XSD を関連付けないでください。これは、XSD ベースのアダプティブフォームを送信する場合、送信されたデータが XSD に準拠していないからです。XSD に準拠したデータは、/afData/afBoundData/ 要素で囲まれます。
>
>**AEM Forms 6.5.1** - XSD を XML 変数に関連付ける場合は、スキーマ要素を参照して変数マッピングを実行できます。スキーマ要素にバインドされていないフォームデータにはアクセスできません。スキーマ要素にバインドされたデータに加え、バインドされていないデータにもアクセスする場合は、ワークフロー内でスキーマを XML 変数にバインドしないでください。適切な XPath 式を使用して、必要なデータを取得する必要があります。

## XML 変数の作成

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### XML 変数とスキーマの使用

**XML 変数とスキーマのマッピング。 この機能は、AEM Forms 6.5.1 以降で使用します。**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=12&learn=on)

#### 送信メールでの変数の使用

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

お使いのシステムでアセットを動作させるには、次の手順に従ってください。

* [パッケージマネージャーを使用して、アセットをダウンロードし AEM に読み込みます。](assets/xmlandstringvariable.zip)
* [ワークフローモデルを調べて](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html)、ワークフローで使用されている変数を理解します。
* [メールサービスを設定します](https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)。
* [アダプティブフォームを開きます](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)。
* 詳細を入力し、フォームを送信します。
