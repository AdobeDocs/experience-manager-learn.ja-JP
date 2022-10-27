---
title: AEM インボックス
description: ワークフローデータに基づいて新しい列を追加してインボックスをカスタマイズ
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: Development
role: Developer
level: Experienced
exl-id: 3e1d86ab-e0c4-45d4-b998-75a44a7e4a3f
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 29%

---

# AEM インボックス

AEM受信トレイFormsワークフローを含む様々なAEMコンポーネントからの通知とタスクを統合します。 タスクを割り当てステップを含む Forms ワークフローがトリガーされると、関連するアプリケーションが担当者のインボックスにタスクとしてリストされます。

インボックスのユーザーインターフェイスでは、リストビューまたはカレンダービューでタスクを表示できます。ビューの設定もできます。様々なパラメーターに基づいて、タスクをフィルターできます。

Experience Managerインボックスをカスタマイズして、列のデフォルトのタイトルを変更したり、列の位置を並べ替えたり、ワークフローのデータに基づいて追加の列を表示したりできます。

>[!NOTE]
>
>インボックス列をカスタマイズするには、管理者またはワークフロー管理者のメンバーである必要があります

## 列のカスタマイズ

[Launch AEM inbox](http://localhost:4502/aem/inbox)
Admin Control を開くには、 _リスト表示_ アイコンと選択 _管理コントロール_ 下のスクリーンショットに示すように

![admin-control](assets/open-customization.png)

列のカスタマイズ UI で、次の操作を実行できます

* 列を削除
* 列の並べ替え
* 列名を変更

## ブランディングのカスタマイズ

ブランディングのカスタマイズでは、次の操作を実行できます

* 組織のロゴを追加
* ヘッダーテキストをカスタマイズ
* ヘルプリンクのカスタマイズ
* ナビゲーションオプションを非表示にする

![inbox-branding](assets/branding-customization.PNG)
