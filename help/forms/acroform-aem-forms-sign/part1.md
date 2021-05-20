---
title: AEM Formsを使用したAcroforms
seo-title: アダプティブフォームデータとAcroformの結合
description: 第1部は、AcroformsとAEM Formsの統合です。 Acroformを使用したアダプティブフォームの作成とデータの結合によるPDFの取得
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 3%

---


# Acroformの作成

Acroformsは、Acrobatを使用して作成されたフォームです。 Acrobatを使用して新しいフォームを最初から作成するか、Microsoft Wordで作成した既存のフォームを使用し、Acrobatを使用してAcroformに変換することができます。 Microsoft Wordで作成したフォームをAcroformに変換するには、次の手順を実行する必要があります。

* Acrobatを使用してWord文書を開く
* Acrobatのフォーム準備ツールを使用して、フォーム上のフォームフィールドを特定します。
* PDFを保存します。 ファイル名にスペースが含まれていないことを確認します。


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>Adobe Signを使用した署名用に入力可能なacroformを送信する場合は、フィールドに適宜名前を付けてください。 例えば、フィールドに&#x200B;**Sig_es_:signer1:signature**&#x200B;と名前を付けることができます。 これは、Adobe Signが理解できる構文です。

>[!NOTE]
>
>XFAベースのドキュメントを送信する場合は、ドキュメントを統合する必要があります。Adobe Signの署名タグは、ドキュメント内に静的テキストとして存在する必要があります。

[Adobe Sign Text Tags Document](https://helpx.adobe.com/jp/sign/using/text-tag.html)

>[!NOTE]
>
>acroformファイル名にスペースが含まれていないことを確認します。 現在のサンプルコードでは、スペースは処理されません。
>
>フォームフィールド名には、次の項目のみを含めることができます。
>
>* 単一スペース
>* 単一アンダースコア
>* 英数字

