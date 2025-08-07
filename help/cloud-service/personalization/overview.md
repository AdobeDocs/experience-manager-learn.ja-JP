---
title: パーソナライズ機能の概要
description: Adobe TargetおよびAdobe Experience Platform アプリケーションを使用してAEM as a Cloud Serviceの web サイトをパーソナライズする方法について説明します。
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18717
thumbnail: null
source-git-commit: 70665c019f63df1e736292ad24c47624a3a80d49
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 6%

---

# パーソナライズ機能の概要

AEM as a Cloud Service（AEMCS）をAdobe TargetおよびAdobe Experience Platform（AEP）と統合する方法について説明します。 A/B テストを使用してパーソナライズされたエクスペリエンスを提供する方法、行動に基づいてユーザーをターゲティングする方法、顧客プロファイルを使用してコンテンツをパーソナライズする方法を説明します。

## 前提条件

様々なパーソナライゼーションシナリオを示すために、このチュートリアルではサンプル [AEM WKND](https://github.com/adobe/aem-guides-wknd/) プロジェクトを使用します。 手順を実行するには、次が必要です。

- 次へのアクセス権を持つAdobe組織。
   - **AEM as a Cloud Service環境** - コンテンツを作成および管理します
   - **Adobe Target** - パーソナライズされたエクスペリエンスを作成して提供します
   - **Adobe Experience Platform アプリケーション** – 顧客プロファイルとオーディエンスを管理します
   - **AEPのタグ（旧称 Launch）** - データ収集およびパーソナライゼーションのために web SDKとカスタム JavaScriptをデプロイします

- AEM コンポーネントおよびエクスペリエンスフラグメントの基本的な理解

- AEM as a Cloud Service環境にデプロイされた [AEM WKND](https://github.com/adobe/aem-guides-wknd/) プロジェクト。

## 今すぐ始める

具体的な使用例を調査する前に、まずパーソナライゼーションにAEM as a Cloud Serviceを設定します。 まず、Adobe Targetと Tags を統合し、AEP web SDKを使用してクライアントサイドパーソナライゼーションを有効にします。 これらの基本的な手順により、AEM ページで実験、オーディエンスのターゲティングおよびリアルタイムパーソナライゼーションをサポートできます。

<!-- CARDS
{target = _self}

* ./setup/integrate-adobe-target.md
  {title = Integrate Adobe Target}
  {description = Integrate AEMCS with Adobe Target to activate personalized content as Adobe Target offers.}
  {image = ./assets/setup/integrate-target.png}
  {cta = Integrate Target}

* ./setup/integrate-adobe-tags.md
  {title = Integrate Tags}
  {description = Integrate AEMCS with Tags to inject the Web SDK and custom JavaScript for data collection and personalization.}
  {image = ./assets/setup/integrate-tags.png}
  {cta = Integrate Tags}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Adobe Target">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-target.md" title="Adobe Target を統合" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-target.png" alt="Adobe Target を統合"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" title="Adobe Target を統合">Adobe Targetの統合 </a>
                    </p>
                    <p class="is-size-6">AEMCS をAdobe Targetと統合して、パーソナライズされたコンテンツをAdobe Target オファーとしてアクティベートします。</p>
                </div>
                <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Target の統合 </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-tags.md" title="タグの統合" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-tags.png" alt="タグの統合"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" title="タグの統合"> タグの統合 </a>
                    </p>
                    <p class="is-size-6">AEMCS とタグを統合し、データ収集とパーソナライゼーションのために web SDKとカスタム JavaScriptを挿入します。</p>
                </div>
                <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> タグの統合 </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## ユースケース

AEMCS、Adobe TargetおよびAdobe Experience Platformでサポートされている以下の一般的なパーソナライゼーションのユースケースについて説明します。

<!-- CARDS
{target = _self}

* ./use-cases/experimentation.md
  {title = Experimentation (A/B Testing)}
  {description = Learn how to test different content variations in AEMCS using Adobe Target for A/B testing.}
  {image = ./assets/use-cases/experiment/experimentation.png}
  {cta = Learn Experimentation}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Experimentation (A/B Testing)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/experimentation.md" title="実験（A/B テスト）" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/experiment/experimentation.png" alt="実験（A/B テスト）"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/experimentation.md" target="_self" rel="referrer" title="実験（A/B テスト）"> 実験（A/B テスト） </a>
                    </p>
                    <p class="is-size-6">A/B テスト用のAdobe Targetを使用して、AEMCS で様々なコンテンツバリエーションをテストする方法を説明します。</p>
                </div>
                <a href="./use-cases/experimentation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> 実験について学ぶ </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



















