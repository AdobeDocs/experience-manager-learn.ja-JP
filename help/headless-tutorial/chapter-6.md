---
title: AEMヘッドレス — 第6章 — JSONとして発行するAEMでのコンテンツの公開
description: AEMヘッドレスチュートリアルの第6章では、必要なパッケージ、設定、コンテンツをすべてAEM Publish上に置いて、モバイルアプリからの消費を許可する方法について説明します。
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 2%

---


# 第6章 — AEM Publish上のコンテンツを配信用に公開する

AEMヘッドレスチュートリアルの第6章では、必要なパッケージ、設定、コンテンツをすべてAEM Publish上に置いて、モバイルアプリでの利用を許可する方法について説明します。

## AEM Content Services用コンテンツの公開

AEM Content Servicesを通じてイベントを導くために作成した設定とコンテンツは、AEM Publishに公開して、Mobile Appがアクセスできるようにする必要があります。

AEM Content Servicesは設定（コンテンツフラグメントモデル、編集可能なテンプレート）、アセット（コンテンツフラグメント、画像）、ページから構築されているので、これらの要素はすべて自動的にAEMコンテンツ管理機能を利用します。次に例を示します。

* レビューと処理のワークフロー
* およびAEM PublishのAEM Content Servicesエンドポイントからコンテンツをプッシュおよびプルするためのアクティベーション/非アクティブ化

1. [第1章](./chapter-1.md#wknd-mobile-application-packages)に示す&#x200B;**[!DNL WKND Mobile]アプリケーションパッケージ**&#x200B;が、**AEMパブリッシュ**&#x200B;上に[!UICONTROL パッケージマネージャー]を使用してインストールされていることを確認します。
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. **[!DNL WKND Mobile Events API]編集可能なテンプレート**&#x200B;を公開
   1. **[!UICONTROL AEM] > [!UICONTROL ツール] > [!UICONTROL 一般] > [!UICONTROL テンプレート] >[!DNL WKND Mobile]**&#x200B;に移動します。
   1. **[!DNL Event API]**&#x200B;テンプレートを選択
   1. 上部のアクションバーで「**[!UICONTROL 発行]**」をタップします
   1. **template**&#x200B;と&#x200B;**すべての参照**&#x200B;を公開します（コンテンツポリシー、コンテンツポリシーのマッピング、テンプレート）

1. **[!DNL WKND Mobile Events]コンテンツフラグメント**&#x200B;を発行します。

イベントAPIがコンテンツフラグメントリストコンポーネントを使用する場合は、コンテンツフラグメントを特に参照しないので、これが必要になることに注意してください。
1. **[!UICONTROL AEM] > [!UICONTROL アセット] > [!UICONTROL ファイル] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**に移動します。
1.すべての**[!DNL Event]**コンテンツフラグメントを選択
1.上部のアクションバーにある「**[!UICONTROL パブリケーションの管理]**」をタップします
1.デフォルトの**「**&#x200B;を現状のまま発行」アクションをそのままにし、上部のアクションバーにある「**[!UICONTROL 次へ]**」をタップします
1. **すべての**コンテンツフラグメントを選択
1.上部のアクションバーにある「**[!UICONTROL 発行]**」をタップします
* *[!DNL Events]コンテンツフラグメントモデルと参照イベント画像は、コンテンツフラグメントと共に自動的に公開されます。*

1. **[!DNL Events API]ページ**&#x200B;を公開します。
   1. **[!UICONTROL AEM] > [!UICONTROL サイト] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**&#x200B;に移動します。
   1. **[!DNL Events]**&#x200B;ページを選択
   1. 上部のアクションバーにある&#x200B;**[!UICONTROL パブリケーションの管理]**&#x200B;をタップします
   1. デフォルトの&#x200B;**「**&#x200B;を公開」アクションをそのままにして、上部のアクションバーにある「**[!UICONTROL 次へ]**」をタップします
   1. **[!DNL Events]**&#x200B;ページを選択
   1. 上部のアクションバーで&#x200B;**[!DNL Publish]**&#x200B;をタップします

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## AEM Publishの検証

1. 新しいWebブラウザーで、AEM Publishからログアウトし、次のURLを要求します（ホスト：port AEM Publishが実行されているものは`http://localhost:4503`に置き換えます）。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   これらのリクエストは、対応するAEM Authorエンドポイントがレビューされた場合と同じJSON応答を返す必要があります。 そうでない場合は、すべてのパブリケーションが成功したことを確認（レプリケーションキューを確認）し、[!DNL WKND Mobile] `ui.apps`パッケージがAEM Publishにインストールされていることを確認し、`error.log` for AEM Publishを確認します。

## 次の手順

インストールする追加のパッケージはありません。 このセクションで説明するコンテンツと設定がAEM Publishに公開されていることを確認してください。そうしないと、後続のチャプターは機能しません。

* [第7章 — モバイルアプリからのAEM Content Servicesの利用](./chapter-7.md)
