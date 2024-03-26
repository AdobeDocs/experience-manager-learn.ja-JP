---
title: Acroforms と AEM Forms の連携
description: Acroform と AEM Forms の統合の第 1 部です。Acroform を使用してアダプティブフォームを作成し、データを結合して PDF を取得する方法を説明します。
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 156
source-git-commit: 4f196539ea73d25b480064f7fc349f0ea29d5e0a
workflow-type: ht
source-wordcount: '216'
ht-degree: 100%

---


# Acroform の作成

Acroforms は、Acrobat を使用して作成されたフォームです。 Acrobat を使用してゼロから新しいフォームを作成したり、Microsoft Word で作成した既存のフォームを Acrobat で Acroform に変換したりできます。Microsoft Word で作成したフォームを Acroform に変換するには、次の手順に従う必要があります。

* Acrobat を使用して Word ドキュメントを開きます。
* Acrobat のフォームを準備ツールを使用して、フォーム上のフォームフィールドを特定します。
* PDF を保存します。ファイル名にスペースが含まれていないことを確認してください。


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=12&learn=on)

>[!NOTE]
>
>入力可能な Acroform を送信して Acrobat Sign で署名する場合は、それに応じてフィールドに名前を付けてください。例えば、フィールドに **`Sig_es_:signer1:signature`** という名前を付けることができます。これは、Acrobat Sign で認識できる構文です。

>[!NOTE]
>
>XFA ベースのドキュメントを送信する場合は、ドキュメントを統合する必要があり、Acrobat Sign の署名タグがドキュメント内に静的テキストとして存在する必要があります。

[Acrobat Sign テキストタグドキュメント](https://helpx.adobe.com/jp/sign/using/text-tag.html)

>[!NOTE]
>
>Acroform ファイル名にスペースが含まれていないことを確認します。現在のサンプルコードではスペースを処理しません。
>
>フォームフィールド名には、次の項目のみを含めることができます。
>
>* 1 つのスペース
>* 1 つのアンダースコア
>* 英数字
