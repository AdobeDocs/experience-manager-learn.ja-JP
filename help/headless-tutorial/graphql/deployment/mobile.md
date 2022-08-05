---
title: AEMヘッドレスモバイルデプロイメント
description: モバイルAEMヘッドレスデプロイメントのデプロイメントに関する考慮事項について説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10796
thumbnail: KT-10796.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 2%

---


# AEMヘッドレスモバイルデプロイメント

AEMヘッドレスモバイルデプロイメントは、iOS、Android™などのネイティブモバイルアプリです。 コンテンツをヘッドレスな方法で消費し、AEMとやり取りする

AEMヘッドレス API への HTTP 接続はブラウザーのコンテキストでは開始されないので、モバイルデプロイメントには最小限の設定が必要です。

## デプロイメント設定

モバイルアプリケーションのデプロイメントでは、次のデプロイメント設定をインプレースにする必要があります。

| モバイルアプリの接続先 | コンテンツ作成者 | AEM パブリッシュ | AEMプレビュー |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher フィルター](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| クロスオリジンリソース共有 (CORS) | ✘ | ✘ | ✘ |
| [AEMホスト](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## モバイルアプリの例

Adobeには、例えば、iOSおよび Android™モバイルアプリが用意されています。

<div class="columns is-multiline">
    <!-- iOS app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="iOS app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/ios-swiftui-app.md" title="iOSアプリ" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/ios-swiftui-app/ios-app-card.png" alt="iOSアプリ">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/ios-swiftui-app.md" title="iOSアプリ">iOSアプリ</a></p>
                   <p class="is-size-6">AEMヘッドレス GraphQL API のコンテンツを使用する、SwiftUI で記述されたiOSアプリの例です。</p>
                   <a href="../example-apps/ios-swiftui-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
    <!-- Android app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Android app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/android-app.md" title="Android™アプリ" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/android-java-app/android-app-card.png" alt="Android アプリ">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/android-app.md" title="Android™アプリ">Android™アプリ</a></p>
                   <p class="is-size-6">AEMヘッドレス GraphQL API のコンテンツを使用する Java™ Android™アプリの例です。</p>
                   <a href="../example-apps/android-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>


