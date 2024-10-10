---
title: 様々なタイプの PDF forms とドキュメントについて
description: PDF は実際には一連のファイル形式です。この記事では、フォーム開発者にとって重要で関連性の高い PDF の種類について説明します。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.4, 6.5
feature: PDF Generator
jira: KT-7071
topic: Development
last-substantial-update: 2020-07-07T00:00:00Z
duration: 273
exl-id: ffa9d243-37e5-420c-91dc-86c73a824083
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '1294'
ht-degree: 100%

---

# PDF

Portable Document Format（PDF）は、実際にはファイル形式のファイルです。この記事では、フォーム開発者に最も関連するファイル形式について詳しく説明します。 様々な PDF タイプの技術的な詳細や標準の多くは、進化し、変化しています。 これらの形式と仕様の一部は国際標準化機関（ISO）規格で、一部はアドビが所有する特定の知的財産です。

この記事では、様々なタイプの PDF の作成方法を説明します。 それぞれの使用方法と理由を理解するのに役立ちます。 これらすべてのタイプは、PDF を表示および操作するための主要なクライアントツールである Adobe Acrobat DC で最適に機能します。

Acrobat DC の PDF/A ファイルの例を次に示します。

![PDFA](assets/pdfa-file-in-acrobat.png)

サンプルファイルは[こちらからダウンロード](assets/pdf-file-types.zip)できます

## XML Forms Architecture PDF（XFAPDF）

アドビでは、AEM Forms Designer で作成するインタラクティブで動的な Forms を指すのに XFAPDF フォームという用語を使用します。Forms と、Designer で作成するファイルは、Adobe の XML Forms Architecture（XFA）に基づいています。多くの点で、XFAPDF ファイル形式は、従来の HTML ファイルよりも PDF ファイルに近い形式です。 例えば、次のコードは、XFAPDF ファイル内のシンプルなテキストオブジェクトがどのように表示されるかを示しています。

![テキストフィールド](assets/text-field.JPG)

XFA Forms は XML ベースです。 このよく構造化された柔軟な形式により、AEM Forms Server は Designer ファイルを、従来の PDF、PDF/A、HTML など、様々な形式に変換できます。 Forms の完全な XML 構造は、Designer でレイアウトエディターの「XML ソース」タブを選択すると表示されます。 AEM Forms Designer では、静的 XFA Forms と動的 XFA 2010 を作成できます。

## 静的 PDF

静的 XFA PDF forms のレイアウトは、実行時に変更されませんが、ユーザーに対してインタラクティブにすることができます。 次に、静的 XFA PDF forms の利点を示します。

* 静的 XFA PDF forms のレイアウトは、実行時に変更されませんが、ユーザーに対してインタラクティブにすることができます。
* 静的 Formsは、Acrobat のコメントツールおよびマークアップツールをサポートします。
* 静的 Forms では、Acrobat コメントの読み込みと書き出しが可能です。
* 静的 Forms は、AEM Forms サーバーで実行できる技術であるフォントサブ設定をサポートします。
* 静的 Forms は、最新のブラウザーに付属している組み込みの PDF ビューアを使用してレンダリングできます。

>[!NOTE]
>
> 静的 PDF は、AEM Forms Designer を使用して作成できます。そのためには、XDP を Adobe Static PDF フォームとして保存します



### 動的 Forms

動的 XFA PDF は、実行時にレイアウトを変更できるので、コメント機能とマークアップ機能はサポートされていません。 ただし、動的 XFA PDF には次の利点があります。

* 動的 Forms は、フォームのレイアウトとページ番号を変更するクライアントサイドスクリプトをサポートしています。例えば、Purchase Order.xdp を動的フォームとして保存すると、無限量のデータに対応するように、Purchase Order.xdp が拡大およびページ番号付けされます
* 動的 Forms は実行時にフォームのすべてのプロパティをサポートしますが、静的 Forms はサブセットのみをサポートします

* [静的 PDF forms と動的 PDF forms の違いについては、このドキュメントを参照してください](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/pdf-forms-and-documents.html?lang=ja#:~:text=Dynamic%20forms%20support%20all%20the,forms%20support%20only%20a%20subset)

>[!NOTE]
>
> XDP を Adobe Dynamic XML Form として保存することにより、AEM Forms Designer を使用して動的 PDF を作成できます。

>[!NOTE]
>
> 最新のブラウザーに組み込まれている PDF ビューアを使用して、動的なフォームをレンダリングすることはできません。

### PDF ファイル（従来の PDF）

認証済みドキュメントは、PDF ドキュメントと Forms の受信者に対して、信頼性と整合性をさらに保証します。

最も一般的で広く使用される PDF 形式は、従来の PDF ファイルです。 Acrobat や多くのサードパーティ PDF を使用するなど、従来のツールファイルを作成する方法は様々です。 Acrobat では、従来の PDF ファイルを次の方法で作成できます。 Acrobat をインストールしていない場合、お使いのコンピューターにこれらのオプションが表示されない場合があります。

* デスクトップアプリケーションの印刷ストリームをキャプチャする方法：オーサリングアプリケーションの「印刷」コマンドを選択し、Adobe PDF プリンターアイコンを選択します。 文書の印刷されたコピーの代わりに、文書の PDF ファイルが作成されます
* Microsoft Office アプリケーションで Acrobat PDFMaker プラグインを使用する場合：Acrobat をインストールすると、Adobe PDF メニューが Microsoft Office アプリケーションに追加され、アイコンが Office リボンに追加されます。 これらの追加機能を使用して、Microsoft Office で直接 PDF ファイルを作成できます
* Acrobat Distiller を使用して、Postscript および Encapsulated Postscript（EPS）ファイルを PDF に変換する方法：Distiller は、通常、印刷出力や、Postscript 形式から PDF 形式への変換が必要なその他のワークフローで使用されます
* 内部では、従来の PDF は XFA PDF とは大きく異なります。 従来の PDF は同じ XML 構造を持たず、ファイルの印刷ストリームをキャプチャすることで作成されるので、静的で読み取り専用のファイルです。

認証済みドキュメントは、PDF ドキュメントと Forms の受信者に対して、信頼性と整合性をさらに保証します。

### AcroForms

AcroForms は、アドビの古いインタラクティブフォームテクノロジーです。Acrobat バージョン 3 にさかのぼります。[Acrobat Forms API リファレンス](assets/FormsAPIReference.pdf)（2003年5月付け）で、アドビはこのテクノロジーの技術的な詳細を提供しています。AcroForms は、次の項目の組み合わせです。


* Form の静的 PDF とグラフィックを定義する従来の PDF。
* Adobe Acrobat プログラムのフォームツールで上部に固定されたインタラクティブなフォームフィールド。これらのフォームツールは、AEM Forms Designer で使用できる小さなサブセットです。

### PDF/A（アーカイブ用 PDF）

PDF/A（アーカイブ用 PDF）は、従来の PDF のドキュメント保管の利点に基づいて構築され、長期的なアーカイブを強化する多くの具体的な詳細を備えています。 従来の PDF ファイル形式は、ドキュメントの長期保管に多くの利点があります。PDF はコンパクトなため、簡単に転送でき、スペースを節約できます。また、構造化された特性により、強力なインデックス作成と検索機能を可能にします。 従来の PDF はメタデータを幅広くサポートし、PDF にはさまざまなコンピュータ環境をサポートしてきた長い歴史があります。

PDFと同様に、PDF/A は ISO 標準仕様です。 これは、AIIM（Association for Information and Image Management）、NPES（National Printing Equipment Association）、および米国裁判所の事務局を含むタスクフォースによって開発されました。PDF/A の仕様の目的は、長期的なアーカイブ形式を提供することなので、多くの PDF 機能が省略され、ファイルを自己完結型にすることができます。 PDF/A ファイルの長期的な再現性を高める仕様に関する重要なポイントを以下に示します。

* すべてのコンテンツがファイルに含まれている必要があり、ハイパーリンク、フォント、ソフトウェアプログラムなど、外部ソースに依存することはありません。
* すべてのフォントが埋め込まれ、電子ドキュメントに対する無制限の使用ライセンスを持つフォントである必要があります。
* JavaScript は使用できません
* 透明度は使用できません
* 暗号化は使用できません
* オーディオおよびビデオコンテンツは使用できません
* カラースペースは、デバイスに依存しない方法で定義する必要があります
* すべてのメタデータは、特定の基準に従う必要があります

### PDF/A ファイルの表示

サンプルファイル内の 2 つのファイルは、同じ Microsoft Word ファイルから作成されました。 1 つは従来の PDF として、もう 1 つは PDF/A ファイルとして作成されました。 Acrobat Professional で次の 2 つのファイルを開きます。

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

ドキュメントは同じように見えますが、PDF/A ファイルが開き、上部に青いバー（のドキュメントを PDF/A モードで表示していることを示す）が表示されます。この青いバーは、Acrobat のドキュメントメッセージバーで、特定の種類の PDF ファイルを開くと表示されます。

![Pdf-img](assets/pdfa-message.png)

ドキュメントメッセージバーには、タスクの完了に役立つ手順と、場合によってはボタンが含まれています。 色分けされ、特殊な種類の PDF（この PDF/A ファイルなど）や認証済みのデジタル署名付き PDF を開くと、青い色が表示されます。 バーは、PDF forms の場合は紫色に変わり、PDF レビューに参加している場合は黄色に変わります。

>[!NOTE]
>
> 「編集を有効にする」をクリックすると、このドキュメントは PDF/A 準拠から除外されます。
