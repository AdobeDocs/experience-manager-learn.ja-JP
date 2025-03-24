---
title: AEM ヘッドレスモバイルデプロイメント
description: モバイル AEM ヘッドレスデプロイメントのデプロイメントに関する考慮事項について説明します。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10796
thumbnail: KT-10796.jpg
exl-id: 1f536079-b3ce-4807-be88-804378e75d37
duration: 31
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 100%

---

# AEM ヘッドレスモバイルデプロイメント

AEM ヘッドレスモバイルデプロイメントは、AEM のコンテンツをヘッドレスで使用および操作する、iOS、Android™ などのネイティブモバイルアプリです。

AEM ヘッドレス API への HTTP 接続はブラウザーのコンテキストでは開始されないので、モバイルデプロイメントには最小限の設定が必要です。

## デプロイメント設定

モバイルアプリケーションのデプロイメントでは、次のデプロイメント設定が必要です。

| モバイルアプリの接続先 → | AEM オーサー | AEM パブリッシュ | AEM プレビュー |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher フィルター](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| クロスオリジンリソース共有（CORS） | ✘ | ✘ | ✘ |
| [AEM ホスト](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## モバイルアプリの例

アドべは、iOS および Android™ モバイルアプリの例を用意しています。

<div class="columns is-multiline">
    <!-- iOS app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="iOS app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/ios-swiftui-app.md" title="iOS アプリ" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/ios-swiftui-app/ios-app-card.png" alt="iOS アプリ">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/ios-swiftui-app.md" title="iOS アプリ">iOS アプリ</a></p>
                   <p class="is-size-6">AEM ヘッドレス GraphQL API のコンテンツを使用する、SwiftUI で記述された iOS アプリの例です。</p>
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
                   <a href="../example-apps/android-app.md" title="Android™ アプリ" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/android-java-app/android-app-card.png" alt="Android アプリ">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/android-app.md" title="Android™ アプリ">Android™ アプリ</a></p>
                   <p class="is-size-6">AEM ヘッドレス GraphQL API のコンテンツを使用する Java™ Android™ アプリの例です。</p>
                   <a href="../example-apps/android-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
