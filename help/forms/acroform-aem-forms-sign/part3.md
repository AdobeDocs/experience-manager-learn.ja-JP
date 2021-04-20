---
title: AEM FormsとのAcroforms
seo-title: アダプティブフォームデータとAcroformの結合
description: AcroformsとAEM Formsの統合のチュートリアルのパート3。 ワークフローとアダプティブフォームをシステムでテストします。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---


# お使いのシステムでこの機能をテストしてください

[このパッケージをダウンロードして](assets/acro-form-aem-form.zip)
AEMTに読み込みます。このパッケージには、サンプルワークフローと、アップロードしたAcroformからスキーマを作成できるhtmlページが含まれています。

## ワークフローの設定

1. [ワークフローモデルを編集モードで開きます](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html)。
2. MergeAcroformDataの手順の設定プロパティを開きます。
3. 「プロセス」タブをクリックします。
4. 渡す引数がサーバー上の有効なフォルダーであることを確認します。
5. 変更内容を保存します。

## アダプティブフォームの作成

1. 前の手順で作成したスキーマを使用して、アダプティブフォームを作成します。
2. いくつかのスキーマ要素をアダプティブフォームにドラッグ&amp;ドロップします。
3. AEMワークフローに送信するアダプティブフォームの送信アクションを設定(MergeAcroformData)
4. **データファイルのパスは必ず「Data.xml」と指定してください。これは、サンプルコードがワークフローのペイロードでData.xmlというファイルを探すので、非常に重要です。**
5. プレビューアダプティブフォームで、フォームに入力して送信します。
6. 設定ワークフローの手順4で指定したフォルダーにデータがマージされたPDFが表示されます

>[!NOTE]
>
>データとacroformを結合して生成されたpdfは、ワークフローのペイロードフォルダーの下のpdfdocument.pdfとして保存されます。 このドキュメントは、ワークフローの一部として、さらに処理するために使用できます
