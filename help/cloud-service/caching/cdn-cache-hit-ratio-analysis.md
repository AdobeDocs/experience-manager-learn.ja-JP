---
title: CDN キャッシュヒット率の分析
description: AEMas a Cloud Serviceの CDN ログを分析する方法を説明します。 最適化のために、キャッシュヒット率、MISS および PASS キャッシュタイプの上位 URL などのインサイトを得ます。
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-11-10T00:00:00Z
jira: KT-13312
thumbnail: KT-13312.jpeg
exl-id: 43aa7133-7f4a-445a-9220-1d78bb913942
duration: 377
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1352'
ht-degree: 0%

---

# CDN キャッシュヒット率の分析

CDN でキャッシュされるコンテンツは、Web サイトユーザーが経験する待ち時間を短縮します。ユーザーは、要求が Apache/Dispatcher またはAEMパブリッシュに戻るのを待つ必要はありません。 このことを念頭に置いておくと、CDN でキャッシュ可能なコンテンツの量を最大限に増やすために、CDN キャッシュのヒット率を最適化することが重要です。

提供されるAEMas a Cloud Serviceの分析方法を説明します **CDN ログ** とは、次のようなインサイトを得る **キャッシュヒット率**、および **の上位の URL _MISS_ および _パス_ キャッシュの種類**、最適化のために使用します。


CDN ログは JSON 形式で利用できます。この形式には、以下のような様々なフィールドが含まれています。 `url`, `cache`. 詳しくは、 [CDN ログ形式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/logging.html?lang=en#cdn-log:~:text=Toggle%20Text%20Wrapping-,Log%20Format,-The%20CDN%20logs). The `cache` フィールドには、 _キャッシュの状態_ とその値は、HIT、MISS、PASS のいずれかです。 可能な値の詳細を確認します。

| キャッシュの状態 </br> 可能な値 | 説明 |
|------------------------------------|:-----------------------------------------------------:|
| ヒット | リクエストされたデータは _は CDN キャッシュに見つかり、フェッチを実行する必要はありません。_ リクエストをAEMサーバーに送信します。 |
| MISS | リクエストされたデータは _が CDN キャッシュに見つからないので、リクエストする必要があります_ をAEMサーバーから取得します。 |
| パス | リクエストされたデータは _明示的にキャッシュしないように設定_ とは、常にAEMサーバーから取得されます。 |

このチュートリアルの目的は、 [AEM WKND プロジェクト](https://github.com/adobe/aem-guides-wknd) がAEMas a Cloud Service環境にデプロイされ、 [Apache JMeter](https://jmeter.apache.org/).

このチュートリアルは、次の手順を実行するように構成されています。
1. Cloud Manager を使用した CDN ログのダウンロード
1. これらの CDN ログの分析。ローカルにインストールされたダッシュボード、またはリモートからアクセスした Jupiter Notebook(Adobe Experience Platformのライセンスを持つユーザー向け ) の 2 つの方法で実行できます。
1. CDN キャッシュ設定の最適化

## CDN ログをダウンロード

CDN ログをダウンロードするには、次の手順に従います。

1. Cloud Manager( ) にログインします。 [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) 組織とプログラムを選択します。

1. 目的の AEMCS 環境で、 **ログをダウンロード** 省略記号メニューから。

   ![ログのダウンロード — Cloud Manager](assets/cdn-logs-analysis/download-logs.png){width="500" zoomable="yes"}

1. Adobe Analytics の **ログをダウンロード** ダイアログで、 **公開** ドロップダウンメニューから「サービス」を選択し、「 **cdn** 行。

   ![CDN ログ — Cloud Manager](assets/cdn-logs-analysis/download-cdn-logs.png){width="500" zoomable="yes"}


ダウンロードしたログファイルが次の場所にある場合： _今日_ ファイルの拡張子はです。 `.log` それ以外の場合、過去のログファイルの拡張子は `.log.gz`.

## ダウンロードした CDN ログの分析

キャッシュヒット率や MISS および PASS キャッシュタイプの上位の URL などのインサイトを得るには、ダウンロードした CDN ログファイルを分析します。 これらのインサイトは、 [CDN キャッシュの設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=ja) サイトのパフォーマンスを向上させます。

CDN ログを分析するには、次の 2 つのオプションを紹介します。 **Elasticsearch、ログスタッシュ、きばな (ELK)** [ダッシュボードツール](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) および [Jupyter Notebook](https://jupyter.org/). ELK ダッシュボードツールは、ノートパソコンにローカルにインストールでき、Jupyry Notebook ツールにリモートからアクセスできます [Adobe Experience Platformの一部として](https://experienceleague.adobe.com/docs/experience-platform/data-science-workspace/jupyterlab/analyze-your-data.html?lang=en) Adobe Experience Platformのライセンスを持つユーザー向けに、追加のソフトウェアをインストールしないで


### オプション 1:ELK ダッシュボードツールの使用

The [ELK スタック](https://www.elastic.co/elastic-stack) は、データを検索、分析、視覚化するための拡張性の高いソリューションを提供する一連のツールです。 Elasticsearch、ログスタッシュ、きばなから成る。

重要な詳細を特定するには、 [AEMCS-CDN-Log-Analysis-ELK-Tool](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) ダッシュボードツールプロジェクト。 このプロジェクトは、ELK スタックの Docker コンテナと、CDN ログを分析するための事前設定済みの Kibana ダッシュボードを提供します。

1. 次の手順に従います。 [ELK Docker コンテナの設定方法](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool#how-to-set-up-the-elk-docker-container) をクリックし、必ず **CDN キャッシュヒット率** きばなダッシュボード。

1. CDN キャッシュヒット率と上位の URL を識別するには、次の手順に従います。

   1. ダウンロードした CDN ログファイルを環境固有のフォルダー内にコピーします。

   1. を開きます。 **CDN キャッシュヒット率** ダッシュボードを表示するには、左上隅のナビゲーションメニュー/ Analytics /ダッシュボード/CDN キャッシュヒット率をクリックします。

      ![CDN キャッシュヒット率 — Kibana ダッシュボード](assets/cdn-logs-analysis/cdn-cache-hit-ratio-dashboard.png){width="500" zoomable="yes"}

   1. 右上隅から目的の時間範囲を選択します。

      ![時間範囲 — きばなダッシュボード](assets/cdn-logs-analysis/time-range.png){width="500" zoomable="yes"}

   1. The **CDN キャッシュヒット率** ダッシュボードは説明がつかない状態です。

   1. The _合計リクエスト分析_ 「 」セクションには、次の詳細が表示されます。
      - キャッシュタイプ別のキャッシュ率
      - キャッシュタイプ別のキャッシュ数

      ![合計リクエスト分析 — Kibana ダッシュボード](assets/cdn-logs-analysis/total-request-analysis.png){width="500" zoomable="yes"}

   1. The _リクエストまたは MIME タイプ別の分析_ には、次の詳細が表示されます。
      - キャッシュタイプ別のキャッシュ率
      - キャッシュタイプ別のキャッシュ数
      - 上位の MISS および PASS URL

      ![リクエストまたは MIME タイプ別の分析 — Kibana ダッシュボード](assets/cdn-logs-analysis/analysis-by-request-or-mime-types.png){width="500" zoomable="yes"}

#### 環境名またはプログラム ID によるフィルタリング

取り込んだログを環境名でフィルターするには、次の手順に従います。

1. 「 CDN Cache Hit Ratio」ダッシュボードで、 **フィルターを追加** アイコン。

   ![フィルター — きばなダッシュボード](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. Adobe Analytics の **フィルターを追加** モーダルを開くには、 `aem_env_name.keyword` フィールドに値を入力し、 `is` 演算子と次のフィールドに必要な環境名を入力し、最後に「 」をクリックします。 _フィルターを追加_.

   ![フィルターを追加 — Kibana ダッシュボード](assets/cdn-logs-analysis/add-filter.png){width="500" zoomable="yes"}

#### ホスト名によるフィルタリング

取り込んだログをホスト名でフィルタリングするには、次の手順に従います。

1. 「 CDN Cache Hit Ratio」ダッシュボードで、 **フィルターを追加** アイコン。

   ![フィルター — きばなダッシュボード](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. Adobe Analytics の **フィルターを追加** モーダルを開くには、 `host.keyword` フィールドに値を入力し、 `is` 演算子と次のフィールドに必要なホスト名を入力し、最後に「 」をクリックします。 _フィルターを追加_.

   ![ホストフィルター — Kibana ダッシュボード](assets/cdn-logs-analysis/add-host-filter.png){width="500" zoomable="yes"}

同様に、分析要件に基づいて、ダッシュボードにフィルターを追加します。

### オプション 2:Jupyter ノートブックの使用

ソフトウェアをローカルにインストールしない（前の節の ELK ダッシュボードツール）場合は、別のオプションもありますが、Adobe Experience Platformのライセンスが必要です。

The [Jupyter Notebook](https://jupyter.org/) は、コード、テキスト、ビジュアライゼーションを含むドキュメントを作成できるオープンソース Web アプリケーションです。 データの変換、ビジュアライゼーション、統計的モデリングに使用されます。 リモートからアクセス可能 [Adobe Experience Platformの一部として](https://experienceleague.adobe.com/docs/experience-platform/data-science-workspace/jupyterlab/analyze-your-data.html?lang=en).

#### インタラクティブ Python ノートブックファイルのダウンロード

まず、 [AEM-as-a-CloudService - CDN ログ分析 — Jupyter Notebook](./assets/cdn-logs-analysis/aemcs_cdn_logs_analysis.ipynb) ファイルに含める必要があります。 この「Interactive Python Notebook」ファイルは説明に基づいて説明できますが、各セクションの主な特徴は次のとおりです。

- **追加のライブラリのインストール**：をインストールします。 `termcolor` および `tabulate` Python ライブラリ。
- **CDN ログを読み込む**：を使用して CDN ログファイルを読み込みます。 `log_file` 変数の値に設定します。必ず値を更新してください。 また、この CDN ログを [Pandas DataFrame](https://pandas.pydata.org/docs/reference/frame.html).
- **分析の実行**：最初のコードブロックは _合計、HTML、JS/CSS およびイメージリクエストの分析結果を表示_キャッシュヒット率の割合、棒グラフおよび円グラフを表示します。
2 番目のコードブロックは、 _HTML、JS/CSS、画像の MISS および PASS リクエスト URL の上位 5 件_&#x200B;の場合に、URL とその数を表形式で表示します。

#### Jupyter ノートブックの実行

次に、次の手順に従って、Adobe Experience Platformで Jupyter ノートブックを実行します。

1. にログインします。 [Adobe Experience Cloud](https://experience.adobe.com/)( ホームページ/ **クイックアクセス** セクション/クリック **Experience Platform**

   ![Experience Platform](assets/cdn-logs-analysis/experience-platform.png){width="500" zoomable="yes"}

1. Adobe Experience Platformホームページ/「 Data Science 」セクションで、 **ノートブック** メニュー項目。 Jupyter Notebooks 環境を開始するには、 **JupyterLab** タブをクリックします。

   ![ノートブックログファイルの値の更新](assets/cdn-logs-analysis/datascience-notebook.png){width="500" zoomable="yes"}

1. JupyterLab メニューで、 **ファイルをアップロード** アイコン、ダウンロードした CDN ログファイルをアップロードし、 `aemcs_cdn_logs_analysis.ipynb` ファイル。

   ![ファイルのアップロード — JupyteLab](assets/cdn-logs-analysis/jupyterlab-upload-file.png){width="500" zoomable="yes"}

1. を開きます。 `aemcs_cdn_logs_analysis.ipynb` ファイルをダブルクリックして保存します。

1. Adobe Analytics の **CDN ログファイルを読み込み** ノートブックのセクションで、 `log_file` の値です。

   ![ノートブックログファイルの値の更新](assets/cdn-logs-analysis/notebook-update-variable.png){width="500" zoomable="yes"}

1. 選択したセルを実行して先に進むには、 **再生** アイコン。

   ![ノートブックログファイルの値の更新](assets/cdn-logs-analysis/notebook-run-cell.png){width="500" zoomable="yes"}

1. 実行後 **合計、HTML、JS/CSS およびイメージリクエストの分析結果を表示** コード・セルには、キャッシュ・ヒット率の割合、棒グラフおよび円グラフが表示されます。

   ![ノートブックログファイルの値の更新](assets/cdn-logs-analysis/output-cache-hit-ratio.png){width="500" zoomable="yes"}

1. 実行後 **HTML、JS/CSS、画像の MISS および PASS リクエスト URL の上位 5 件** コードセルの場合、出力には上位 5 件の MISS および PASS リクエスト URL が表示されます。

   ![ノートブックログファイルの値の更新](assets/cdn-logs-analysis/output-top-urls.png){width="500" zoomable="yes"}

Jupyter Notebook を拡張し、要件に基づいて CDN ログを分析できます。

## CDN キャッシュ設定の最適化

CDN ログを分析した後、CDN キャッシュの設定を最適化して、サイトのパフォーマンスを向上させることができます。 AEMのベストプラクティスは、キャッシュのヒット率を 90%以上にすることです。

詳しくは、 [CDN キャッシュ設定の最適化](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#caching).

AEM WKND プロジェクトには、参照用 CDN 設定があります。詳しくは、 [CDN 設定](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L137-L190) から `wknd.vhost` ファイル。
