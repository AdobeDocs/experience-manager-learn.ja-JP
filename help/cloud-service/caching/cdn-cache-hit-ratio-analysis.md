---
title: CDN キャッシュヒット率の分析
description: AEM as a Cloud Service が提供する CDN ログを分析する方法について説明します。キャッシュヒット率、MISS および PASS キャッシュタイプの上位 URL などのインサイトを取得して最適化を実現します。
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
duration: 342
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1352'
ht-degree: 63%

---

# CDN キャッシュヒット率の分析

CDN でキャッシュされるコンテンツは、Web サイトユーザーが経験する待ち時間を短縮します。ユーザーは、要求が Apache/Dispatcher またはAEMパブリッシュに戻るのを待つ必要はありません。 このことを念頭に置いておくと、CDN でキャッシュ可能なコンテンツの量を最大限に増やすために、CDN キャッシュのヒット率を最適化することが重要です。

提供されるAEMas a Cloud Serviceの分析方法を説明します **CDN ログ** とは、次のようなインサイトを得る **キャッシュヒット率**、および **の上位の URL _MISS_ および _パス_ キャッシュの種類**、最適化のために使用します。


CDN ログは JSON 形式で利用できます。この形式には、以下のような様々なフィールドが含まれています。 `url`, `cache`. 詳しくは、 [CDN ログ形式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/logging.html?lang=ja#cdn-log:~:text=Toggle%20Text%20Wrapping-,Log%20Format,-The%20CDN%20logs). `cache` フィールドでは、_キャッシュの状態_&#x200B;に関する情報を提供し、その考えられる値は HIT、MISS または PASS です。考えられる値の詳細を確認してみましょう。

| キャッシュの状態 </br> 考えられる値 | 説明 |
|------------------------------------|:-----------------------------------------------------:|
| HIT | リクエストしたデータは _CDN キャッシュ内で見つかるので、AEM サーバーに取得リクエストを行う必要はありません_。 |
| MISS | リクエストしたデータは _CDN キャッシュ内で見つからないので、AEM サーバーからリクエストする必要があります_。 |
| PASS | リクエストされたデータは&#x200B;_キャッシュされないように明示的に設定_&#x200B;され、常に AEM サーバーから取得されます。 |

このチュートリアルの目的は、 [AEM WKND プロジェクト](https://github.com/adobe/aem-guides-wknd) がAEMas a Cloud Service環境にデプロイされ、 [Apache JMeter](https://jmeter.apache.org/).

このチュートリアルは、次の手順を実行するように構成されています。
1. Cloud Manager を使用した CDN ログのダウンロード
1. これらの CDN ログの分析。ローカルにインストールされたダッシュボード、またはリモートからアクセスした Jupiter Notebook(Adobe Experience Platformのライセンスを持つユーザー向け ) の 2 つの方法で実行できます。
1. CDN キャッシュ設定の最適化

## CDN ログのダウンロード

CDN ログをダウンロードするには、次の手順に従います。

1. [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) で Cloud Manager にログインし、組織とプログラムを選択します。

1. 必要な AEMCS 環境の場合は、省略記号メニューから「**ログをダウンロード**」を選択します。

   ![ログのダウンロード - Cloud Manager](assets/cdn-logs-analysis/download-logs.png){width="500" zoomable="yes"}

1. **ログをダウンロード**&#x200B;ダイアログで、ドロップダウンメニューから&#x200B;**公開**&#x200B;サービスを選択し、**cdn** 行の横にあるダウンロードアイコンをクリックします。

   ![CDN ログ - Cloud Manager](assets/cdn-logs-analysis/download-cdn-logs.png){width="500" zoomable="yes"}


ダウンロードしたログファイルが&#x200B;_今日_&#x200B;のファイルである場合、ファイル拡張子は `.log` です。それ以外で、過去のログファイルの場合、拡張子は `.log.gz` です。

## ダウンロードした CDN ログの分析

キャッシュヒット率や MISS および PASS キャッシュタイプの上位の URL などのインサイトを得るには、ダウンロードした CDN ログファイルを分析します。 これらのインサイトは、[CDN キャッシュ設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=ja)を最適化し、サイトのパフォーマンスを向上させるのに役立ちます。

CDN ログを分析するには、次の 2 つのオプションを紹介します。 **Elasticsearch、ログスタッシュ、きばな (ELK)** [ダッシュボードツール](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) および [Jupyter Notebook](https://jupyter.org/). ELK ダッシュボードツールは、ノートパソコンにローカルにインストールでき、Jupyry Notebook ツールにリモートからアクセスできます [Adobe Experience Platformの一部として](https://experienceleague.adobe.com/docs/experience-platform/data-science-workspace/jupyterlab/analyze-your-data.html?lang=en) Adobe Experience Platformのライセンスを持つユーザー向けに、追加のソフトウェアをインストールしないで


### オプション 1:ELK ダッシュボードツールの使用

[ELK スタック](https://www.elastic.co/elastic-stack)は、データを検索、分析、視覚化するためのスケーラブルなソリューションを提供するツールのセットです。Elasticsearch、Logstash、Kibana で構成されます。

重要な詳細を特定するには、[AEMCS-CDN-Log-Analysis-ELK-Tool](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) ダッシュボードツールプロジェクトを使用してみましょう。このプロジェクトでは、ELK スタックの Docker コンテナと、CDN ログを分析するための事前設定済みの Kibana ダッシュボードを提供します。

1. [ELK Docker コンテナの設定方法](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool#how-to-set-up-the-elk-docker-container)の手順に従って、**CDN キャッシュヒット率** Kibana ダッシュボードを読み込む必要があります。

1. CDN キャッシュヒット率と上位の URL を特定するには、次の手順に従います。

   1. ダウンロードした CDN ログファイルを環境固有のフォルダー内にコピーします。

   1. を開きます。 **CDN キャッシュヒット率** ダッシュボードを表示するには、左上隅のナビゲーションメニュー/ Analytics /ダッシュボード/CDN キャッシュヒット率をクリックします。

      ![CDN キャッシュヒット率 - Kibana ダッシュボード](assets/cdn-logs-analysis/cdn-cache-hit-ratio-dashboard.png){width="500" zoomable="yes"}

   1. 右上隅から目的の時間範囲を選択します。

      ![時間範囲 - Kibana ダッシュボード](assets/cdn-logs-analysis/time-range.png){width="500" zoomable="yes"}

   1. **CDN キャッシュヒット率**&#x200B;ダッシュボードは一目瞭然です。

   1. 「_合計リクエスト分析_」セクションには、次の詳細が表示されます。
      - キャッシュタイプ別のキャッシュ率
      - キャッシュタイプ別のキャッシュ数

      ![合計リクエスト分析 - Kibana ダッシュボード](assets/cdn-logs-analysis/total-request-analysis.png){width="500" zoomable="yes"}

   1. _リクエストまたは MIME タイプ別の分析_&#x200B;には、次の詳細が表示されます。
      - キャッシュタイプ別のキャッシュ率
      - キャッシュタイプ別のキャッシュ数
      - 上位の MISS および PASS URL

      ![リクエストまたは MIME タイプ別の分析 - Kibana ダッシュボード](assets/cdn-logs-analysis/analysis-by-request-or-mime-types.png){width="500" zoomable="yes"}

#### 環境名またはプログラム ID によるフィルタリング

取り込んだログを環境名でフィルタリングするには、次の手順に従います。

1. CDN キャッシュヒット率ダッシュボードで、「**フィルターを追加**」アイコンをクリックします。

   ![フィルター - Kibana ダッシュボード](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. **フィルターを追加**&#x200B;モーダルで、ドロップダウンメニューから「`aem_env_name.keyword`」フィールドを選択し、次のフィールドに `is` 演算子と目的の環境名を選択して、最後に「_フィルターを追加_」をクリックします。

   ![フィルターを追加 - Kibana ダッシュボード](assets/cdn-logs-analysis/add-filter.png){width="500" zoomable="yes"}

#### ホスト名別のフィルタリング

取り込んだログをホスト名でフィルタリングするには、次の手順に従います。

1. CDN キャッシュヒット率ダッシュボードで、「**フィルターを追加**」アイコンをクリックします。

   ![フィルター - Kibana ダッシュボード](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. **フィルターを追加**&#x200B;モーダルで、ドロップダウンメニューから「`host.keyword`」フィールドを選択し、次のフィールドに `is` 演算子と目的のホスト名を選択して、最後に「_フィルターを追加_」をクリックします。

   ![ホストフィルター - Kibana ダッシュボード](assets/cdn-logs-analysis/add-host-filter.png){width="500" zoomable="yes"}

同様に、分析要件に基づいて、ダッシュボードにフィルターを追加します。

### オプション 2:Jupyter ノートブックの使用

ソフトウェアをローカルにインストールしない（前の節の ELK ダッシュボードツール）場合は、別のオプションもありますが、Adobe Experience Platformのライセンスが必要です。

[Jupyter Notebook](https://jupyter.org/) は、コード、テキスト、ビジュアライゼーションを含むドキュメントを作成できるオープンソース web アプリケーションです。データの変換、ビジュアライゼーション、統計的モデリングに使用されます。 リモートからアクセス可能 [Adobe Experience Platformの一部として](https://experienceleague.adobe.com/docs/experience-platform/data-science-workspace/jupyterlab/analyze-your-data.html?lang=en).

#### インタラクティブ Python ノートブックファイルのダウンロード

まず、 [AEM-as-a-CloudService - CDN ログ分析 — Jupyter Notebook](./assets/cdn-logs-analysis/aemcs_cdn_logs_analysis.ipynb) ファイルに含める必要があります。 この「Interactive Python Notebook」ファイルは説明に基づいて説明できますが、各セクションの主な特徴は次のとおりです。

- **追加のライブラリをインストール**：`termcolor` および `tabulate` Python ライブラリをインストールします。
- **CDN ログを読み込む**：を使用して CDN ログファイルを読み込みます。 `log_file` 変数の値に設定します。必ず値を更新してください。 また、この CDN ログを [Pandas DataFrame](https://pandas.pydata.org/docs/reference/frame.html) に変換します。
- **分析の実行**：最初のコードブロックは _合計、HTML、JS/CSS およびイメージリクエストの分析結果を表示_キャッシュヒット率の割合、棒グラフおよび円グラフを表示します。
2 番目のコードブロックは、 _HTML、JS/CSS、画像の MISS および PASS リクエスト URL の上位 5 件_&#x200B;の場合に、URL とその数を表形式で表示します。

#### Jupyter ノートブックの実行

次に、次の手順に従って、Adobe Experience Platformで Jupyter ノートブックを実行します。

1. [Adobe Experience Cloud](https://experience.adobe.com/) にログインし、ホームページ／「**クイックアクセス**」セクション／**Experience Platform** をクリックします

   ![Experience Platform](assets/cdn-logs-analysis/experience-platform.png){width="500" zoomable="yes"}

1. Adobe Experience Platform ホームページ／「データサイエンス」セクションで、**Notebooks** メニュー項目をクリックします。Jupyter Notebooks 環境を開始するには、「**JupyterLab**」タブをクリックします。

   ![Notebook ログファイル値の更新](assets/cdn-logs-analysis/datascience-notebook.png){width="500" zoomable="yes"}

1. JupyterLab メニューで、「**ファイルをアップロード**」アイコンを使用して、ダウンロードした CDN ログファイルと、`aemcs_cdn_logs_analysis.ipynb` ファイルをアップロードします。

   ![ファイルをアップロード - JupyteLab](assets/cdn-logs-analysis/jupyterlab-upload-file.png){width="500" zoomable="yes"}

1. `aemcs_cdn_logs_analysis.ipynb` ファイルをダブルクリックして開きます。

1. ノートブックの「**CDN ログファイルを読み込み**」セクションで、`log_file` 値を更新します。

   ![ノートブックのログファイルの値の更新](assets/cdn-logs-analysis/notebook-update-variable.png){width="500" zoomable="yes"}

1. 選択したセルを実行して先に進むには、「**再生**」アイコンをクリックします。

   ![ノートブックのログファイルの値の更新](assets/cdn-logs-analysis/notebook-run-cell.png){width="500" zoomable="yes"}

1. **合計、HTML、JS/CSS および画像リクエストの分析結果を表示**&#x200B;コードセルを実行すると、出力にキャッシュヒット率の割合、棒グラフおよび円グラフが表示されます。

   ![ノートブックのログファイルの値の更新](assets/cdn-logs-analysis/output-cache-hit-ratio.png){width="500" zoomable="yes"}

1. **HTML、JS/CSS および画像の上位 5 つの MISS および PASS リクエスト URL** コードセルを実行すると、出力に上位 5 つの MISS および PASS リクエスト URL が表示されます。

   ![ノートブックのログファイルの値の更新](assets/cdn-logs-analysis/output-top-urls.png){width="500" zoomable="yes"}

Jupyter Notebook を拡張し、要件に基づいて CDN ログを分析できます。

## CDN キャッシュ設定の最適化

CDN ログを分析した後、CDN キャッシュ設定を最適化してサイトのパフォーマンスを向上させることができます。AEM のベストプラクティスは、キャッシュヒット率を 90％以上にすることです。

詳しくは、[CDN キャッシュ設定の最適化](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=ja#caching)を参照してください。

AEM WKND プロジェクトには参照 CDN 設定があります。詳しくは、`wknd.vhost` ファイルの [CDN 設定](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L137-L190)を参照してください。
