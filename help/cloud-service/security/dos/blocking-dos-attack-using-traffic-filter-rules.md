---
title: トラフィックフィルタールールを使用した DoS および DDoS 攻撃のブロック
description: AEMas a Cloud Service提供の CDN でトラフィックフィルタールールを使用して DoS 攻撃と DDoS 攻撃をブロックする方法を説明します。
version: Cloud Service
feature: Security, Operations
topic: Security, Administration, Performance
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-03-18T00:00:00Z
jira: KT-15184
thumbnail: KT-15184.jpeg
exl-id: 60c2306f-3cb6-4a6e-9588-5fa71472acf7
source-git-commit: 9af60e1d45eb4569e05ba135fec43e4c744b2a0d
workflow-type: tm+mt
source-wordcount: '1918'
ht-degree: 1%

---

# トラフィックフィルタールールを使用した DoS および DDoS 攻撃のブロック

を使用してサービス拒否（DoS）攻撃と分散型サービス拒否（DDoS）攻撃をブロックする方法を説明します **レート制限トラフィックフィルター** AEMas a Cloud Service（AEMCS）が管理する CDN のルールおよびその他の戦略。 これらの攻撃は、CDN および潜在的にAEM パブリッシュサービス（オリジン）でトラフィックスパイクを引き起こし、サイトの応答性と可用性に影響を与える可能性があります。

このチュートリアルは、次の方法のガイドとして機能します _トラフィックパターンを分析し、レート制限を設定する方法 [トラフィックフィルタールール](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)_ これらの攻撃を軽減する。 このチュートリアルでは、次の方法についても説明します [アラートの設定](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) そのため、攻撃が疑われる場合に通知されます。

## 保護について

AEM web サイトのデフォルトの DDoS 保護機能を説明します。

- **キャッシュ：** 適切なキャッシュポリシーを使用すると、CDN がほとんどのリクエストが接触チャネルに送信されてパフォーマンスの低下を引き起こすのを防ぐので、DDoS 攻撃の影響はより制限されます。
- **自動スケーリング：** AEM オーサーサービスとパブリッシュサービスは、トラフィックスパイクを処理するために自動スケールされますが、トラフィックの突然の大幅な増加によって引き続き影響を受ける可能性があります。
- **ブロック：** Adobe CDN は、CDN PoP （Point of Presence）ごとに特定の IP アドレスからAdobeが定義した速度を超えた場合、発信元へのトラフィックをブロックします。
- **アラート：** トラフィックが特定の割合を超えた場合、アクションセンターはオリジンアラート通知でトラフィックスパイクを送信します。 このアラートは、特定の CDN PoP へのトラフィックがを超えると起動されます _Adobe定義_ ip アドレスごとのリクエスト率。 参照： [トラフィックフィルタールールールのアラート](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) を参照してください。

これらの組み込み保護は、DDoS 攻撃のパフォーマンスへの影響を最小限に抑える組織の能力のベースラインと考える必要があります。 Web サイトごとにパフォーマンス特性が異なり、Adobeが定義したレート制限に達する前にパフォーマンスが低下する可能性があるので、次の方法でデフォルトの保護機能を拡張することをお勧めします _顧客設定_.

DDoS 攻撃から web サイトを保護するために、顧客が実行できる追加の推奨される手段を見てみましょう。

- 宣言 **レート制限トラフィックフィルタールール** pop ごとに、単一の IP アドレスから一定のレートを超えるトラフィックをブロックする。 これらは通常、Adobeが定義したレート制限よりも低いしきい値です。
- 設定 **アラート** 「アラートアクション」を介したレート制限トラフィックフィルタールールにより、ルールがトリガーされると、アクションセンターの通知が送信されます。
- を宣言してキャッシュカバレッジを増やす **リクエスト変換** ：クエリパラメーターを無視します。

>[!NOTE]
>
>この [トラフィックフィルタールールアラート](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) 機能はまだリリースされていません。 早期導入プログラムを通じてアクセスできるようにするには、電子メールを送信します **<aemcs-waf-adopter@adobe.com>**.

### レート制限トラフィックルールのバリエーション {#rate-limit-variations}

レート制限トラフィックルールには、次の 2 つのバリエーションがあります。

1. エッジ – 特定の IP に対するすべてのトラフィック（CDN キャッシュから提供できるトラフィックを含む）の割合に基づいて、PoP ごとにリクエストをブロックします。
1. オリジン - PoP ごとに、特定の IP を対象とするオリジン宛てのトラフィックの割合に基づいてリクエストをブロックします。

## カスタマージャーニー

以下の手順は、顧客が web サイトの保護に取り組む必要があるプロセスを反映しています。

1. レート制限トラフィックフィルタールールの必要性の認識。 これは、Adobeの標準のトラフィックスパイクを接触チャネルアラートで受信した結果である可能性もあれば、DDoS が成功するリスクを減らすための予防措置を講じるためのプロアクティブな判断である可能性があります。
1. サイトが既に稼働している場合は、ダッシュボードを使用してトラフィックパターンを分析し、レート制限トラフィックフィルタールールに最適なしきい値を決定します。 サイトがまだ運用されていない場合は、トラフィックの期待に基づいて値を選択します。
1. 前のステップの値を使用して、レート制限トラフィックフィルタールールを設定します。 対応するアラートを必ず有効にし、しきい値に達した場合に通知を受け取るようにします。
1. トラフィックスパイクが発生するたびにトラフィックフィルタールールのアラートを受信し、組織が悪意のあるアクターによってターゲットされている可能性があるかどうかに関する貴重なインサイトを提供します。
1. 必要に応じて、アラートに基づいて動作します。 トラフィックを分析し、スパイクが攻撃ではなく、正当なリクエストを反映しているかどうかを判断します。 トラフィックが適切な場合は、しきい値を増やし、そうでない場合は減らします。

このチュートリアルの残りの部分では、このプロセスを順を追って説明します。

## ルール設定の必要性の認識 {#recognize-the-need}

前述したように、Adobeはデフォルトで、特定のレートを超える CDN のトラフィックをブロックしますが、一部の web サイトでは、そのしきい値を下回るとパフォーマンスが低下する場合があります。 したがって、レート制限トラフィックフィルタールールを設定する必要があります。

実稼動環境への移行を開始する前に、ルールを設定するのが理想です。 実際には、多くの組織は、攻撃の可能性を示すトラフィックスパイクにアラートが送信されたルールを 1 回だけ積極的に宣言します。

Adobeがオリジンアラートでトラフィックスパイクを [アクション センターの通知](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/operations/actions-center) 特定の PoP に対して、単一の IP アドレスからのトラフィックのデフォルトのしきい値を超えた場合。 このようなアラートを受信した場合は、レート制限トラフィックフィルタールールを設定することをお勧めします。 このデフォルトのアラートは、トラフィックフィルタールールを定義する際に、顧客が明示的に有効にする必要があるアラートとは異なります。このルールについては、後の節で説明します。


## トラフィックパターンの分析 {#analyze-traffic}

サイトが既に実稼働している場合は、CDN ログと次のいずれかの方法を使用して、トラフィックパターンを分析できます。

### ELK - ダッシュボードツールの設定

この **Elasticsearch、ログ、およびキバナ （ELK）** Adobeが提供するダッシュボードツールを使用して、CDN ログを分析できます。 このツールには、トラフィックパターンを視覚化するダッシュボードが含まれており、レート制限トラフィックフィルタールールの最適なしきい値を簡単に決定できます。

- のクローン [AEMCS-CDN-Log-Analysis-ELK-Tool](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) GitHub リポジトリ。
- 次の手順に従ってツールを設定します [ELK Docker コンテナの設定方法](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool?tab=readme-ov-file#how-to-set-up-the-elk-docker-container) 手順。
- 設定の一環として、を読み込みます。 `traffic-filter-rules-analysis-dashboard.ndjson` ファイルを使用してデータを視覚化します。 この _CDN トラフィック_ ダッシュボードには、CDN エッジとオリジンでの IP/POP ごとの最大要求数を表示するビジュアライゼーションが含まれています。
- から [Cloud Manager](https://my.cloudmanager.adobe.com/)の _環境_ カードで、AEMCS パブリッシュサービスの CDN ログをダウンロードします。

  ![Cloud Manager CDN ログのダウンロード](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > 新しいリクエストが CDN ログに表示されるまでに最大 5 分かかる場合があります。

### Splunk - ダッシュボードツールの設定

次の顧客： [Splunk ログ転送が有効](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) 新しいダッシュボードを作成して、トラフィックパターンを分析できます。 次の XML ファイルは、Splunk でダッシュボードを作成するのに役立ちます。

- [CDN - トラフィックダッシュボード](./assets/traffic-dashboard.xml)：このダッシュボードは、CDN エッジおよびオリジンのトラフィックパターンに関するインサイトを提供します。 CDN エッジとオリジンでの IP/POP ごとの最大要求数を表示するビジュアライゼーションが含まれます。

### データの確認

ELK ダッシュボードと Splunk ダッシュボードでは、次のビジュアライゼーションを使用できます。

- **クライアント IP および POP ごとのエッジ RPS**：このビジュアライゼーションには、IP/POP あたりの最大要求数が表示されます **cdn エッジで**. ビジュアライゼーションのピークは、リクエストの最大数を示します。

  **ELK ダッシュボード**:
  ![ELK ダッシュボード - IP/POP あたりの最大リクエスト数](./assets/elk-edge-max-per-ip-pop.png)

  **Splunk ダッシュボード**:\
  ![Splunk ダッシュボード - IP/POP あたりの最大リクエスト数](./assets/splunk-edge-max-per-ip-pop.png)

- **クライアント IP および POP ごとのオリジン RPS**：このビジュアライゼーションには、IP/POP あたりの最大要求数が表示されます **原点で**. ビジュアライゼーションのピークは、リクエストの最大数を示します。

  **ELK ダッシュボード**:
  ![ELK ダッシュボード - IP/POP あたりの最大オリジンリクエスト数](./assets/elk-origin-max-per-ip-pop.png)

  **Splunk ダッシュボード**:
  ![Splunk ダッシュボード - IP/POP あたりの最大オリジンリクエスト数](./assets/splunk-origin-max-per-ip-pop.png)

## しきい値の選択

レート制限トラフィックフィルタールールのしきい値は、上記の分析に基づく必要があり、正当なトラフィックがブロックされないようにする必要があります。 しきい値の値の選択方法のガイダンスについては、次の表を参照してください。

| バリエーション | 値 |
| :--------- | :------- |
| 複製元 | 次の場所にある、IP/POP あたりの最大オリジンリクエスト数の最大値を取得します **標準** トラフィック条件（つまり、DDoS の時点でのレートではなく）を増やし、それを倍数で増やします |
| Edge | 次の場所にある IP/POP あたりの最大エッジ要求数の最大値を取得します **標準** トラフィック条件（つまり、DDoS の時点でのレートではなく）を増やし、それを倍数で増やします |

使用する乗数は、有機トラフィック、キャンペーン、その他のイベントに起因するトラフィックの通常のスパイクと予想される値によって異なります。 5 ～ 10 の倍数は妥当です。

サイトがまだ運用されていない場合は、分析するデータがないので、レート制限トラフィックフィルタールールに設定する適切な値について知識に基づいて推測する必要があります。 次に例を示します。

| バリエーション | 値 |
|------------------------------ |:-----------:|
| Edge | 500 |
| 複製元 | 100 |

## ルールの設定 {#configure-rules}

の設定 **レート制限トラフィックフィルター** AEM プロジェクトのルール `/config/cdn.yaml` ファイル。上記の説明に基づいた値を指定します。 必要に応じて、Web セキュリティチームに問い合わせて、レート制限値が適切であり、正当なトラフィックをブロックしていないことを確認してください。

こちらを参照してください [AEM プロジェクトでのルールの作成](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#create-rules-in-your-aem-project) を参照してください。

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
    #  Prevent attack at edge by blocking client for 5 minutes if they make more than 500 requests per second on average
      - name: prevent-dos-attacks-edge
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 500 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: all # count all requests
          groupBy:
            - reqProperty: clientIp
        action: 
          type: log
          experimental_alert: true
    #  Prevent attack at origin by blocking client for 5 minutes if they make more than 100 requests per second on average            
      - name: prevent-dos-attacks-origin
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 100 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: fetches # count only fetches
          groupBy:
            - reqProperty: clientIp
        action: 
          type: log
          experimental_alert: true   
          
```

接触チャネルルールとエッジルールの両方が宣言され、アラートプロパティがに設定されていることに注意してください `true` したがって、しきい値に達した場合は常にアラートを受け取り、攻撃を示す可能性が高くなります。

>[!NOTE]
>
>この _実験的_ experimental_alert の前にある prefix_は、アラート機能がリリースされると削除されます。 早期導入プログラムに参加するには、次のメールを送信します **<aemcs-waf-adopter@adobe.com>**.

数時間または数日間トラフィックを監視できるように、アクションタイプを最初はログに記録するように設定し、正当なトラフィックがこれらのレートを超えないようにすることをお勧めします。 数日後、ブロックモードに変更します。

AEMCS 環境に変更をデプロイするには、次の手順に従います。

- 上記の変更をコミットして Cloud Manager の Git リポジトリにプッシュします。
- Cloud Manager の設定パイプラインを使用して、変更を AEMCS 環境にデプロイします。 参照 [Cloud Manager を使用したルールのデプロイ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager) を参照してください。
- を検証するには： **レート制限トラフィックフィルタールール** が期待どおりに動作している場合は、の説明に従って攻撃をシミュレートできます。 [攻撃シミュレーション](#attack-simulation) セクション。 リクエスト数を、ルールで設定されているレート制限値より大きい値に制限します。

### リクエスト変換ルールの設定 {#configure-request-transform-rules}

レート制限トラフィックフィルタールールに加えて、を使用することをお勧めします。 [リクエスト変換](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#request-transformations) キャッシュバスティング技術を通じてキャッシュをバイパスする方法を最小限に抑えるために、アプリケーションが必要としないクエリパラメーターの設定を解除する。 例えば、の許可のみをおこなう場合は、 `search` および `campaignId` クエリパラメーター。次のルールを宣言できます。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: 
    - dev
    - stage
    - prod  
data:  
  experimental_requestTransformations:
    rules:            
      - name: unset-all-query-params-except-those-needed
        when:
          reqProperty: tier
          in: ["publish"]
        actions:
          - type: unset
            queryParamMatch: ^(?!search$|campaignId$).*$
```

## トラフィックフィルタールールールのアラートの受信 {#receiving-alerts}

前述のように、トラフィックフィルタールールにが含まれる場合 *experimental_alert: true*&#x200B;の場合、ルールが一致するとアラートを受け取ります。

## アラートに対するアクション {#acting-on-alerts}

場合によっては、アラートは情報目的であり、攻撃の頻度を把握できます。 上記のダッシュボードを使用して CDN データを分析し、トラフィックスパイクが正当なトラフィック量の増加だけでなく攻撃によるものであることを検証することをお勧めします。 後者の場合は、しきい値を増やすことを検討してください。

## 攻撃シミュレーション{#attack-simulation}

ここでは、DoS 攻撃をシミュレートする方法について説明します。この方法を使用して、このチュートリアルで使用するダッシュボードのデータを生成したり、設定されたルールが攻撃を正常にブロックしたことを検証したりできます。

>[!CAUTION]
>
> これらの手順は、実稼動環境では実行しないでください。 次の手順はシミュレーションの目的でのみ使用します。
> 
>トラフィックのスパイクを示すアラートを受信した場合は、に進みます [トラフィックパターンの分析](#analyzing-traffic-patterns) セクション。

攻撃をシミュレートするには、次のようなツールがあります [Apache ベンチマーク](https://httpd.apache.org/docs/2.4/programs/ab.html), [Apache JMeter](https://jmeter.apache.org/), [ベジータ](https://github.com/tsenart/vegeta)、などを使用できます。

### Edge リクエスト

次を使用します [ベジータ](https://github.com/tsenart/vegeta) コマンド web サイトに対して多くのリクエストを行うことができます。

```shell
$ echo "GET https://<YOUR-WEBSITE-DOMAIN>" | vegeta attack -rate=120 -duration=5s | vegeta report
```

上記のコマンドでは、120 件のリクエストを 5 秒間実行し、レポートを出力します。 Web サイトがレート制限されていないと、トラフィックのスパイクが発生する可能性があります。

### 接触チャネルリクエスト

CDN キャッシュを回避し、オリジン（AEM パブリッシュサービス）にリクエストを送信するには、URL に一意のクエリパラメーターを追加できます。 からサンプルの Apache JMeter スクリプトを参照してください [JMeter スクリプトを使用して DoS 攻撃をシミュレート](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection#simulate-dos-attack-using-jmeter-script)

