---
title: AEM のシステム概要ダッシュボードの使用
description: 以前のバージョンの AEM では、管理者は AEM インスタンスの全体像を把握するために、複数の場所を参照する必要がありました。システム概要では、AEM インスタンスの設定、ハードウェアおよびヘルスの概要を 1 つのダッシュボードにすべて表示することでこれを解決することを目的としています。
version: Experience Manager 6.4, Experience Manager 6.5
feature: Operations
doc-type: Technical Video
topic: Administration
role: Admin
level: Beginner
exl-id: af8f499c-4955-44b5-8f21-085263ca31a3
duration: 357
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '142'
ht-degree: 100%

---

# システム概要ダッシュボードの使用

Adobe Experience Manager（AEM）[!UICONTROL システム概要]は、AEM インスタンスの設定、ハードウェアおよびヘルスの概要を、1 つのダッシュボードにすべて表示します。

>[!VIDEO](https://video.tv.adobe.com/v/40394?quality=12&learn=on&captions=jpn)

1. システム概要には、 **AEM 開始**／**[!UICONTROL ツール]**／**[!UICONTROL 運用]**／**[!UICONTROL システム概要]**&#x200B;でアクセスできます。

   **`<server-host>/libs/granite/operations/content/systemoverview.html`** で直接アクセスすることもできます。

1. [!UICONTROL システム概要]に表示される情報は、「[!UICONTROL ダウンロード] 」ボタンをクリックして書き出すことができます。情報は、以下の [!DNL REST] エンドポイントでも公開されます。
1. 以下は、[!UICONTROL システム概要]から書き出されたサンプル JSON 出力です。

   ```json
   {
       "Health Checks": {
           "1 Critical": "System Maintenance",
           "3 Warn": "Replication Queue, Log Errors, Sling/Granite Content Access Check"
       },
       "Instance": {
           "Adobe Experience Manager": "6.4.0",
           "Run Modes": "s7connect, crx3, non-composite, author, samplecontent, crx3tar",
           "Instance Up Since": "2018-01-22 10:50:37"
       },
       "Repository": {
           "Apache Jackrabbit Oak": "1.8.0",
           "Node Store": "Segment Tar",
           "Repository Size": "0.26 GB",
           "File Data Store": "crx-quickstart/repository/datastore"
       },
       "Maintenance Tasks": {
           "Failed": "AuditLog Maintenance Task, Project Purge, Workflow Purge",
           "Succeeded": "Data Store Garbage Collection, Lucene Binaries Cleanup, Revision Clean Up, Version Purge, Purge of ad-hoc tasks"
       },
       "System Information": {
           "Mac OS X": "10.12.6",
           "System Load Average": "2.29",
           "Usable Disk Space": "89.44 GB",
           "Maximum Heap": "0.97 GB"
       },
       "Estimated Node Counts": {
           "Total": "232448",
           "Tags": "62",
           "Assets": "267",
           "Authorizables": "210",
           "Pages": "1592"
       },
       "Replication Agents": {
           "Blocked": "publish, publish2",
           "Idle": "offloading_b3deb296-9b28-4f60-8587-c06afa2e632c, offloading_outbox, offloading_reverse_b3deb296-9b28-4f60-8587-c06afa2e632c, publish_reverse, scene7, screens, screens2, test_and_target"
       },
       "Distribution Agents": {
           "Blocked": "publish"
       },
       "Workflows": {
           "Running Workflows": "15",
           "Failed Workflows": "15",
           "Failed Jobs": "150"
       },
       "Sling Jobs": {
           "Failed": "305"
       }
   }
   ```
