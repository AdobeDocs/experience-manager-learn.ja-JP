---
title: WAF ルールを含むトラフィックフィルタールールのベストプラクティス
description: WAF ルールを含むトラフィックフィルタールールの推奨ベストプラクティスについて説明します。
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-20T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: fa28ae232a5353eb34788fd2abe8402b42a62f66
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---


# WAF ルールを含むトラフィックフィルタールールのベストプラクティス

WAF ルールを含む、トラフィックフィルタールールに関する推奨ベストプラクティスを説明します。 この記事に記載されているベストプラクティスは完全なものではなく、独自のセキュリティポリシーや手順の代用になるものではないことに注意する必要があります。

## 一般的なベストプラクティス

- 組織に適したルールを決定するには、セキュリティチームと共同作業します。
- ルールをステージング環境と実稼動環境にデプロイする前に、必ず開発環境でルールをテストしてください。
- ルールの宣言と検証をおこなう場合は、常に次のように指定してください。 `action` type `log` ルールが正当なトラフィックをブロックしないようにするため。
- 特定のルールの場合、 `log` から `block` は、純粋に十分なサイトトラフィックの分析に基づくものです。
- ルールを段階的に導入し、テストチーム（QA、パフォーマンス、侵入テスト）をプロセスに含めることを検討します。
- ルールの影響を定期的に分析するには、 [ダッシュボードツール](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool). サイトのトラフィック量に応じて、分析は日別、週別、月別におこなわれます。
- 分析後に知っている可能性のある悪意のあるトラフィックをブロックするには、ルールを追加します。 例えば、サイトを攻撃している特定の IP などです。
- ルールの作成、デプロイメント、分析は、継続的な反復的なプロセスである必要があります。 これは 1 回限りのアクティビティではありません。

## トラフィックフィルタールールのベストプラクティス

AEMプロジェクトに対して、以下のトラフィックフィルタールールを有効にします。 ただし、 `rateLimit` および `clientCountry` プロパティは、セキュリティチームと連携して決定する必要があります。

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

## WAF ルールのベストプラクティス

WAF のライセンスが取得され、プログラムで有効になると、ルールで宣言しなかった場合でも、WAF フラグに一致するトラフィックがグラフやリクエストログに表示されます。 これにより、悪意のあるトラフィックが新たに発生した可能性が常に認識され、必要に応じてルールを作成することができます。 宣言されたルールに反映されていない WAF フラグを見て、宣言を検討してください。

AEMプロジェクトでは、以下の WAF ルールを考慮します。 ただし、 `action` および `wafFlags` プロパティは、セキュリティチームと連携して決定する必要があります。

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

## 概要

最後に、このチュートリアルでは、Adobe Experience Manager as a Cloud Service(AEMCS) での Web アプリケーションのセキュリティを強化するために必要な知識とツールを習得しました。 実用的なルールの例と結果の分析に対するインサイトを使用すると、Web サイトやアプリケーションを効果的に保護できます。
