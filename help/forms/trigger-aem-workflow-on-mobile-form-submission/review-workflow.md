---
title: HTML 5 フォーム送信時のAEM トリガーワークフロー – PDFを確認し、承認します
description: 送信されたPDFを確認するワークフロー
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 5f42678502a785ead29982044d1f3f5ecf023e0f
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 69%

---

# 送信された PDF をレビューして承認するワ―クフロー

最後のステップは、レビューと承認を受けるための静的（非インタラクティブ）PDFを生成するAEM ワークフローを作成することです。 ワークフローは、ノード `/content/formsubmissions` に設定された AEM ランチャーを介してトリガーされます。

次のスクリーンショットは、ワークフローに含まれているステップを示しています。

![ワークフロー](assets/workflow.PNG)

## 非インタラクティブ PDF の生成ワークフローステップ

XDP テンプレートと、テンプレートと結合されるデータは、ここで指定します。 結合されるデータは、PDF 内の送信データです。 この送信データは、ノード ```/content/formsubmissions``` に保存されます。

![ワークフロー](assets/generate-pdf1.PNG)

生成された PDF は、`submittedPDF` という名前のワークフロー変数に割り当てられます。

![ワークフロー](assets/generate-pdf2.PNG)

### 生成された PDF のレビューおよび承認への割り当て

ここでタスクの割り当てワークフローコンポーネントを使用して、生成された PDF をレビューおよび承認の対象として割り当てます。 変数 `submittedPDF` は、タスクを割り当てワークフローコンポーネントの「フォームとドキュメント」タブで使用します。

![ワークフロー](assets/assign-task.PNG)


## 次の手順

[環境へのアセットのデプロイ](./deploy-assets.md)