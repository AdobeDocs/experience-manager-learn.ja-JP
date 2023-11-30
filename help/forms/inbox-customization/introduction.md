---
title: AEM インボックス
description: ワークフローデータに基づいて新しい列を追加してインボックスをカスタマイズします。
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: 3e1d86ab-e0c4-45d4-b998-75a44a7e4a3f
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 100%

---

# AEM インボックス

AEM インボックスは、Forms ワークフローを含む様々な AEM コンポーネントの通知やタスクを統合します。タスクを割り当てステップを含む Forms ワークフローがトリガーされると、関連するアプリケーションが担当者のインボックスにタスクとしてリストされます。

インボックスのユーザーインターフェイスでは、リストビューまたはカレンダービューでタスクを表示できます。ビューの設定もできます。様々なパラメーターに基づいて、タスクをフィルターできます。

Experience Manager インボックスをカスタマイズして、列のデフォルトのタイトルを変更したり、列の位置を並べ替えたり、ワークフローのデータに基づいて追加の列を表示したりできます。

>[!NOTE]
>
>インボックス列をカスタマイズするには、管理者またはワークフロー管理者のメンバーである必要があります

## 列をカスタマイズ

[Experience Platform Launch AEM インボックス](http://localhost:4502/aem/inbox)
以下のスクリーンショットに示すように、_リスト表示_&#x200B;アイコンをクリックしてから&#x200B;_管理者制御_&#x200B;を選択して、管理者制御を開きます。

![admin-control](assets/open-customization.png)

列のカスタマイズ UI で、次の操作を実行できます。

* 列を削除
* 列を並べ替え
* 列名を変更

## ブランディングのカスタマイズ

ブランディングのカスタマイズでは以下の操作を行うことができます。

* 組織のロゴを追加
* ヘッダーテキストをカスタマイズ
* ヘルプリンクをカスタマイズ
* ナビゲーションオプションを非表示

![inbox-branding](assets/branding-customization.PNG)

## 次の手順

[既婚列の追加](./add-married-column.md)
