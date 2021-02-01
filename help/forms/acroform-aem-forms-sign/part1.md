---
title: AEM FormsとのAcroforms
seo-title: アダプティブフォームデータとAcroformの結合
description: パート1:AcroformsとAEM Formsの統合 Acroformを使用したアダプティブフォームの作成とデータの結合によるPDFの取得
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 3%

---


# Acroformの作成

Acroformsは、Acrobatを使用して作成されたフォームです。 Acrobatを使用して最初から新しいフォームを作成するか、Microsoft Wordで作成した既存のフォームを使用して、Acrobatを使用してAcroformに変換することができます。 Microsoft Wordで作成したフォームをAcroformに変換するには、次の手順に従う必要があります。

* Acrobatを使用して単語ドキュメントを開く
* Acrobatのフォーム準備ツールを使用して、フォーム上のフォームフィールドを特定します。
* pdfを保存します。 ファイル名にスペースが含まれていないことを確認します。


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>Adobe Signを使用して署名するために入力可能なAcrobatフォームを送信する場合は、それに応じてフィールドに名前を付けてください。 例えば、フィールドに&#x200B;**Sig_es_:signer1:signature**&#x200B;という名前を付けることができます。 これがAdobe Signが理解する構文です。

>[!NOTE]
>
>XFAベースのドキュメントを送信する場合は、ドキュメントを統合する必要があります。また、Adobe Sign署名タグはドキュメント内にスタティックテキストとして存在する必要があります。

[Adobe Signテキストタグドキュメント](https://helpx.adobe.com/jp/sign/using/text-tag.html)

>[!NOTE]
>
>Acroformファイル名にスペースが含まれていないことを確認します。 現在のサンプルコードでは、スペースは処理されません。
>
>フォームフィールド名には、次のみを含めることができます。
>
>* 単一スペース
>* 単一アンダースコア
>* 英数字

