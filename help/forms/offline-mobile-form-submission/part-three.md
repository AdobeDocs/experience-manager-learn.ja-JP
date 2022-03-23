---
title: HTM5 フォーム送信でのトリガーAEMワークフロー —PDFのレビューと承認
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: モバイルフォームをオフラインモードで入力し続け、モバイルフォームをトリガーAEMワークフローに送信します
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 2%

---

# 送信されたワークフローを確認して承認するPDF

最後となる手順は、レビューと承認用の静的または非インタラクティブなPDFを生成するAEMワークフローを作成することです。 ワークフローは、ノードで設定されたAEMランチャーを介してトリガーされます `/content/pdfsubmissions`.

次のスクリーンショットは、ワークフローに含まれる手順を示しています。

![ワークフロー](assets/workflow.PNG)

## 非インタラクティブPDFワークフローステップの生成

XDP テンプレートと、テンプレートと結合されるデータは、ここで指定します。 結合されるデータは、PDFから送信されたデータです。 この送信済みデータは、ノードに保存されます。 `/content/pdfsubmissions`.

![ワークフロー](assets/generate-pdf1.PNG)

生成されたPDFは、という名前のワークフロー変数に割り当てられます。 `submittedPDF`.

![ワークフロー](assets/generate-pdf2.PNG)

### 生成された PDF をレビューおよび承認用に割り当てます

タスクの割り当てワークフローコンポーネントは、ここで、レビューと承認用に生成されたPDFを割り当てる際に使用します。 変数 `submittedPDF` は、タスクを割り当てワークフローコンポーネントの「 Formsとドキュメント」タブで使用されます。

![ワークフロー](assets/assign-task.PNG)
