---
title: 実際のユーザー監視 (RUM)
description: AEM as a Cloud Service Web サイトの Real User Monitoring(RUM) について説明します。
version: Cloud Service
feature: Operations
topic: Performance
role: Admin, Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 0
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-03-18T00:00:00Z
source-git-commit: 7c80bb25b79a77c4a0bb2bbedf8a7c338177b857
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 0%

---


# 実際のユーザー監視 (RUM)

AEM as a Cloud Service Web サイトの Real User Monitoring(RUM) について説明します。 RUM を有効にする方法、収集されるデータ、および RUM データを使用して Web サイト上のユーザーエクスペリエンスを最適化する方法を理解します。

## 概要

Real User Monitoring(RUM) は、 _ユーザーのインタラクションとエクスペリエンスを収集、測定、分析します。_ Web サイトをリアルタイムで表示できます。 サイト訪問者の行動、パフォーマンス、全体的なエクスペリエンスなど、サイト訪問者が Web サイトとどのように関わっているかに関するインサイトを提供します。 これは、サイトのページに小さな JavaScript コードを挿入することで実現されます。

RUM は、JavaScript コードを使用して、Web サイトとやり取りする際に、ユーザーのブラウザーから直接データを取り込みます。 このデータを使用して、パフォーマンスの問題を特定および診断し、ユーザーエクスペリエンスを最適化し、ビジネス成果を改善できます。

AEM as a Cloud Serviceの RUM 機能を使用すると、Web サイトのユーザーエクスペリエンスの包括的なビューが提供されます。 ユーザーが訪問した各ページ (URL) に対して、次の主要指標が取り込まれます。

- [最大のコンテンツペイント (LCP)](https://web.dev/articles/lcp)  — 読み込みパフォーマンスを測定します。
- [累積レイアウトシフト (CLS)](https://web.dev/articles/cls) ・視覚の安定性を測定する。
- [次のペイントとの相互作用 (INP)](https://web.dev/articles/inp) ・相互作用を測定する。
- ページビュー数 — あるページが表示された回数を測定します。

また、Web サイトの 404 件のエラーとページビューグラフも取り込みます。

LCP、CLS、INP の指標は、 [コア Web 仮想レポート](https://web.dev/articles/vitals) :web サイトの速度、応答性、視覚的安定性に関連する一連の指標です。 これらの指標は、Googleが Web サイトでのユーザーエクスペリエンスを測定するために使用し、検索エンジンのランク付けに重要です。

## RUM を有効にする

AEMCS Web サイトで RUM を有効にするには、 [Real User Monitoring Service の設定方法](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#how-to-set-up-the-rum-service).

AEMCS での RUM の主な詳細は次のとおりです。

- RUM は、AEMCS のパブリッシュサービスにのみ適用できます。つまり、JavaScript コードはパブリッシュ環境にのみ挿入されます。
- The `com.adobe.granite.webvitals.WebVitalsConfig` OSGi 設定は、Include パスと Exclude パスを制御します。これらはリポジトリパスで、URL パスではありません。
- デフォルト `/content` パスが含まれます。
- パスを除外するには、 `AEM_WEBVITALS_EXCLUDE` Cloud Manager 環境変数 ( [環境変数の追加](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#add-variables). パスはコンマで区切られます。
- OOTB コードは、ページへの JavaScript コードの挿入を担当します。

### 検証

Web サイトで RUM が有効になっているかどうかを確認するには、公開されたページのHTML・ソースを表示して、次のスクリプト・ブロックを検索します。

```html
...

<!-- Added before the closing </head> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('top');
    window.addEventListener('load', () => sampleRUM('load'));
    document.addEventListener('click', () => sampleRUM('click'));
</script>

...

<!-- Added before the closing </body> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('lazy');
    sampleRUM('cwv');
</script>
```

## RUM データ収集

- RUM データは、 `sampleRUM()` 関数を渡すことができます。 上記の例では、チェックポイントは次のとおりです。 `top`, `load`, `click`, `lazy`、および `cwv`.
- チェックポイントは、ページを読み込み、操作する際のシーケンス内の名前付きイベントです。

関連トピック [Real User Monitoring Service and Privacy](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#rum-service-and-privacy) および [収集されるデータ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#what-data-is-being-collected) を参照してください。

## RUM データビュー

必要な RUM データを表示するには、次の手順に従います。 `domainkey`Adobeは、RUM 設定の一部として提供します。 RUM ダッシュボードは、次の場所で利用できます。 [https://data.aem.live/](https://data.aem.live/) また、ドメインキーと url を使用してアクセスできます。

例えば、以下のスクリーンショットは、AEM WKND Web サイトの RUM ダッシュボードを示しています。

![RUM ダッシュボード](./assets/rum/RUM-Dashboard-WKND.png)

RUM ダッシュボードは、次の主要なインサイトを提供します。

- **パフォーマンス指標** - LCP、CLS、INP、Pageviews。
- **エラー指標** - 404 エラー。
- **Pageview グラフ**  — 時間の経過に伴うページビュー数。

## RUM データの使用方法

上記のインサイトを使用すると、Web サイト上のユーザーエクスペリエンスを最適化できます。 次に例を示します。

- LCP、CLS、INP を減らして、ページの読み込みパフォーマンスとインタラクティビティを向上させます。 詳しくは、 [LCP を改善する方法](https://web.dev/articles/lcp#improve-lcp), [CLS の改善方法](https://web.dev/articles/cls#improve-cls) および [INP の改善方法](https://web.dev/articles/inp#improve-inp)を参照してください。
- ユーザーエクスペリエンスを向上させるために、404 エラーを修正します。
- ユーザーの行動を理解し、コンテンツを最適化するには、ページビューグラフを分析します。

Adobeは、特にメジャーまたはマイナーリリースの後に、RUM ダッシュボードの定期的なレビューをお勧めします。

関連トピック [Real User Monitoring Service のメリットを受けられるユーザー](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#who-can-benefit-from-rum-service) を参照してください。
