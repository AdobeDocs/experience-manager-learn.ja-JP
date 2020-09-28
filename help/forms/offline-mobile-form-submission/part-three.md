---
title: HTM5フォーム送信でのAEMワークフローのトリガー
seo-title: HTML5フォーム送信時のAEMワークフローのトリガー
description: オフラインモードでモバイルフォームの入力を続け、モバイルフォームを送信してAEMワークフローをトリガーする
seo-description: オフラインモードでモバイルフォームの入力を続け、モバイルフォームを送信してAEMワークフローをトリガーする
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: c56942831614b981684861ea78f1bd15f3bb1ab9
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 2%

---


# 送信済みPDFをレビューおよび承認するためのワークフロー

最後と最後のステップは、レビューと承認のための静的または非インタラクティブなPDFを生成するAEMワークフローを作成することです。 ワークフローは、ノード上で設定されたAEMランチャーを介してトリガーされ `/content/pdfsubmissions`ます。

次のスクリーンショットは、ワークフローに関連する手順を示しています。

![ワークフロー](assets/workflow.PNG)

## 非インタラクティブPDFワークフローの手順を生成

ここでは、XDPテンプレートとテンプレートに結合するデータを指定します。 結合されるデータは、PDFから送信されたデータです。 この送信データはノードの下に保存され `/content/pdfsubmissions`ます。

![ワークフロー](assets/generate-pdf1.PNG)

生成されたPDFは、という名前のワークフロー変数に割り当てられ `submittedPDF`ます。

![ワークフロー](assets/generate-pdf2.PNG)

### 生成されたpdfをレビューおよび承認用に割り当てます

「タスクの割り当て」ワークフローコンポーネントは、生成されたPDFをレビューおよび承認用に割り当てるために使用します。 この変数 `submittedPDF` は、タスク割り当てワークフローコンポーネントの「Formsとドキュメント」タブで使用されます。

![ワークフロー](assets/assign-task.PNG)
