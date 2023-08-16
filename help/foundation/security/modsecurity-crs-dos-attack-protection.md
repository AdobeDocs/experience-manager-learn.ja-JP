---
title: ModSecurity を使用して、AEMサイトを DoS 攻撃から保護します。
description: OWASP ModSecurity コアルールセット (CRS) を使用して、ModSecurity を有効にして、サイトをサービス拒否 (DoS) 攻撃から保護する方法を説明します。
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect
level: Experienced
kt: 10385
thumbnail: KT-10385.png
doc-type: article
last-substantial-update: 2023-08-15T00:00:00Z
source-git-commit: 31d54b14fc6381e8b231cf85d3c808b88c7df098
workflow-type: tm+mt
source-wordcount: '1252'
ht-degree: 3%

---


# ModSecurity を使用してAEMサイトを DoS 攻撃から保護する

ModSecurity を有効にして、 **OWASP ModSecurity コアルールセット (CRS)** をAdobe Experience Manager(AEM)Publish Dispatcher に設定します。

## 概要

The [Web アプリケーションセキュリティプロジェクト® (OWASP) を開く](https://owasp.org/) 基盤は [**OWASP トップ 10**](https://owasp.org/www-project-top-ten/) web アプリケーションに関する 10 の最も重要なセキュリティ上の問題の概要を説明します。

ModSecurity は、Web アプリケーションに対する様々な攻撃から保護する、オープンソースのクロスプラットフォームソリューションです。 また、HTTP トラフィックの監視、ログ、リアルタイム分析も可能です。

OWSAP®は、 [OWASP® ModSecurity コアルールセット (CRS)](https://github.com/coreruleset/coreruleset). CRS は汎用のセットです **攻撃検出** ModSecurity で使用するルールです。 CRS は、OWASP Top 10 を含む幅広い攻撃から Web アプリケーションを保護し、最低限の誤ったアラートを提供することを目的としています。

このチュートリアルでは、 **DOS 保護** 潜在的な DoS 攻撃からサイトを保護する CRS ルール。

>[!TIP]
>
>AEMas a Cloud Serviceの [マネージド CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=ja) は、ほとんどのお客様のパフォーマンスとセキュリティの要件を満たします。 ただし、ModSecurity はセキュリティの追加層を提供し、お客様固有のルールおよび設定を可能にします。

## Dispatcher プロジェクトモジュールに CRS を追加する

1. をダウンロードして抽出します。 [最新の OWASP ModSecurity コアルールセット](https://github.com/coreruleset/coreruleset/releases).

   ```shell
   # Replace the X.Y.Z with relevent version numbers.
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/vX.Y.Z.tar.gz
   
   # For version v3.3.5 when this tutorial is published
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/v3.3.5.tar.gz
   
   # Extract the downloaded file
   $ tar -xvzf coreruleset-3.3.5.tar.gz
   ```

1. を作成します。 `modsec/crs` 内のフォルダ `dispatcher/src/conf.d/` をAEMプロジェクトのコードに追加します。 例えば、 [AEM WKND Sites プロジェクト](https://github.com/adobe/aem-guides-wknd).

   ![AEMプロジェクトコード内の CRS フォルダー — ModSecurity](assets/modsecurity-crs/crs-folder-in-aem-dispatcher-module.png){width="200" zoomable="yes"}

1. をコピーします。 `coreruleset-X.Y.Z/rules` ダウンロードした CRS リリースパッケージから `dispatcher/src/conf.d/modsec/crs` フォルダー。
1. をコピーします。 `coreruleset-X.Y.Z/crs-setup.conf.example` ファイルをダウンロードした CRS リリースパッケージから `dispatcher/src/conf.d/modsec/crs` フォルダーと名前をに変更します。 `crs-setup.conf`.
1. コピーしたすべての CRS ルールを `dispatcher/src/conf.d/modsec/crs/rules` 名前をに変更して、 `XXXX-XXX-XXX.conf.disabled`. 以下のコマンドを使用して、すべてのファイルの名前を一度に変更できます。

   ```shell
   # Go inside the newly created rules directory within the dispathcher module
   $ cd dispatcher/src/conf.d/modsec/crs/rules
   
   # Rename all '.conf' extension files to '.conf.disabled'
   $ for i in *.conf; do mv -- "$i" "$i.disabled"; done
   ```

   WKND プロジェクトコードで、名前が変更された CRS ルールと設定ファイルを参照してください。

   ![AEMプロジェクトコード内で CRS ルールを無効にしました — ModSecurity ](assets/modsecurity-crs/disabled-crs-rules.png){width="200" zoomable="yes"}

## サービス拒否 (DoS) 保護ルールを有効にして構成します

サービス拒否 (DoS) 保護ルールを有効にして設定するには、次の手順に従います。

1. 名前を変更して DoS 保護規則を有効にする `REQUEST-912-DOS-PROTECTION.conf.disabled` から `REQUEST-912-DOS-PROTECTION.conf` ( または `.disabled` （rulename 拡張から） `dispatcher/src/conf.d/modsec/crs/rules` フォルダー。
1. ルールを設定するには、  **DOS_COUNTER_THRESHOLD, DOS_BURST_TIME_SLICE, DOS_BLOCK_TIMEOUT** 変数。
   1. の作成 `crs-setup.custom.conf` ファイルを `dispatcher/src/conf.d/modsec/crs` フォルダー。
   1. 以下のルールスニペットを新しく作成したファイルに追加します。

   ```
   # The Denial of Service (DoS) protection against clients making requests too quickly.
   # When a client is making more than 25 requests (excluding static files) within
   # 60 seconds, this is considered a 'burst'. After two bursts, the client is
   # blocked for 600 seconds.
   SecAction \
       "id:900700,\
       phase:1,\
       nolog,\
       pass,\
       t:none,\
       setvar:'tx.dos_burst_time_slice=60',\
       setvar:'tx.dos_counter_threshold=25',\
       setvar:'tx.dos_block_timeout=600'"    
   ```

この例のルール設定では、 **DOS_COUNTER_THRESHOLD** は 25 で、 **DOS_BURST_TIME_SLICE** は 60 秒、 **DOS_BLOCK_TIMEOUT** タイムアウトは 600 秒です。 この設定では、静的ファイルを除く 25 件を超えるリクエスト (60 秒以内に DoS 攻撃と見なされ、その結果、リクエストクライアントが 600 秒（または 10 分）ブロックされます。

>[!WARNING]
>
>ニーズに適した値を定義するには、Web セキュリティチームと共同作業します。

## CRS を初期化します。

CRS を初期化し、一般的な偽陽性を削除して、サイトにローカルの例外を追加するには、次の手順に従います。

1. CRS を初期化するには、を削除します。 `.disabled` から **REQUEST-901-INITIALIZATION** ファイル。 つまり、 `REQUEST-901-INITIALIZATION.conf.disabled` ～にファイルを送る `REQUEST-901-INITIALIZATION.conf`.
1. ローカル IP (127.0.0.1) ping などの一般的な偽陽性を削除するには、 `.disabled` から **REQUEST-905-COMMON-EXCEPTIONS** ファイル。
1. AEMプラットフォームやサイト固有のパスなど、ローカルの例外を追加するには、 `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example` から `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf`
   1. 新しく名前を変更したファイルに、AEMプラットフォーム固有のパス例外を追加します。

   ```
   ########################################################
   # AEM as a Cloud Service exclusions                    #
   ########################################################
   # Ignoring AEM-CS Specific internal and reserved paths
   
   SecRule REQUEST_URI "@beginsWith /systemready" \
       "id:1010,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"    
   
   SecRule REQUEST_URI "@beginsWith /system/probes" \
       "id:1011,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   SecRule REQUEST_URI "@beginsWith /gitinit-status" \
       "id:1012,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   ########################################################
   # ADD YOUR SITE related exclusions                     #
   ########################################################
   ...
   ```

1. また、 `.disabled` から **REQUEST-910-IP-REPUTATION.conf.disabled** （IP レピュテーションブロックチェック用）および `REQUEST-949-BLOCKING-EVALUATION.conf.disabled` 異常値スコアチェック用。

>[!TIP]
>
>AEM 6.5 で設定する場合は、上記のパスを、AEMのヘルスを検証する AMS またはオンプレミスの各パス（ハートビートパス）に必ず置き換えてください。

## ModSecurity Apache 設定を追加

ModSecurity ( `mod_security` Apache モジュール )、次の手順に従います。

1. 作成 `modsecurity.conf` 時刻 `dispatcher/src/conf.d/modsec/modsecurity.conf` を次のキー設定に置き換えます。

   ```
   # Include the baseline crs setup
   Include conf.d/modsec/crs/crs-setup.conf
   
   # Include your customizations to crs setup if exist
   IncludeOptional conf.d/modsec/crs/crs-setup.custom.conf
   
   # Select all available CRS rules:
   #Include conf.d/modsec/crs/rules/*.conf
   
   # Or alternatively list only specific ones you want to enable e.g.
   Include conf.d/modsec/crs/rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
   Include conf.d/modsec/crs/rules/REQUEST-901-INITIALIZATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-905-COMMON-EXCEPTIONS.conf
   Include conf.d/modsec/crs/rules/REQUEST-910-IP-REPUTATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf
   Include conf.d/modsec/crs/rules/REQUEST-949-BLOCKING-EVALUATION.conf
   
   # Start initially with engine off, then switch to detection and observe, and when sure enable engine actions
   #SecRuleEngine Off
   #SecRuleEngine DetectionOnly
   SecRuleEngine On
   
   # Remember to use relative path for logs:
   SecDebugLog logs/httpd_mod_security_debug.log
   
   # Start with low debug level
   SecDebugLogLevel 0
   #SecDebugLogLevel 1
   
   # Start without auditing
   SecAuditEngine Off
   #SecAuditEngine RelevantOnly
   #SecAuditEngine On
   
   # Tune audit accordingly:
   SecAuditLogRelevantStatus "^(?:5|4(?!04))"
   SecAuditLogParts ABIJDEFHZ
   SecAuditLogType Serial
   
   # Remember to use relative path for logs:
   SecAuditLog logs/httpd_mod_security_audit.log
   
   # You might still use /tmp for temporary/work files:
   SecTmpDir /tmp
   SecDataDir /tmp
   ```

1. 目的のを選択します。 `.vhost` AEMプロジェクトの Dispatcher モジュールから `dispatcher/src/conf.d/available_vhosts`例： `wknd.vhost`をクリックし、 `<VirtualHost>` ブロック。

   ```
   # Enable the ModSecurity and OWASP CRS
   <IfModule mod_security2.c>
       Include conf.d/modsec/modsecurity.conf
   </IfModule>
   
   ...
   
   <VirtualHost *:80>
       ServerName    "publish"
       ...
   </VirtualHost>
   ```

上記すべて _ModSecurity CRS_ および _DOS 保護_ 設定は、AEM WKND Sites プロジェクトの [tutorial/enable-modsecurity-crs-dos-protection](https://github.com/adobe/aem-guides-wknd/tree/tutorial/enable-modsecurity-crs-dos-protection) 分岐を使用してレビューを実行します。

### Dispatcher 設定の検証

AEM as a Cloud Serviceを使用する場合は、 _Dispatcher の設定_ 変更の場合は、 `validate` のスクリプト [AEM SDK の Dispatcher ツール](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=ja).

```
# Go inside Dispatcher SDK 'bin' directory
$ cd <YOUR-AEM-SDK-DIR>/<DISPATCHER-SDK-DIR>/bin

# Validate the updated Dispatcher configurations
$ ./validate.sh <YOUR-AEM-PROJECT-CODE-DIR>/dispatcher/src
```

## デプロイ

Cloud Manager を使用して、ローカルで検証された Dispatcher 設定をデプロイします [Web 層](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?#web-tier-config) または [フルスタック](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?#full-stack-code) パイプライン。 また、 [迅速な開発環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=ja) ターンラウンド時間を短縮します。

## 確認

この例では、DoS 保護を検証するために、60 秒の間に 50 件を超えるリクエスト（25 件のリクエストしきい値に 2 回）を送信します。 ただし、これらのリクエストはAEM as a Cloud Serviceを通過する必要があります [ビルトイン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=ja) または任意の [その他の CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?#point-to-point-CDN) Web サイトを最初に開く。

CDN パススルーを実現する 1 つの方法は、 **各サイトページリクエストの新しいランダム値**.

60 秒など、多数の要求（50 以上）を短時間でトリガーする場合、Apache [JMeter](https://jmeter.apache.org/) または [Benchmark または Ab ツール](https://httpd.apache.org/docs/2.4/programs/ab.html) を使用できます。

### JMeter スクリプトを使用して DoS 攻撃をシミュレートする

JMeter を使用して DoS 攻撃をシミュレートするには、次の手順に従います。

1. [Apache JMeter のダウンロード](https://jmeter.apache.org/download_jmeter.cgi) および [install](https://jmeter.apache.org/usermanual/get-started.html#install) ローカルで
1. [実行](https://jmeter.apache.org/usermanual/get-started.html#running) それは、 `jmeter` スクリプトを `<JMETER-INSTALL-DIR>/bin` ディレクトリ。
1. サンプルを開く [WKND-DoS-Attack-Simulation-Test](assets/modsecurity-crs/WKND-DoS-Attack-Simulation-Test.jmx) JMX スクリプトを JMeter に送信するには、 **開く** ツールメニューを使用します。

   ![サンプルの WKND DoS Attack JMX Test Script - ModSecurity を開きます。](assets/modsecurity-crs/open-wknd-dos-attack-jmx-test-script.png)

1. を更新します。 **サーバー名または IP** フィールド値 _ホームページ_ および _アドベンチャーページ_ テストAEM環境 URL と一致する HTTP リクエストサンプラー。 サンプル JMeter スクリプトのその他の詳細を確認します。

   ![AEMサーバ名 HTTP リクエスト JMetere - ModSecurity](assets/modsecurity-crs/aem-server-name-http-request.png)

1. スクリプトを実行するには、 **開始** 」ボタンをクリックします。 スクリプトは、WKND サイトの _ホームページ_ および _アドベンチャーページ_. したがって、非静的ファイルに対する合計 100 件のリクエストに対して、DOS 攻撃の対象となります。 **DOS 保護** CRS ルールのカスタム設定。

   ![JMeter スクリプトを実行 — ModSecurity](assets/modsecurity-crs/execute-jmeter-script.png)

1. The **結果を表に表示** JMeter リスナーの表示 **失敗** 要求番号～ 53 以降の応答ステータス。

   ![表 JMeter での結果表示での失敗した応答 — ModSecurity](assets/modsecurity-crs/failed-response-jmeter.png)

1. The **503 HTTP 応答コード** 失敗したリクエストに対してが返される場合は、 **結果ツリーを表示** JMeter リスナー。

   ![503 応答 JMeter - ModSecurity](assets/modsecurity-crs/503-response-jmeter.png)

### ログの確認

ModSecurity ロガー設定は、DoS 攻撃インシデントの詳細を記録します。 詳細を表示するには、次の手順に従います。

1. をダウンロードして開きます。 `httpderror` のログファイル **Dispatcher を公開**.
1. 単語を検索 `burst` ログファイルで、 **エラー** 行

   ```
   Tue Aug 15 15:19:40.229262 2023 [security2:error] [pid 308:tid 140200050567992] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Warning. Operator GE matched 2 at IP:dos_burst_counter. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "265"] [id "912170"] [msg "Potential Denial of Service (DoS) Attack from 192.150.10.209 - # of Request Bursts: 2"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/content/wknd/us/en/adventures.html"] [unique_id "ZNuXi9ft_9sa85dovgTN5gAAANI"]
   
   ...
   
   Tue Aug 15 15:19:40.515237 2023 [security2:error] [pid 309:tid 140200051428152] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Access denied with connection close (phase 1). Operator EQ matched 0 at IP. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "120"] [id "912120"] [msg "Denial of Service (DoS) attack identified from 192.150.10.209 (1 hits since last alert)"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/us/en.html"] [unique_id "ZNuXjAN7ZtmIYHGpDEkmmwAAAQw"]
   ```

1. 次のような詳細を確認します。 _クライアント IP アドレス_、アクション、エラーメッセージおよびリクエストの詳細。

## ModSecurity のパフォーマンスへの影響

ModSecurity と関連ルールを有効にすると、パフォーマンスに多少の影響が生じるので、必要なルール、冗長ルール、スキップルールに注意してください。 Web セキュリティのエキスパートと協力して、CRS ルールを有効にし、カスタマイズします。

### 追加のルール

このチュートリアルでは、を有効にし、カスタマイズするだけです **DOS 保護** デモ用の CRS ルール。 適切なルールを理解、確認、設定するには、Web セキュリティの専門家と提携することをお勧めします。
