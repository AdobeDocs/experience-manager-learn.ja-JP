---
title: WAF ルールを含むトラフィックフィルタールールの設定方法
description: WAF ルールを含むトラフィックフィルタールールの結果を作成、デプロイ、テスト、および分析するための設定方法について説明します。
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: 87266a250eb91a82cf39c4a87e8f0119658cf4aa
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 3%

---


# WAF ルールを含むトラフィックフィルタールールの設定方法

学ぶ **設定方法** トラフィックフィルタールール（WAF ルールを含む）。 結果の作成、デプロイ、テスト、分析についてお読みください。

## セットアップ

設定プロセスには、次のものが含まれます。

- _ルールの作成_ 適切なAEMプロジェクト構造および設定ファイルを使用して、
- _ルールのデプロイ_ AdobeCloud Manager の設定パイプラインを使用している場合。
- _テストルール_ 様々なツールを使用したトラフィックの生成。
- _結果の分析_ AEMCS CDN ログとダッシュボードツールを使用する。

### AEMプロジェクトでのルールの作成

ルールを作成するには、次の手順に従います。

1. AEMプロジェクトの最上位レベルで、フォルダーを作成します。 `config`.

1. 内 `config` フォルダを作成し、新しいファイルを作成します。 `cdn.yaml`.

1. 次のメタデータを `cdn.yaml` ファイル：

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
```

次の例を参照してください： `cdn.yaml` ファイルをAEM Guides WKND Sites Project 内に配置します。

![WKND AEMプロジェクトルールファイルとフォルダー](./assets/wknd-rules-file-and-folder.png){width="800" zoomable="yes"}

### Cloud Manager を使用したルールのデプロイ {#deploy-rules-through-cloud-manager}

ルールをデプロイするには、次の手順に従います。

1. [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) で Cloud Manager にログインし、適切な組織とプログラムを選択します。

1. 次に移動： _パイプライン_ カード _プログラムの概要_ ページで、 **+追加** 」ボタンをクリックして、目的のパイプラインタイプを選択します。

   ![Cloud Manager パイプラインカード](./assets/cloud-manager-pipelines-card.png)

   上の例では、デモ用です。 _実稼動以外のパイプラインを追加_ は、開発環境が使用されているので、選択されています。

1. Adobe Analytics の _実稼動以外のパイプラインを追加_ ダイアログで、次の詳細を選択して入力します。

   1. 設定手順：

      - **タイプ**：デプロイメントパイプライン
      - **パイプライン名**:Dev-Config

      ![Cloud Manager 設定パイプラインダイアログ](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. ソースコードの手順：

      - **デプロイするコード**：ターゲットのデプロイメント
      - **次を含む**：設定
      - **デプロイメント環境**：環境の名前（例：wknd-program-dev）。
      - **リポジトリ**：パイプラインがコードを取得する元の Git リポジトリ。例： `wknd-site`
      - **Git ブランチ**:Git リポジトリブランチの名前。
      - **コードの場所**: `/config`（前の手順で作成した最上位の設定フォルダーに対応）

      ![Cloud Manager 設定パイプラインダイアログ](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### トラフィック生成によるルールのテスト

ルールをテストするには、様々なサードパーティツールが使用でき、組織が推奨するツールを使用できます。 デモ用に、次のツールを使用します。

- [Curl](https://curl.se/) を参照してください。

- [ベジータ](https://github.com/tsenart/vegeta) サービス拒否 (DOS) を実行する。 次のインストール手順に従います： [Vegeta GitHub](https://github.com/tsenart/vegeta#install).

- [Nikto](https://github.com/sullo/nikto/wiki) XSS や SQL インジェクションなどの潜在的な問題やセキュリティの脆弱性を見つける。 次のインストール手順に従います： [Nikto GitHub](https://github.com/sullo/nikto).

- 次のコマンドを実行して、ツールがインストールされ、ターミナルで使用可能であることを確認します。

  ```shell
  # Curl version check
  $ curl --version
  
  # Vegeta version check
  $ vegeta -version
  
  # Nikto version check
  $ cd <PATH-OF-CLONED-REPO>/program
  ./nikto.pl -Version
  ```

### ダッシュボードツールを使用した結果の分析

ルールを作成、デプロイ、テストした後、 **Elasticsearch、ログスタッシュ、きばな (ELK)** ダッシュボードツール。 AEMCS CDN ログを解析でき、結果を様々なグラフやグラフの形式で視覚化できます。

ダッシュボードツールは、 [AEMCS-CDN-Log-Analysis-ELK-Tool GitHub リポジトリ](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) をインストールし、 **トラフィックフィルタールール（WAF を含む）** ダッシュボード。

- サンプルダッシュボードを読み込むと、Elastic ダッシュボードのツールページは次のようになります。

  ![ELK トラフィックフィルタールールダッシュボード](./assets/elk-dashboard.png)

>[!NOTE]
>
>    AEMCS CDN ログはまだ取り込まれていないので、ダッシュボードは空です。


## 次の手順

WAF ルールを含むトラフィックフィルタールールを [例と結果の分析](./examples-and-analysis.md) この章では、 AEM WKND Sites Project を使用します。
