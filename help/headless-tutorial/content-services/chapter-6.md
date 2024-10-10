---
title: 第 6 章 - AEM Publish でコンテンツを JSON 形式で公開する - コンテンツサービス
description: AEM ヘッドレスチュートリアルの第 6 章では、モバイルアプリからの使用を許可するために、必要なすべてのパッケージ、設定、コンテンツを AEM Publish 上で確保する方法について説明します。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
duration: 196
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '453'
ht-degree: 100%

---

# 第 6 章 - AEM Publish 上でコンテンツを配信用に公開する

AEM ヘッドレスチュートリアルの第 6 章では、モバイルアプリでの使用を許可するために、必要なすべてのパッケージ、設定、コンテンツを AEM Publish 上で確保する方法について説明します。

## AEM Content Services でのコンテンツの公開

AEM Content Services を通じてイベントを始動するために作成された設定とコンテンツを AEM Publish に公開して、モバイルアプリがアクセスできるようにする必要があります。

AEM Content Services は設定（コンテンツフラグメントモデル、編集可能テンプレート）、アセット（コンテンツフラグメント、画像）、ページから構築されているため、以下の AEM コンテンツ管理機能を自動的に利用できます。

* レビューおよび処理のワークフロー
* AEM Publish の AEM Content Services エンドポイントからコンテンツをプッシュおよびプルするためのアクティベーション／アクティベーション解除

1. [第 1 章](./chapter-1.md#wknd-mobile-application-packages)にリストされている&#x200B;**[!DNL WKND Mobile]アプリケーションパッケージ**&#x200B;が、[!UICONTROL パッケージマネージャー]を使用して **AEM Publish** にインストールされていることを確認します。
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. **[!DNL WKND Mobile Events API]編集可能テンプレート**&#x200B;を公開します。
   1. **[!UICONTROL AEM]／[!UICONTROL ツール]／[!UICONTROL 全般]／[!UICONTROL テンプレート]／[!DNL WKND Mobile]**&#x200B;に移動します。
   1. **[!DNL Event API]** テンプレートを選択します。
   1. 上部のアクションバーの「**[!UICONTROL 公開]**」をタップします。
   1. **テンプレート**&#x200B;および&#x200B;**すべての参照**（コンテンツポリシー、コンテンツポリシーマッピング、テンプレート）を公開します。

1.  **[!DNL WKND Mobile Events]コンテンツフラグメント**&#x200B;を公開します。

   Events API は、具体的にコンテンツフラグメントを参照しないコンテンツフラグメントリストコンポーネントを使用するので、これが必要です。

   1. **[!UICONTROL AEM]／[!UICONTROL Assets]／[!UICONTROL ファイル]／[!DNL WKND Mobile]／[!DNL English]／[!DNL Events]**&#x200B;に移動します。
   1. すべての **[!DNL Event]** コンテンツフラグメントを選択します。
   1. 上部のアクションバーにある「**[!UICONTROL 公開を管理]**」をタップします。 
   1. デフォルトの「**公開**」アクションをそのままにし、上部のアクションバーにある「**[!UICONTROL 次へ]**」をタップします。  
   1. **すべて** のコンテンツフラグメントを選択します。
   1.  上部のアクションバーの「**[!UICONTROL 公開]**」をタップします。
      * *[!DNL Events] コンテンツフラグメントモデルと参照イベント画像は、コンテンツフラグメントと共に自動的に公開されます。*

1. **[!DNL Events API]ページ**&#x200B;を公開します。
   1. **[!UICONTROL AEM]／[!UICONTROL Sites]／[!DNL WKND Mobile]／[!DNL English]／[!DNL API]**&#x200B;に移動します。
   1. **[!DNL Events]** ページを選択します。
   1. 上部のアクションバーで「**[!UICONTROL 公開を管理]**」をタップします。 
   1. デフォルトの「**公開**」アクションはそのままにして、上部のアクションバーの「**[!UICONTROL 次へ]**」をタップします。
   1. **[!DNL Events]** ページを選択します。 
   1. 上部のアクションバーの「**[!DNL Publish]**」をタップします。

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## AEM Publish の確認

1. 新しい web ブラウザーで、AEM Publish からログアウトし、次の URL をリクエストします（ホスト：ポートで AEM Publish が実行されている場合は `http://localhost:4503`）。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   これらのリクエストでは、対応する AEM オーサーエンドポイントがレビューされた場合と同じ JSON 応答を返す必要があります。 そうでない場合は、すべてのパブリケーションが成功したことを確認します（レプリケーションキューをチェックします）。 [!DNL WKND Mobile] `ui.apps` パッケージが AEM Publish にインストールされるので、 AEM Publish の `error.log` を見直します。

## 次の手順

インストールする追加のパッケージはありません。 この節で説明されているコンテンツと設定は、必ず AEM Publish に公開してください。そうしないと、後続の章は機能しません。

* [第 7 章 - モバイルアプリからの AEM Content Services の使用](./chapter-7.md)
