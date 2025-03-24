---
title: AEM ヘッドレスデプロイメント
description: AEM ヘッドレスアプリの様々なデプロイメントに関する考慮事項について説明します。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
duration: 59
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 100%

---

# AEM ヘッドレスデプロイメント

AEM ヘッドレスクライアントのデプロイメントには、AEM がホストする SPA、外部 SPA、web サイト、モバイルアプリ、さらにはサーバー間プロセスという多くの種類があります。

クライアントとそのデプロイ方法に応じて、AEM ヘッドレスデプロイメントには様々な考慮事項があります。

## AEM サービスのアーキテクチャ

デプロイメントに関する考慮事項を確認する前に、AEM の論理アーキテクチャ、AEM as a Cloud Service のサービス層の分離と役割について理解することが不可欠です。 AEM as a Cloud Service は、次の 2 つの論理サービスで構成されます。

+ __AEM オーサー__&#x200B;は、チームがコンテンツフラグメント（および他のアセット）を作成、共同作業および公開するサービスです。
+ __AEM パブリッシュ__&#x200B;は、公開されたコンテンツフラグメント（および他のアセット）が、一般的な使用のためにレプリケートされるサービスです。
+ __AEM プレビュー__&#x200B;は、AEM パブリッシュの動作を模倣するサービスですが、プレビューやレビューの目的でコンテンツが公開されるサービスです。 AEM プレビューは、内部オーディエンス向けであり、コンテンツの一般配信向けではありません。 目的のワークフローに応じて、AEM プレビューの使用は任意です。

![AEM サービスのアーキテクチャ](./assets/overview/aem-service-architecture.png)

AEM as a Cloud Service ヘッドレスの典型的なデプロイメントアーキテクチャ_

通常、実稼動の処理能力で動作する AEM ヘッドレスクライアントは、承認された公開済みコンテンツが含まれる AEM パブリッシュとやり取りします。AEM オーサーインスタンスはデフォルトで安全であり、すべてのリクエストに対する認証が必要で、進行中の作業中や未承認のコンテンツが含まれる場合があるため、AEM オーサーインスタンスとやり取りするクライアントは特に注意する必要があります。

## ヘッドレスクライアントのデプロイメント

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="単一ページアプリ（SPA）" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="単一ページアプリ（SPA）">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="単一ページアプリ（SPA）">単一ページアプリ（SPA）</a></p>
                   <p class="is-size-6">単一ページアプリ（SPA）のデプロイメントに関する考慮事項について学びます。</p>
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
               <a href="./web-component.md" title="Web コンポーネント／JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Web コンポーネント／JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Web コンポーネント／JS">Web コンポーネント／JS</a></p>
               <p class="is-size-6">Web コンポーネントとブラウザーベースの JavaScript ヘッドレスコンシューマー向けのデプロイメントに関する考慮事項について学びます。</p>
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
               <p class="is-size-6">モバイルアプリのデプロイメントに関する考慮事項について学びます。</p>
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
               <p class="is-size-6">サーバー間アプリのデプロイメントに関する考慮事項について学びます。</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">学ぶ</span>
 </a>
           </div>
       </div>
   </div>
</div>
</div>
