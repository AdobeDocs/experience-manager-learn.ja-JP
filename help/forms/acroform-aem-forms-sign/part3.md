---
title: AEM Formsを使用した Acroforms
seo-title: Merge Adaptive Form data with Acroform
description: Acroforms とAEM Formsの統合に関するチュートリアルの第 3 部。 システム上でワークフローとアダプティブフォームをテストします。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 1%

---


# お使いのシステムでこの機能をテストします。

[このパッケージをダウンロードしてAEMにインポート](assets/acro-form-aem-form.zip)
このパッケージには、サンプルワークフローと、アップロードされた Acroform からスキーマを作成できる html ページが含まれています。

## ワークフローを設定

1. [ワークフローモデルを編集モードで開く](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. MergeAcroformData 手順の設定プロパティを開きます。
3. 「プロセス」タブをクリックします。
4. 渡す引数がサーバー上の有効なフォルダーであることを確認します。
5. 変更を保存します。

## アダプティブフォームを作成

1. 前の手順で作成したスキーマを使用して、アダプティブフォームを作成します。
2. いくつかのスキーマ要素をアダプティブフォームにドラッグ&amp;ドロップします。
3. アダプティブフォームの送信アクションを、AEMワークフロー (MergeAcroformData) に送信するように設定します。
4. **データファイルのパスとして「Data.xml」を指定していることを確認します。 これは、サンプルコードがワークフローペイロード内で Data.xml と呼ばれるファイルを探すので、非常に重要です。**
5. アダプティブフォームをプレビューし、フォームに入力して送信します。
6. ワークフローの設定PDFの手順 4 で指定したフォルダーに保存された、データが結合されたフォルダーが表示されます。

>[!NOTE]
>
>データを acroform と結合して生成された PDF は、ワークフローの payload フォルダーの下に pdfdocument.pdf として保存されます。 その後、このドキュメントをワークフローの一部として追加の処理に使用できます
