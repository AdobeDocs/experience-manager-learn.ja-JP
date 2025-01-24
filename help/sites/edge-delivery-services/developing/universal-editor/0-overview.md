---
title: Edge Delivery Servicesとユニバーサルエディターの開発者向けチュートリアル
description: AEM ユニバーサルエディターで作成し、Edge Delivery Servicesを使用して配信される新しい web サイトの開発の基本について説明します。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Catalog
jira: KT-15832
duration: 89
exl-id: aeac08a2-75a0-4adb-b32e-0e7f85e7eb1d
source-git-commit: aa8ea183639c4c63be74f7ef1ce099c89454c099
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 7%

---

# Edge Delivery Servicesとユニバーサルエディターの開発者向けチュートリアル

![Edge Delivery Servicesとユニバーサルエディターの開発者向けチュートリアル ](./assets/0-overview/hero.png)

このチュートリアルでは、強力なオーサリングと、ユニバーサルエディターおよびEdge Delivery Servicesを使用した超高速配信を組み合わせたAEM web サイトを構築するための基本を説明します。 最後に、新しいプロジェクトを作成する方法、ローカル開発環境を設定する方法、新しいブロックを作成する方法の基本的な理解が得られます。

## プロジェクトのセットアップ

AEM as a Cloud Serviceでコードプロジェクトを作成し、新しいサイトを設定する方法について説明します。 このセットアップにより、ユニバーサルエディターとのシームレスな開発が可能になり、コンテンツの作成と、Edge Delivery Servicesを通じた迅速なコンテンツ配信が可能になります。

<!-- XCARDS 

* ./1-new-code-project.md
* ./2-new-aem-site.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create a code project">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./1-new-code-project.md" title="コードプロジェクトの作成" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/1-new-project/new-project.png" alt="コードプロジェクトの作成"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./1-new-code-project.md" target="_blank" rel="referrer" title="コードプロジェクトの作成"> コードプロジェクトの作成 </a>
                    </p>
                    <p class="is-size-6">ユニバーサルエディターを使用して編集可能な、Edge Delivery Services用のコードプロジェクトを作成します。</p>
                </div>
                <a href="./1-new-code-project.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create an AEM site">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./2-new-aem-site.md" title="AEM サイトの作成" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/2-new-aem-site/new-site.png" alt="AEM サイトの作成"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./2-new-aem-site.md" target="_blank" rel="referrer" title="AEM サイトの作成">AEM サイトの作成 </a>
                    </p>
                    <p class="is-size-6">AEM Sitesで、ユニバーサルエディターを使用して編集可能なEdge Delivery Services用のサイトを作成します。</p>
                </div>
                <a href="./2-new-aem-site.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 開発設定

迅速な web サイトローカル開発を可能にする開発環境の設定方法を説明します。 このセットアップにより、ユニバーサルエディターを使用したシームレスなサイト作成と、Edge Delivery Servicesを通じた効率的なコンテンツ配信が可能になり、最適化されたスムーズな開発ワークフローが実現します。
<!-- XCARDS 

* ./3-local-development-environment.md
* ./4-website-branding.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up a local development environment">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./3-local-development-environment.md" title="ローカル開発環境を設定" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/3-local-development-environment/github-clone.png" alt="ローカル開発環境を設定"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./3-local-development-environment.md" target="_blank" rel="referrer" title="ローカル開発環境を設定">ローカル開発環境のセットアップ </a>
                    </p>
                    <p class="is-size-6">Edge Delivery Servicesが配信され、ユニバーサルエディターで編集可能なサイトのローカル開発環境を設定します。</p>
                </div>
                <a href="./3-local-development-environment.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Add website branding">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./4-website-branding.md" title="Web サイトのブランディングの追加" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/4-website-branding/github-issues.png" alt="Web サイトのブランディングの追加"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./4-website-branding.md" target="_blank" rel="referrer" title="Web サイトのブランディングの追加">Web サイトのブランディングを追加 </a>
                    </p>
                    <p class="is-size-6">Edge Delivery Servicesサイトのグローバル CSS、CSS 変数および web フォントを定義します。</p>
                </div>
                <a href="./4-website-branding.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## ブロック開発

コンテンツモデルを定義し、テストおよび開発用にサンプルコンテンツを設定することで、新しいブロックを作成する方法を説明します。 ブロックをレンダリングする 2 つの方法を検討し、AEMとEdge Delivery Servicesで最適なパフォーマンスと柔軟性を得るためにブロックを構築する方法を理解します。

<!-- CARDS 

* ./5-new-block.md {image = ./assets/5-new-block/card.png}
* ./6-author-block.md {image = ./assets/6-author-block/card.png}
* ./7a-block-css.md {image = ./assets/7a-block-css/card.png}
* ./7b-block-js-css.md {image = ./assets/7b-block-js-css/card.png}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create a block">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./5-new-block.md" title="ブロックの作成" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/5-new-block/teaser-block.png" alt="ブロックの作成"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./5-new-block.md" target="_blank" rel="referrer" title="ブロックの作成"> ブロックの作成 </a>
                    </p>
                    <p class="is-size-6">ユニバーサルエディターで編集可能なEdge Delivery Services web サイトのブロックを作成します。</p>
                </div>
                <a href="./5-new-block.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Author a block">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./6-author-block.md" title="ブロックのオーサリング" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/6-author-block/open-new-site.png" alt="ブロックのオーサリング"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./6-author-block.md" target="_blank" rel="referrer" title="ブロックのオーサリング"> ブロックのオーサリング </a>
                    </p>
                    <p class="is-size-6">ユニバーサルエディターを使用してEdge Delivery Services ブロックを作成します。</p>
                </div>
                <a href="./6-author-block.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Develop a block with CSS">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./7a-block-css.md" title="CSS を使用したブロックの開発" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/7a-block-css/inspect-block-dom.png" alt="CSS を使用したブロックの開発"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./7a-block-css.md" target="_blank" rel="referrer" title="CSS を使用したブロックの開発">CSS を使用したブロックの開発 </a>
                    </p>
                    <p class="is-size-6">ユニバーサルエディターを使用して編集可能な、Edge Delivery Services用の CSS を使用したブロックを開発します。</p>
                </div>
                <a href="./7a-block-css.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Develop a block with CSS and JS">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./7b-block-js-css.md" title="CSS と JS を使用したブロックの開発" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/7a-block-css/inspect-block-dom.png" alt="CSS と JS を使用したブロックの開発"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./7b-block-js-css.md" target="_blank" rel="referrer" title="CSS と JS を使用したブロックの開発">CSS と JS を使用したブロックの開発 </a>
                    </p>
                    <p class="is-size-6">ユニバーサルエディターを使用して編集可能な、CSS とJavaScriptを使用したEdge Delivery Services用ブロックを作成します。</p>
                </div>
                <a href="./7b-block-js-css.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
