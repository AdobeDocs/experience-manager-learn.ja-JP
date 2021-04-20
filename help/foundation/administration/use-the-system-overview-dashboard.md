---
title: AEMのシステム概要ダッシュボードの使用
description: 以前のバージョンのAEM管理者は、AEMインスタンスの全体像を取得するために複数の場所を調べる必要がありました。 システム概要では、AEMインスタンスの設定、ハードウェア、および正常性の高レベルの表示をすべて1つのダッシュボードから提供することで、この問題を解決することを目的としています。
version: 6.4, 6.5
topics: administration, operations, monitoring
activity: use
audience: administrator, architect, developer, implementer
doc-type: technical video
contentOwner: dgordon
topic: Administration
role: Administrator
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 0%

---


# システム概要ダッシュボードの使用

Adobe Experience Manager(AEM) [!UICONTROL システム概要]は、AEMインスタンスの構成、ハードウェア、および正常性を、すべて1つのダッシュボードから高度に表示します。

>[!VIDEO](https://video.tv.adobe.com/v/21340?quality=12&learn=on)

1. システム概要には次の場所からアクセスできます。**AEM開始** > **[!UICONTROL ツール]** > **[!UICONTROL 操作]** > **[!UICONTROL システムの概要]**

   **`<server-host>/libs/granite/operations/content/systemoverview.html`**&#x200B;に直接

1. 「[!UICONTROL システム概要]」の情報は、「[!UICONTROL ダウンロード]」ボタンをクリックするとエクスポートできます。 この情報は、次の[!DNL REST]エンドポイントを介しても公開されます。
1. 以下は、[!UICONTROL システム概要]からエクスポートされたJSONの出力例です。

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
