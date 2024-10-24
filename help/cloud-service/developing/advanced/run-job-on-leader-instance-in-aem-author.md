---
title: AEM as a Cloud Serviceでリーダーインスタンスのジョブを実行する方法
description: AEM as a Cloud Serviceのリーダーインスタンスでジョブを実行する方法を説明します。
version: Cloud Service
topic: Development
feature: OSGI, Cloud Manager
role: Architect, Developer
level: Intermediate, Experienced
doc-type: Article
duration: 0
last-substantial-update: 2024-10-23T00:00:00Z
jira: KT-16399
thumbnail: KT-16399.jpeg
source-git-commit: 7dca86137d476418c39af62c3c7fa612635c0583
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 0%

---


# AEM as a Cloud Serviceでリーダーインスタンスのジョブを実行する方法

AEM as a Cloud Serviceの一部として、AEM オーサーサービスのリーダーインスタンスでジョブを実行する方法と、1 回だけ実行するように設定する方法について説明します。

Sling ジョブは、バックグラウンドで動作する非同期タスクで、システムまたはユーザーがトリガーするイベントを処理するように設計されています。 デフォルトでは、これらのジョブはクラスター内のすべてのインスタンス（ポッド）に均等に分散されます。

詳しくは、[Apache Sling のイベントとジョブ処理 ](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html) を参照してください。

## ジョブの作成と処理

デモのために、ジョブプロセッサーにメッセージをログに記録するように指示する単純な _ジョブ_ を作成しましょう。

### ジョブの作成

次のコードを使用して、Apache Sling ジョブを _作成_ します。

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import java.util.HashMap;
import java.util.Map;

import org.apache.sling.event.jobs.JobManager;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(immediate = true)
public class SimpleJobCreaterImpl {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobCreaterImpl.class);

    // Define the topic on which the job will be created
    protected static final String TOPIC = "wknd/simple/job/topic";

    // Inject a JobManager
    @Reference
    private JobManager jobManager;

    @Activate
    protected final void activate() throws Exception {
        log.info("SimpleJobCreater activated successfully");
        createJob();
        log.info("SimpleJobCreater created a job");
    }

    private void createJob() {
        // Create a job and add it on the above defined topic
        Map<String, Object> jobProperties = new HashMap<>();
        jobProperties.put("action", "log");
        jobProperties.put("message", "Job metadata is: Created in activate method");
        jobManager.addJob(TOPIC, jobProperties);
    }
}
```

上記のコードで注意すべき重要な点は次のとおりです。

- ジョブのペイロードには、`action` と `message` の 2 つのプロパティがあります。
- [JobManager](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/apache/sling/event/jobs/JobManager.html) の `addJob(...)` メソッドを使用して、トピック `wknd/simple/job/topic` にジョブを追加します。

### ジョブの処理

上記の Apache Sling ジョブを _処理_ するには、以下のコードを使用します。

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import org.apache.sling.event.jobs.Job;
import org.apache.sling.event.jobs.consumer.JobConsumer;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = JobConsumer.class, property = {
        JobConsumer.PROPERTY_TOPICS + "=" + SimpleJobCreaterImpl.TOPIC
}, immediate = true)
public class SimpleJobConsumerImpl implements JobConsumer {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobConsumerImpl.class);

    @Override
    public JobResult process(Job job) {
        // Get the action and message properties
        String action = job.getProperty("action", String.class);
        String message = job.getProperty("message", String.class);

        // Log the message
        if ("log".equals(action)) {
            log.info("Processing WKND Job, and {}", message);
        }

        // Return a successful result
        return JobResult.OK;
    }

}
```

上記のコードで注意すべき重要な点は次のとおりです。

- `SimpleJobConsumerImpl` クラスは、`JobConsumer` インターフェイスを実装します。
- これは、トピック `wknd/simple/job/topic` からジョブを使用するように登録されたサービスです。
- `process(...)` メソッドは、ジョブペイロードの `message` プロパティをログに記録することでジョブを処理します。

### デフォルトのジョブ処理

上記のコードをAEM as a Cloud Service環境にデプロイし、複数のAEM オーサー JVM を備えたクラスターとして動作するAEM オーサーサービスで実行すると、ジョブは各AEM オーサーインスタンス（ポッド）で 1 回実行されます。つまり、作成されたジョブの数がポッドの数と一致します。 ポッドの数は、常に複数になりますが（非 RDE 環境の場合）、AEM as a Cloud Serviceの内部リソース管理に基づいて変動します。

ジョブはAEM オーサーインスタンス（ポッド）ごとに実行されます。これは、`wknd/simple/job/topic` がAEMのメインキューに関連付けられていて、使用可能なすべてのインスタンスにジョブが配布されるためです。

リソースや外部サービスの作成や更新など、ジョブが状態の変更を担当している場合は、これが問題になることがよくあります。

AEM オーサーサービスでジョブを 1 回だけ実行する場合は、以下に説明する [ ジョブキュー設定 ](#how-to-run-a-job-on-the-leader-instance) を追加します。

[Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs#cloud-manager) のAEM オーサーサービスのログを確認することで、これを確かめることができます。

![ すべてのインスタンスで処理されるジョブ ](./assets/run-job-once/job-processed-by-all-instances.png)


以下が表示されます。

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-nxxcx] *INFO* [sling-oak-observation-15] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method

<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-r4zk7] *INFO* [sling-oak-observation-11] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```

2 つのログエントリがあり、各AEM オーサーインスタンス（`68775db964-nxxcx` と `68775db964-r4zk7`）に 1 つずつ存在しています。これは、各インスタンス（ポッド）がジョブを処理したことを示しています。

## リーダーインスタンスでジョブを実行する方法

AEM オーサーサービスでジョブを _1 回だけ_ 実行するには、タイプ **Ordered** の新しい Sling ジョブキューを作成し、そのキューにジョブトピック（`wknd/simple/job/topic`）を関連付けます。 この設定では、リーダーのAEM オーサーインスタンス（ポッド）のみがジョブを処理できます。

AEM プロジェクトの `ui.config` モジュールで、OSGi 設定ファイル（`org.apache.sling.event.jobs.QueueConfiguration~wknd.cfg.json`）を作成し、`ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author` フォルダーに保存します。

```json
{
    "queue.name":"WKND Queue - ORDERED",
    "queue.topics":[
      "wknd/simple/job/topic"
    ],
    "queue.type":"ORDERED",
    "queue.retries":1,
    "queue.maxparallel":1.0
  }
```

上記の設定で注意すべき重要な点は次のとおりです。

- キュートピックが `wknd/simple/job/topic` に設定されています。
- キューの種類は `ORDERED` に設定されています。
- 並列ジョブの最大数は `1` に設定されています。

上記の設定をデプロイすると、ジョブはリーダーインスタンスによってのみ処理され、AEM オーサーサービス全体で 1 回だけ実行されるようになります。

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-7475cf85df-qdbq5] *INFO* [FelixLogListener] Events.Service.org.apache.sling.event Service [QueueMBean for queue WKND Queue - ORDERED,7755, [org.apache.sling.event.jobs.jmx.StatisticsMBean]] ServiceEvent REGISTERED
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
<DD.MM.YYYY HH:mm:ss.SSS> [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```
