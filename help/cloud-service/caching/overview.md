---
title: AEMas a Cloud Serviceキャッシュ
description: AEMas a Cloud Serviceキャッシュの概要です。
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
source-git-commit: 6cbd8f3c49d44e75337715c35c198008da8ae7b9
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---


# AEMas a Cloud Serviceキャッシュ

AEMas a Cloud Serviceでは、キャッシュの理解が重要です。 キャッシュには、以前に取得したデータの保存と再利用が含まれるので、システムの効率が向上し、読み込み時間が短縮されます。 このメカニズムにより、コンテンツ配信が大幅に高速化され、Web サイトのパフォーマンスが向上し、ユーザーエクスペリエンスが最適化されます。

AEM as a Cloud Serviceには複数のキャッシュレイヤーと、オーサーサービスとパブリッシュサービスで異なる戦略があります。

![AEMas a Cloud Serviceキャッシュの概要](./assets/overview/all.png){align="center"}

## AEM caching

AEM as a Cloud Serviceには、CDN、AEM Dispatcher、およびオプションで顧客管理 CDN を含む、堅牢で設定可能な複数層キャッシュ戦略が用意されています。 レイヤー間のキャッシュは、パフォーマンスを最適化するように微調整でき、AEMが最適なエクスペリエンスのみを提供できるようにします。 AEMのオーサーサービスとパブリッシュサービスでは、キャッシュに関する問題が異なります。 以下の各サービスのキャッシュ戦略を参照してください。


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="AEM Publish Service" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="AEMパブリッシュサービスのキャッシュ">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="AEMパブリッシュサービスのキャッシュ">AEMパブリッシュサービスのキャッシュ</a></p>
            <p class="is-size-6">AEMパブリッシュサービスは、マネージド CDN とAEM Dispatcher を使用して、エンドユーザーの Web エクスペリエンスを最適化します。</p>
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
                <a href="./author.md" title="AEMオーサーサービスのキャッシュ" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="AEMオーサーサービスのキャッシュ">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="AEMオーサーサービスのキャッシュ">AEMオーサーサービスのキャッシュ</a></p>
                <p class="is-size-6">AEMオーサーサービスは、マネージド CDN を使用して、最適化されたオーサリングエクスペリエンスを提供します。</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学ぶ</span>
 </a>
            </div>
            </div>
        </div>
    </div>
</div>
