---
title: AEMヘッドレスデプロイメント
description: AEMヘッドレスアプリの様々なデプロイメントに関する考慮事項について説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10794
thumbnail: kt-10794.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---


# AEMヘッドレスデプロイメント

AEMヘッドレスクライアントのデプロイメントには、多くの形式が必要です。AEMがホストするSPA、外部SPA、Web サイト、モバイルアプリ、さらにはサーバー間プロセス。

クライアントとそのデプロイ方法に応じて、AEMヘッドレスデプロイメントには様々な考慮事項があります。

## AEMサービスのアーキテクチャ

デプロイメントに関する考慮事項を調べる前に、AEMの論理アーキテクチャ、AEMas a Cloud Serviceのサービス層の分離と役割について理解することが不可欠です。 AEM as a Cloud Serviceは、次の 2 つの論理サービスで構成されます。

+ __AEM オーサー__ は、チームがコンテンツフラグメント（および他のアセット）を作成、共同作業および公開するサービスです。
+ __AEM パブリッシュ__ は、公開されたコンテンツフラグメント（および他のアセット）が、一般的に使用するためにレプリケートされるサービスです。
+ __AEMプレビュー__ は、AEM パブリッシュの動作を模倣するサービスですが、プレビューやレビューの目的でコンテンツが公開されているサービスです。 AEMプレビューは、内部オーディエンス向けであり、コンテンツの一般配信向けではありません。 目的のワークフローに応じて、AEMプレビューの使用はオプションです。

![AEMサービスのアーキテクチャ](./assets/overview/aem-service-architecture.png)

典型的なAEMas a Cloud Serviceヘッドレスデプロイメントアーキテクチャ_

実稼動環境で動作するAEMヘッドレスクライアントは通常、AEM パブリッシュとやり取りします。AEM パブリッシュには、承認済みの公開済みコンテンツが含まれます。 AEM オーサーインスタンスはデフォルトで安全で、すべてのリクエストに対する認証が必要で、進行中の作業中や未承認のコンテンツが含まれる場合があるので、AEM オーサーインスタンスとやり取りするクライアントは特に注意する必要があります。

## ヘッドレスクライアントの導入

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="シングルページアプリ (SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="シングルページアプリ (SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="シングルページアプリ (SPA)">シングルページアプリ (SPA)</a></p>
                   <p class="is-size-6">シングルページアプリ (SPA) のデプロイメントに関する考慮事項について説明します。</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学ぶ</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
<!-- Web component/JS -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web component/JS" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./web-component.md" title="Web コンポーネント/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Web コンポーネント/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Web コンポーネント/JS">Web コンポーネント/JS</a></p>
               <p class="is-size-6">Web コンポーネントとブラウザーベースの JavaScript ヘッドレスコンシューマー向けのデプロイメントに関する考慮事項について説明します。</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学ぶ</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Mobile apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./mobile.md" title="モバイルアプリ" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/mobile/mobile-card.png" alt="モバイルアプリ">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="モバイルアプリ">モバイルアプリ</a></p>
               <p class="is-size-6">モバイルアプリのデプロイに関する考慮事項について説明します。</p>
               <a href="./mobile.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学ぶ</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Server-to-server apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./server-to-server.md" title="サーバー間アプリ" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/server-to-server/server-to-server-card.png" alt="サーバー間アプリ">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./server-to-server.md" title="サーバー間アプリ">サーバー間アプリ</a></p>
               <p class="is-size-6">サーバー間アプリの展開に関する考慮事項について説明します</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学ぶ</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>