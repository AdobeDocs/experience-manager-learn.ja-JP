---
title: AEM Formsを使用した Acroforms
seo-title: Merge Adaptive Form data with Acroform
description: 第 1 部は、Acroforms とAEM Formsの統合です。 Acroform を使用してアダプティブフォームを作成し、データを結合してPDFを取得する
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 2%

---


# Acroform の作成

Acroforms は、Acrobatを使用して作成されたフォームです。 Acrobatを使用して最初から新しいフォームを作成したり、Microsoft Word で作成した既存のフォームを使用して、Acrobatを使用して Acroform に変換したりできます。 Microsoft Word で作成したフォームを Acroform に変換するには、次の手順に従う必要があります。

* Acrobatを使用して Word ドキュメントを開く
* Acrobatのフォーム準備ツールを使用して、フォーム上のフォームフィールドを識別します。
* PDF を保存します。 ファイル名にスペースが含まれていないことを確認してください。


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=12&learn=on)

>[!NOTE]
>
>Acrobat Signを使用した署名用に入力可能な acroform を送信する場合は、フィールドに適宜名前を付けてください。 例えば、フィールドに **Sig_es_:signer1:署名**. これは、Acrobat Signが理解できる構文です。

>[!NOTE]
>
>XFA ベースのドキュメントを送信する場合は、統合したドキュメントを送信し、ドキュメント内に静的テキストとしてAcrobat Signの署名タグを配置する必要があります。

[Acrobat Sign Text Tags Document](https://helpx.adobe.com/jp/sign/using/text-tag.html)

>[!NOTE]
>
>acroform ファイル名にスペースが含まれていないことを確認します。 現在のサンプルコードではスペースを処理しません。
>
>フォームフィールド名には、次の項目のみを含めることができます。
>
>* 1 つのスペース
>* 1 つのアンダースコア
>* 英数字

