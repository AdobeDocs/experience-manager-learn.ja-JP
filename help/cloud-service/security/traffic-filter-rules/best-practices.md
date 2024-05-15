---
title: WAF ルールを含むトラフィックフィルタールールのベストプラクティス
description: WAF ルールを含むトラフィックフィルタールールの推奨ベストプラクティスについて説明します。
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
duration: 170
source-git-commit: c7c78ca56c1d72f13d2dc80229a10704ab0f14ab
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 88%

---

# WAF ルールを含むトラフィックフィルタールールのベストプラクティス

WAF ルールを含むトラフィックフィルタールールの推奨ベストプラクティスについて説明します。この記事に記載されているベストプラクティスは完全なものではなく、独自のセキュリティポリシーや手順の代用になるものではない点に注意してください。

>[!VIDEO](https://video.tv.adobe.com/v/3425408?quality=12&learn=on)

## 一般的なベストプラクティス

- 組織に適したルールを決定するには、セキュリティチームと共同作業します。
- ルールをステージング環境と実稼動環境にデプロイする前に、開発環境でルールを常にテストします。
- ルールを宣言して検証する際は、`action` タイプ `log` から開始し、ルールが正当なトラフィックをブロックしていないことを常に確認します。
- 特定のルールでは、`log` から `block` への移行は、十分なサイトトラフィックの分析にのみ基づく必要があります。
- ルールを段階的に導入し、テストチーム（QA、パフォーマンス、侵入テスト）をプロセスに含めることを検討します。
- [ダッシュボードツール](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling)を使用して、ルールの影響を定期的に分析します。サイトのトラフィック量に応じて、分析は日別、週別、月別に実行できます。
- 分析後に判明した悪意のあるトラフィックをブロックするには、ルールを追加します。例えば、サイトを攻撃している特定の IP などです。
- ルールの作成、デプロイメント、分析は、継続的な反復的なプロセスである必要があります。これは 1 回限りのアクティビティです。

## トラフィックフィルタールールのベストプラクティス

AEM プロジェクトに対して、以下のトラフィックフィルタールールを有効にします。ただし、次の目的の値 `rateLimit` および `clientCountry` セキュリティ チームと協力してプロパティを決定する必要があります。

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
    # Block requests coming from OFAC countries
      - name: block-ofac-countries
        when:
          allOf:
              - reqProperty: tier
              - matches: publish
              - reqProperty: clientCountry
                in:
                  - SY
                  - BY
                  - MM
                  - KP
                  - IQ
                  - CD
                  - SD
                  - IR
                  - LR
                  - ZW
                  - CU
                  - CI
```

>[!WARNING]
>
>実稼動環境では、web セキュリティチームと共同作業して、`rateLimit` の適切な値を決定します。

## WAF ルールのベストプラクティス

WAF のライセンスを取得し、プログラムで有効にすると、ルールで宣言していない場合でも、WAF フラグに一致するトラフィックがグラフとリクエストログに表示されます。したがって、新しい悪意のあるトラフィックの可能性を常に認識しており、必要に応じてルールを作成できます。 宣言したルールに反映されていない WAF フラグを確認し、宣言することを検討します。

AEM プロジェクトでは、以下の WAF ルールを検討します。ただし、次の目的の値 `action` および `wafFlags` セキュリティ チームと協力してプロパティを決定してください。

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

    # Traffic Filter rules shown in above section
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
    # Disable protection against CMDEXE on /bin
      - name: allow-cdmexe-on-root-bin
        when:
          allOf:
            - reqProperty: tier
              matches: "author|publish"
            - reqProperty: path
              matches: "^/bin/.*"
        action:
          type: allow
          wafFlags:
            - CMDEXE
```

## 概要

最後に、このチュートリアルでは、Adobe Experience Manager as a Cloud Service（AEMCS）で web アプリケーションのセキュリティを強化するために必要な知識とツールを習得しました。実用的なルールの例と結果分析のインサイトを使用すると、web サイトとアプリケーションを効果的に保護できます。



