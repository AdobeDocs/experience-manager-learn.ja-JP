---
title: HTM5 フォーム送信での AEM ワークフローのトリガー - PDF のレビューと承認
description: オフラインモードでのモバイルフォームへの入力の継続とモバイルフォームの送信による AEM ワークフローのトリガー
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
duration: 32
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 100%

---

# 送信された PDF をレビューして承認するワ―クフロー

最後のステップは、レビューと承認を受けるための静的（非インタラクティブ）PDF を生成する AEM ワークフローを作成することです。 ワークフローは、ノード `/content/pdfsubmissions` に設定された AEM ランチャーを介してトリガーされます。

次のスクリーンショットは、ワークフローに含まれているステップを示しています。

![ワークフロー](assets/workflow.PNG)

## 非インタラクティブ PDF の生成ワークフローステップ

XDP テンプレートと、テンプレートと結合されるデータは、ここで指定します。 結合されるデータは、PDF 内の送信データです。 この送信データは、ノード `/content/pdfsubmissions` に保存されます。

![ワークフロー](assets/generate-pdf1.PNG)

生成された PDF は、`submittedPDF` という名前のワークフロー変数に割り当てられます。

![ワークフロー](assets/generate-pdf2.PNG)

### 生成された PDF のレビューおよび承認への割り当て

ここでタスクの割り当てワークフローコンポーネントを使用して、生成された PDF をレビューおよび承認の対象として割り当てます。 変数 `submittedPDF` は、タスクを割り当てワークフローコンポーネントの「フォームとドキュメント」タブで使用します。

![ワークフロー](assets/assign-task.PNG)
