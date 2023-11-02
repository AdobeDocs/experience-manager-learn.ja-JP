---
title: WAF ルールを含むトラフィックフィルタールールの例と結果の分析
description: WAF ルールの例を含む、様々なトラフィックフィルタールールについて説明します。 また、AEMas a Cloud Service(AEMCS)CDN ログを使用して結果を分析する方法についても説明します。
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 49becbcb-7965-4378-bb8e-b662fda716b7
source-git-commit: ceb498f751ffc50d0022a16b63f9f52594bc507e
workflow-type: tm+mt
source-wordcount: '1512'
ht-degree: 1%

---

# WAF ルールを含むトラフィックフィルタールールの例と結果の分析

Adobe Experience Manager as a Cloud Service(AEMCS)CDN ログとダッシュボードツールを使用して、様々なタイプのトラフィックフィルタールールを宣言し、結果を分析する方法について説明します。

この節では、WAF ルールを含むトラフィックフィルタールールの実用的な例を見ていきます。 URI（またはパス）、IP アドレス、リクエスト数、様々な攻撃タイプに基づいて、 [AEM WKND Sites Project](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

さらに、AEMCS CDN ログを取り込むダッシュボードツールを使用して、Adobe提供のサンプルダッシュボードを通じて重要な指標を視覚化する方法について説明します。

特定の要件に合わせるには、カスタムダッシュボードを拡張および作成して、AEMサイトのルール設定をより深く把握し、最適化します。

>[!VIDEO](https://video.tv.adobe.com/v/3425404?quality=12&learn=on)

## 例

WAF ルールを含むトラフィックフィルタールールの様々な例を見てみましょう。 前述の説明に従って、必要な設定プロセスが完了していることを確認します。 [設定方法](./how-to-setup.md) チャプターとあなたがクローンを作成した [AEM WKND Sites Project](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

### リクエストのログ

開始日 **WKND ログインパスとログアウトパスのリクエストのログ** をAEM Publish サービスに対して設定します。

- WKND プロジェクトの `/config/cdn.yaml` ファイル。

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
    # On AEM Publish service log WKND Login and Logout requests 
      - name: publish-auth-requests
        when:
          allOf:
            - reqProperty: tier
              matches: publish
            - reqProperty: path
              in:
                - /system/sling/login/j_security_check
                - /system/sling/logout
        action: log
```

- 変更をコミットして Cloud Manager Git リポジトリにプッシュします。

- Cloud Manager を使用して、AEM開発環境に変更をデプロイします。 `Dev-Config` 設定パイプライン [以前に作成済み](how-to-setup.md#deploy-rules-through-cloud-manager).

  ![Cloud Manager 設定パイプライン](./assets/cloud-manager-config-pipeline.png)

- パブリッシュサービスでプログラムの WKND サイトにログインおよびサインアウトして、ルールをテストします ( 例： `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`) をクリックします。 以下を使用できます。 `asmith/asmith` ユーザー名とパスワード。

  ![WKND ログイン](./assets/wknd-login.png)

#### 分析中{#analyzing}

次に、 `publish-auth-requests` ルールを作成するには、Cloud Manager から AEMCS CDN ログをダウンロードし、 [ダッシュボードツール](how-to-setup.md#analyze-results-using-elk-dashboard-tool)：前の章で設定した内容。

- 送信者 [Cloud Manager](https://my.cloudmanager.adobe.com/)&#39;s **環境** カード、AEMCS のダウンロード **公開** サービスの CDN ログ。

  ![Cloud Manager CDN ログのダウンロード](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    新しいリクエストが CDN ログに表示されるまでに最大 5 分かかる場合があります。

- ダウンロードしたログファイルをコピーします ( 例： `publish_cdn_2023-10-24.log` 以下のスクリーンショットで ) `logs/dev` Elastic dashboard ツールプロジェクトのフォルダー。

  ![ELK ツールログフォルダ](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- Elastic ダッシュボードツールページを更新します。
   - 上部 **グローバルフィルター** セクション、編集 `aem_env_name.keyword` フィルターを適用して選択します。 `dev` 環境の値。

     ![ELK ツールグローバルフィルタ](./assets/elk-tool-global-filter.png)

   - 期間を変更するには、右上隅のカレンダーアイコンをクリックし、目的の期間を選択します。

     ![ELK ツールの時間間隔](./assets/elk-tool-time-interval.png)

- 更新されたダッシュボードの  **分析済みリクエスト**, **フラグ付きリクエスト**、および **フラグ付きリクエストの詳細** パネル。 CDN ログエントリを照合するには、各エントリのクライアント IP(cli_ip)、host、url、action(waf_action)、rule-name(waf_match) の値を表示する必要があります。

  ![ELK ツールダッシュボード](./assets/elk-tool-dashboard.png)


### リクエストのブロック

この例では、 _内部_ パスのフォルダー `/content/wknd/internal` をデプロイします。 次に、次のようなトラフィックフィルタールールを宣言します。 **トラフィックをブロック** を使用して、組織に一致する指定の IP アドレス（企業 VPN など）以外の場所からサブページを作成できます。

独自の内部ページ ( `demo-page.html`) または [添付のパッケージ](./assets/demo-internal-pages-package.zip).

- WKND プロジェクトの `/config/cdn.yaml` ファイル：

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
    ...

    # Block requests to (demo) internal only page/s from public IP address but allow from internal IP address.
    # Make sure to replace the IP address with your own IP address.
      - name: block-internal-paths
        when:
          allOf:
            - reqProperty: path
              matches: /content/wknd/internal
            - reqProperty: clientIp
              notIn: [192.150.10.0/24]
        action: block
```

- 変更をコミットして Cloud Manager Git リポジトリにプッシュします。

- を使用して、AEM開発環境に変更をデプロイします。 [以前作成済み](how-to-setup.md#deploy-rules-through-cloud-manager) `Dev-Config` Cloud Manager の設定パイプライン。

- WKND サイトの内部ページ（例： ）にアクセスして、ルールをテストします。 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` または、次の CURL コマンドを使用します。

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- ルールで使用した IP アドレスと、別の IP アドレス（例えば、携帯電話を使用する場合）の両方から上記の手順を繰り返します。

#### 分析中

結果を分析するには `block-internal-paths` ルールを使用する場合は、 [以前の例](#analyzing).

ただし、今回は、 **ブロックされた要求** および対応する値をクライアント IP(cli_ip)、ホスト、URL、アクション (waf_action)、ルール名 (waf_match) の各列に格納します。

![ELK ツールダッシュボードブロック済みリクエスト](./assets/elk-tool-dashboard-blocked.png)


### DoS 攻撃の防止

さあ **DoS 攻撃を防ぐ** IP アドレスからの要求を 1 秒に 100 個の要求でブロックすることで、5 分間ブロックされます。

- 以下を追加します。 [レート制限トラフィックフィルタールール](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#ratelimit-structure) WKND プロジェクトの `/config/cdn.yaml` ファイル。

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
    ...
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block     
```

>[!WARNING]
>
>実稼動環境に合わせて、Web セキュリティチームと連携し、 `rateLimit`,

- 変更のコミット、プッシュ、デプロイ ( [以前の例](#logging-requests).

- DoS 攻撃をシミュレートするには、次を使用します。 [ベジータ](https://github.com/tsenart/vegeta) コマンドを使用します。

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=5s | vegeta report
  ```

  このコマンドは、120 件のリクエストを 5 秒間実行し、レポートを出力します。 ご覧のように、成功率は 32.5%です。残りの部分に対しては 406 HTTP 応答コードを受信し、トラフィックがブロックされたことを示しています。

  ![ベジータ・ドス攻撃](./assets/vegeta-dos-attack.png)

#### 分析中

結果を分析するには `prevent-dos-attacks` ルールを使用する場合は、 [以前の例](#analyzing).

今回は、 **ブロックされた要求** および対応する値をクライアント IP(cli_ip)、ホスト、url、アクション (waf_action)、ルール名 (waf_match) の各列に格納します。

![ELK ツールダッシュボード DoS リクエスト](./assets/elk-tool-dashboard-dos.png)

また、 **クライアント IP、国、ユーザーエージェントによる上位 100 件の攻撃** パネルには、追加の詳細が表示されます。この詳細を使用して、ルールの設定をさらに最適化できます。

![ELK ツールダッシュボードで上位 100 件のリクエストを実行](./assets/elk-tool-dashboard-dos-top-100.png)

### WAF ルール

これまでのトラフィックフィルタールールの例は、Sites およびFormsのすべてのお客様が設定できます。

次に、拡張セキュリティまたは WAF-DDoS 保護ライセンスを購入したお客様の経験を見てみましょう。このライセンスを使用して、より高度な攻撃から Web サイトを保護する高度なルールを設定できます。

続行する前に、トラフィックフィルタールールのドキュメントに記載されているように、プログラムの WAF-DoS 保護を有効にします。 [設定手順](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en#setup).

#### WAFFlags を使用しない場合

WAF ルールが宣言される前でも、まずエクスペリエンスを確認してみましょう。 プログラムで WAF-DDoS が有効になっている場合、CDN はデフォルトで悪意のあるトラフィックとの一致をログに記録するので、適切なルールを考案する適切な情報を持ちます。

まず、WAF ルールを追加せずに WKND サイトを攻撃する ( または `wafFlags` プロパティ ) を参照し、結果を分析します。

- 攻撃をシミュレートするには、 [Nikto](https://github.com/sullo/nikto) コマンドを使用して、約 700 件の悪意のある要求を 6 分で送信します。

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![ニクト攻撃シミュレーション](./assets/nikto-attack.png)

  攻撃シミュレーションについて詳しくは、 [Nikto — スキャンの調整](https://github.com/sullo/nikto/wiki/Scan-Tuning) ドキュメント。含めるまたは除外するテスト攻撃のタイプを指定する方法を示します。

##### 分析中

攻撃シミュレーションの結果を分析するには、 [以前の例](#analyzing).

ただし、今回は、 **フラグ付きリクエスト** および対応する値をクライアント IP(cli_ip)、ホスト、url、アクション (waf_action)、ルール名 (waf_match) の各列に格納します。 この情報を使用すると、結果を分析し、ルールの設定を最適化できます。

![ELK ツールダッシュボード WAF フラグ付きリクエスト](./assets/elk-tool-dashboard-waf-flagged.png)

以下の点に注意してください。 **WAF フラグ配布** および **トップ攻撃** パネルには、追加の詳細が表示されます。これは、ルールの設定をさらに最適化するために使用できます。

![ELK ツールダッシュボード WAF フラグ攻撃リクエスト](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![ELK ツールダッシュボード WAF トップ攻撃リクエスト](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### WAFFlags を使用

次に、次を含む WAF ルールを追加します。 `wafFlags` プロパティを `action` プロパティと **模擬攻撃要求を阻止する**.

構文の観点から見ると、WAF ルールは以前に見たものと似ていますが、 `action` プロパティが 1 つ以上の `wafFlags` 値。 詳しくは、 `wafFlags`、レビュー [WAF フラグリスト](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#waf-flags-list) 」セクションに入力します。

- WKND プロジェクトの `/config/cdn.yaml` ファイル。 以下の点に注意してください。 `block-waf-flags` ルールには、悪意のあるトラフィックをシミュレートして攻撃されたときにダッシュボードツールに表示された wafFlags の一部が含まれます。 実際、ログを分析し、脅威の状況が拡大するにつれて、どの新しいルールを宣言するかを決定するのは、時間の経過と共に推奨されます。

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
    ...     
    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - SIGSCI-IP
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8        
```

- 変更のコミット、プッシュ、デプロイ ( [以前の例](#logging-requests).

- 攻撃をシミュレートするには、同じ [Nikto](https://github.com/sullo/nikto) コマンドを使用します。

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### 分析中

同じ手順を繰り返します ( [以前の例](#analyzing).

今回は、「 **ブロックされた要求** およびクライアント IP(cli_ip)、host、url、action(waf_action)、rule-name(waf_match) 列の対応する値。

![ELK ツールダッシュボード WAF ブロックリクエスト](./assets/elk-tool-dashboard-waf-blocked.png)

また、 **WAF フラグ配布** および **トップ攻撃** パネルに追加の詳細が表示されます。

![ELK ツールダッシュボード WAF フラグ攻撃リクエスト](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![ELK ツールダッシュボード WAF トップ攻撃リクエスト](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### 包括的な分析

上記の _分析_ セクションでは、ダッシュボードツールを使用して特定のルールの結果を分析する方法を学びました。 以下を含む他のダッシュボードパネルを使用して、分析結果をさらに詳しく調べることができます。


- 分析済み、フラグ付きおよびブロック済みのリクエスト
- WAF フラグの配布の経時的変化
- 時間の経過と共にトリガーされるトラフィックフィルタールール
- WAF フラグ ID によるトップ攻撃
- 上位のトリガートラフィックフィルター
- クライアント IP、国、ユーザーエージェントによる攻撃者上位 100 人

![ELK ツールダッシュボードの包括的な分析](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![ELK ツールダッシュボードの包括的な分析](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## 次の手順

推奨される情報を確認 [ベストプラクティス](./best-practices.md) セキュリティ侵害のリスクを軽減する。

## その他のリソース

[トラフィックフィルタールールの構文](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#rules-syntax)

[CDN ログ形式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#cdn-log-format)
