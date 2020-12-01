---
title: Adobeクラウドマネージャーについて
description: AdobeCloud Managerは、AEM環境の管理、内省、セルフサービスを容易にする、シンプルで堅牢なソリューションです。
sub-product: クラウドマネージャー，ファンデーション
feature: pipelines, programs, projects, quality-gates, reports
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 22%

---


# Adobeクラウドマネージャーについて

AdobeCloud Managerは、AEM環境の管理、内省、セルフサービスを容易にする、シンプルで堅牢なソリューションです。

## Cloud Manager の概要

このビデオシリーズでは、AEM向けCloud Managerの主な機能を紹介します。

* [プログラム](#programs)
* [環境](#environments)
* [レポート](#reports)
* [CI/CD実稼働パイプライン](#cicd-production-pipeline)
* [CI/CD非実稼働パイプライン](#cicd-non-production-pipeline)
* [アクティビティ](#activity)

概要については、[Cloud Manager ユーザーガイド](https://docs.adobe.com/content/help/ja-JP/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html)を参照してください。

## プログラム {#programs}

[Cloud Manager](https://docs.adobe.com/content/help/ja-JP/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) プログラムは、ビジネスイニシアチブの論理的な組み合わせをサポートするAEM環境のセットを表します。通常、SLA(Service Level Agreement)に対応します。例えば、あるプログラムがグローバルパブリックWebサイトをサポートするAEMリソースを表し、別のプログラムが内部Central DAMを表す場合があります。

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## 環境 {#environments}

[Cloud Manager](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) 環境は、AEM Author、AEM Publish、およびディスパッチャーの各インスタンスで構成されます。環境が異なると役割がサポートされ、さまざまなCI/CDパイプラインを使用して関与できます（以下で説明）。 通常、Cloud Manager環境には、実稼働環境とステージ環境が1つあります。

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## レポート {#reports}

[Cloud Manager のレポートでは、各 AEM インスタンスの様々な指標をレポートおよび追跡する一連のグラフを通じて、プログラムの環境と AEM インスタンスを確認できます。](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html)

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD実稼働パイプライン{#cicd-production-pipeline}

*[Adobeクラウド](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) マネージャービデオシリーズのCI/CDパイプラインを使用すると、実稼働パイプラインの実行を詳細に分析できます。失敗したデプロイメントや成功したデプロイメントの調査などを行うことができます。*

>[!NOTE]
>
> これらのビデオでは、ビルド、テスト、導入の時間が短縮され、ビデオの所要時間が短縮されています。 プロジェクトのサイズ、AEMインスタンス数、UATプロセス数に応じて、通常、完全なパイプラインの実行には45分以上かかります（30分の必須のパフォーマンステストを含む）。

### 設定

[CI/CD Production Pipeline](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html)構成は、パイプラインを開始するトリガーを定義します。このトリガーは、実稼働環境の展開とパフォーマンステストのパラメーターを制御するパラメーターです。

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### パイプラインの実行

[CI/CD Production Pipeline](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html)は、Stageを使用してコードを構築し、実稼働環境に導入する際に使用します。これにより、タイム・トゥ・バリューが減少します。

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD非実稼働パイプライン{#cicd-non-production-pipeline}

[CI／CD 非実稼動パイプラインは、コード品質パイプラインとデプロイパイプラインの 2 つのカテゴリに分類されます。](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines)コード品質は、Git ブランチのすべてのコードをパイプライン化し、Cloud Manager のコード品質スキャンに対して構築および評価されます。
デプロイメントパイプラインは、Gitリポジトリから実稼働以外の環境(StageまたはProduction以外のプロビジョニング済みAEM環境)へのコードの自動デプロイをサポートしています。

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## アクティビティ {#activity}

Cloud Managerは、プログラムのアクティビティに統合された表示を提供します。本番環境と非実稼働環境の両方のCI/CDパイプライン実行がすべてリストされ、過去と現在のアクティビティを表示でき、アクティビティの詳細を確認できます。

また、Cloud Managerは、ユーザーごとのレベルで、[Adobe Experience Cloud通知](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html)と統合され、あらゆる面でのイベントや関心のある行動に対する表示を提供します。

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
