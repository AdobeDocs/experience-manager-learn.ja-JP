---
title: HTM5フォーム送信時のトリガーAEMワークフロー
seo-title: HTML5フォーム送信時のトリガーAEMワークフロー
description: オフラインモードでのモバイルフォームの入力を続け、モバイルフォームをトリガーAEMワークフローに送信する
seo-description: オフラインモードでのモバイルフォームの入力を続け、モバイルフォームをトリガーAEMワークフローに送信する
feature: 'モバイルフォーム '
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開発
role: デベロッパー
level: 経験豊富な
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 4%

---


# 送信済みPDFをレビューおよび承認するためのワークフロー

最後と最後のステップは、レビューと承認のための静的または非インタラクティブなPDFを生成するAEMワークフローを作成することです。 ワークフローは、ノード`/content/pdfsubmissions`上で構成されたAEMランチャーを介してトリガーされます。

次のスクリーンショットは、ワークフローに関連する手順を示しています。

![ワークフロー](assets/workflow.PNG)

## 非インタラクティブPDFワークフローの手順を生成

ここでは、XDPテンプレートとテンプレートに結合するデータを指定します。 結合されるデータは、PDFから送信されたデータです。 この送信データはノード`/content/pdfsubmissions`の下に保存されます。

![ワークフロー](assets/generate-pdf1.PNG)

生成されたPDFは`submittedPDF`という名前のワークフロー変数に割り当てられます。

![ワークフロー](assets/generate-pdf2.PNG)

### 生成されたpdfをレビューおよび承認用に割り当てます

「タスクの割り当て」ワークフローコンポーネントは、生成されたPDFをレビューおよび承認用に割り当てるために使用します。 変数`submittedPDF`は、タスク割り当てワークフローコンポーネントの「Formsとドキュメント」タブで使用されます。

![ワークフロー](assets/assign-task.PNG)
