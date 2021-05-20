---
title: HTM5フォーム送信時のトリガーAEMワークフロー
seo-title: HTML5フォーム送信時のトリガーAEMワークフロー
description: オフラインモードでモバイルフォームの入力を続け、モバイルフォームをトリガーAEMワークフローに送信する
seo-description: オフラインモードでモバイルフォームの入力を続け、モバイルフォームをトリガーAEMワークフローに送信する
feature: 'モバイルフォーム '
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 3%

---


# 送信されたPDFを確認および承認するワークフロー

最後の手順は、レビューと承認用の静的または非インタラクティブなPDFを生成するAEMワークフローを作成することです。 ワークフローは、ノード`/content/pdfsubmissions`上に設定されたAEM Launcherを介してトリガーされます。

次のスクリーンショットは、ワークフローに含まれる手順を示しています。

![ワークフロー](assets/workflow.PNG)

## 非インタラクティブPDFワークフローステップの生成

XDPテンプレートと、テンプレートにマージされるデータをここで指定します。 結合されるデータは、PDFから送信されたデータです。 この送信データは、ノード`/content/pdfsubmissions`に保存されます。

![ワークフロー](assets/generate-pdf1.PNG)

生成されたPDFは、`submittedPDF`という名前のワークフロー変数に割り当てられます。

![ワークフロー](assets/generate-pdf2.PNG)

### 生成されたPDFをレビューおよび承認用に割り当てます

タスクの割り当てワークフローコンポーネントは、ここで生成されたPDFをレビューおよび承認用に割り当てる際に使用します。 変数`submittedPDF`は、タスクの割り当てワークフローコンポーネントの「Formsとドキュメント」タブで使用されます。

![ワークフロー](assets/assign-task.PNG)
