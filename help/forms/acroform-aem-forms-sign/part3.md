---
title: Acroforms と AEM Forms の連携
description: Acroforms と AEM Forms の統合に関するチュートリアルのパート 3 です。お使いのシステムでワークフローとアダプティブフォームをテストします。
feature: adaptive-forms
doc-type: Tutorial
version: Experience Manager 6.5
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 45
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '228'
ht-degree: 100%

---


# システム上でのこの機能のテスト

[AEM へのこのパッケージのダウンロードと読み込み](assets/acro-form-aem-form.zip)
このパッケージには、サンプルワークフローのほか、アップロードされた Acroform からスキーマを作成できる html ページが含まれています。

## ワークフローの設定

1. [ワークフローモデルを編集モードで開きます](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html)。
2. MergeAcroformData ステップの設定プロパティを開きます。
3. 「プロセス」タブをクリックします。
4. 渡す引数がサーバー上の有効なフォルダーであることを確認します。
5. 変更を保存します。

## アダプティブフォームの作成

1. 前の手順で作成したスキーマを使用して、アダプティブフォームを作成します。
2. いくつかのスキーマ要素をアダプティブフォームにドラッグ＆ドロップします。
3. AEM ワークフロー（MergeAcroformData）に送信するように、アダプティブフォームの送信アクションを設定します。
4. **データファイルパスを「Data.xml」として指定していることを確認します。サンプルコードはワークフローペイロードで Data.xml というファイルを検索するので、これは非常に重要です。**
5. アダプティブフォームをプレビューし、フォームに入力して送信します。
6. 設定ワークフローのステップ 4 で指定したフォルダーに結合データが保存された PDF が表示されます。

>[!NOTE]
>
>データを Acroform と結合して生成された PDF は、ワークフローのペイロードフォルダーに pdfdocument.pdf として保存されます。このドキュメントは次に、ワークフローの一部としてさらに処理するために使用できます
