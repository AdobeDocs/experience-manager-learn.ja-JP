---
title: AEM Formsを使用したAcroforms
seo-title: アダプティブフォームデータとAcroformの結合
description: AcroformsとAEM Formsの統合に関するチュートリアルの第3部。 システム上でワークフローとアダプティブフォームをテストします。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---


# システムでこの機能をテストする

[このパッケージをダウンロードしてAEMTに読み込みま](assets/acro-form-aem-form.zip)
す。このパッケージには、サンプルワークフローと、アップロードされたAcroformからスキーマを作成できるHTMLページが含まれています。

## ワークフローの設定

1. [ワークフローモデルを編集モードで開きます](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html)。
2. 「 MergeAcroformData 」手順の設定プロパティを開きます。
3. 「プロセス」タブをクリックします。
4. 渡す引数がサーバー上の有効なフォルダーであることを確認します。
5. 変更内容を保存します。

## アダプティブフォームの作成

1. 前の手順で作成したスキーマを使用して、アダプティブフォームを作成します。
2. いくつかのスキーマ要素をアダプティブフォームにドラッグ&amp;ドロップします。
3. アダプティブフォームの送信アクションをAEMワークフローに送信するように設定する(MergeAcroformData)。
4. **データファイルのパスを「Data.xml」と指定していることを確認します。これは、サンプルコードがワークフローペイロード内でData.xmlというファイルを探すので、非常に重要です。**
5. アダプティブフォームをプレビューし、フォームに入力して送信します。
6. ワークフローの設定の手順4で指定したフォルダーにデータがマージされたPDFが表示されます。

>[!NOTE]
>
>データをacroformと結合して生成されたPDFは、ワークフローのpayloadフォルダーの下にpdfdocument.pdfとして保存されます。 このドキュメントは、ワークフローの一部として追加の処理に使用できます
