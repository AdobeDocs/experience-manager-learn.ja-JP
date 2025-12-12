---
title: AEM as a Cloud Service のキャッシュ
description: AEM as a Cloud Service のキャッシュの一般的な概要を説明します。
version: Experience Manager as a Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: e76ed4c5-3220-4274-a315-a75e549f8b40
duration: 36
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 100%

---

# AEM as a Cloud Service のキャッシュ

AEM as a Cloud Service では、キャッシュの理解が重要です。キャッシュには、システム効率を向上させ、読み込み時間を短縮するために、以前に取得したデータの保存と再利用が含まれます。このメカニズムにより、コンテンツ配信が大幅に高速化され、web サイトのパフォーマンスが向上し、ユーザーエクスペリエンスが最適化されます。

AEM as a Cloud Service には複数のキャッシュレイヤーがあり、オーサーサービスとパブリッシュサービスで異なる戦略があります。

![AEM as a Cloud Service のキャッシュの概要](./assets/overview/all.png){align="center"}

## AEM のキャッシュ

AEM as a Cloud Service には、CDN、AEM Dispatcher およびオプションで顧客の管理による CDN を含む、堅牢で設定可能なマルチレイヤーキャッシュ戦略があります。レイヤー全体のキャッシュを微調整してパフォーマンスを最適化し、AEM が最高のエクスペリエンスのみを提供できるようにします。AEM では、オーサーサービスとパブリッシュサービスに対してキャッシュの問題が異なります。各サービスのキャッシュ戦略については、以下で詳しく説明します。


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="AEM パブリッシュサービス" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="AEM パブリッシュサービスのキャッシュ">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="AEM パブリッシュサービスのキャッシュ">AEM パブリッシュサービスのキャッシュ</a></p>
            <p class="is-size-6">AEM パブリッシュサービスは、管理による CDN と AEM Dispatcher を使用して、エンドユーザーの web エクスペリエンスを最適化します。</p>
            <a href="./publish.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学ぶ</span>
 </a>
          </div>
        </div>
      </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Author service caching">
        <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
            <div class="card-image">
            <figure class="image is-16by9">
                <a href="./author.md" title="AEM オーサーサービスのキャッシュ" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="AEM オーサーサービスのキャッシュ">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="AEM オーサーサービスのキャッシュ">AEM オーサーサービスのキャッシュ</a></p>
                <p class="is-size-6">AEM オーサーサービスは、管理による CDN を使用して、最適化されたオーサリングエクスペリエンスを提供します。</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学ぶ</span>
 </a>
            </div>
            </div>
        </div>
    </div>
</div>
