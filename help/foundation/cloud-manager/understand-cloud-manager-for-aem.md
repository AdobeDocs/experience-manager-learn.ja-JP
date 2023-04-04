---
title: AdobeCloud Manager について
description: AdobeCloud Manager は、AEM環境の管理、内観、セルフサービスを容易におこなえる、シンプルで堅牢なソリューションを提供します。
sub-product: Experience Manager Cloud Manager, Experience Manager
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Architecture
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '512'
ht-degree: 18%

---

# AdobeCloud Manager について

AdobeCloud Manager は、AEM環境の管理、内観、セルフサービスを容易におこなえる、シンプルで堅牢なソリューションを提供します。

## Cloud Manager の概要

このビデオシリーズでは、AEM向け Cloud Manager の主な機能を紹介します。以下に例を示します。

* [プログラム](#programs)
* [環境](#environments)
* [レポート](#reports)
* [CI/CD 実稼動パイプライン](#cicd-production-pipeline)
* [CI/CD 非実稼動パイプライン](#cicd-non-production-pipeline)
* [アクティビティ](#activity)

概要については、[Cloud Manager ユーザーガイド](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=ja)を参照してください。

## プログラム {#programs}

[Cloud Manager プログラム](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html) は、通常、購入したサービス契約 (SLA) に対応する、ビジネスイニシアチブの論理的なセットをサポートするAEM環境のセットを表します。 例えば、あるプログラムが、グローバルなパブリック Web サイトをサポートする AEM リソースを表す一方で、別のプログラムは社内の中核的 DAM を表します。

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## 環境 {#environments}

[Cloud Manager 環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html) は、AEM オーサー、AEM パブリッシュおよび Dispatcher インスタンスで構成されます。 環境が異なれば、役割をサポートし、異なる CI/CD パイプラインを使用してエンゲージできます（以下で説明）。 Cloud Manager 環境には通常、実稼動環境とステージング環境が 1 つあります。

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## レポート {#reports}

[Cloud Manager レポート](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html?lang=ja) 各AEMインスタンスの様々な指標をレポートおよび追跡する一連のグラフを通じて、プログラムの環境とAEMインスタンスを表示します。

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## CI/CD 実稼動パイプライン {#cicd-production-pipeline}

*[Cloud Manager での CI/CD パイプラインのAdobe](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) ビデオシリーズでは、失敗したデプロイメントや成功したデプロイメントの調査など、実稼動パイプラインの実行について詳しく説明します。*

>[!NOTE]
>
> これらのビデオ全体で、ビルド、テストおよびデプロイメントに要する時間が長くなり、ビデオの時間が短縮されました。 完全なパイプライン実行には通常 45 分以上かかります（必須の 30 分のパフォーマンステストを含む）。プロジェクトのサイズ、AEMインスタンスの数、UAT プロセスの数に応じて異なります。

### 設定

この [CI/CD 実稼動パイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) 設定は、パイプラインを開始するトリガー、および実稼動環境のデプロイメントとパフォーマンステストのパラメーターを制御するパラメーターを定義します。

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### パイプラインの実行

この [CI/CD 実稼動パイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html) は、ステージングを通じてコードを構築し、実稼動環境にデプロイするために使用され、価値創出までの時間を短縮します。

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## CI/CD 非実稼動パイプライン {#cicd-non-production-pipeline}

[CI／CD 実稼動以外のパイプラインは、コード品質パイプラインとデプロイパイプラインの 2 つのカテゴリに分類されます。](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html)コード品質は、Git ブランチのすべてのコードをパイプライン化し、Cloud Manager のコード品質スキャンに対して構築および評価されます。デプロイメントパイプラインは、Git リポジトリから非実稼動環境へのコードの自動デプロイメントをサポートしています。つまり、ステージング環境や実稼動環境以外のプロビジョニング済みAEM環境がサポートされます。

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## アクティビティ {#activity}

Cloud Manager では、すべての CI/CD パイプライン実行をリストし、過去と現在のアクティビティやアクティビティの詳細を確認できる、実稼動と非実稼動の両方を含む、プログラムのアクティビティを統合表示できます。

また、Cloud Manager は、ユーザー単位で、 [Adobe Experience Cloud Notifications](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html)を使用して、目的のイベントとアクションの全体像を提供する。

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
