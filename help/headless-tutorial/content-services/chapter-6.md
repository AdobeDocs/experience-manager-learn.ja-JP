---
title: 第 6 章 — JSON として AEM パブリッシュでのコンテンツの公開 — コンテンツサービス
description: AEMヘッドレスチュートリアルの第 6 章では、モバイルアプリからの使用を許可するために、必要なすべてのパッケージ、設定、コンテンツを AEM Publish 上で確認する方法について説明します。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# 第 6 章 — 配信用に AEM パブリッシュ上でコンテンツを公開

AEMヘッドレスチュートリアルの第 6 章では、モバイルアプリでの使用を許可するために、必要なすべてのパッケージ、設定、コンテンツを AEM Publish 上で確認する方法について説明します。

## AEM Content Services のコンテンツの公開

AEM Content Services を通じてイベントを駆動するために作成された設定とコンテンツを AEM Publish に公開して、モバイルアプリがアクセスできるようにする必要があります。

AEM Content Services は設定（コンテンツフラグメントモデル、編集可能テンプレート）、アセット（コンテンツフラグメント、画像）、ページから構築されているので、これらすべてでAEMコンテンツ管理機能を自動的に利用できます。

* レビューおよび処理のワークフロー
* AEM パブリッシュのAEM Content Services エンドポイントからコンテンツをプッシュおよびプルするためのアクティベーション/非アクティブ化

1. 次を確認します。 **[!DNL WKND Mobile]アプリケーションパッケージ**（にリストされている） [第 1 章](./chapter-1.md#wknd-mobile-application-packages)、がにインストールされている **AEM パブリッシュ** using [!UICONTROL パッケージマネージャー].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. を公開します。 **[!DNL WKND Mobile Events API]編集可能テンプレート**
   1. に移動します。 **[!UICONTROL AEM] > [!UICONTROL ツール] > [!UICONTROL 一般] > [!UICONTROL テンプレート] >[!DNL WKND Mobile]**
   1. を選択します。 **[!DNL Event API]** テンプレート
   1. タップ **[!UICONTROL 公開]** 上部のアクションバー
   1. を公開します。 **テンプレート** および **すべての参照** （コンテンツポリシー、コンテンツポリシーマッピング、テンプレート）

1. を公開します。 **[!DNL WKND Mobile Events]コンテンツフラグメント**.

   イベント API はコンテンツフラグメントを特に参照しないコンテンツフラグメントリストコンポーネントを使用するので、これが必要です。

   1. に移動します。 **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL ファイル] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. すべての **[!DNL Event]** コンテンツフラグメント
   1. 次をタップします。 **[!UICONTROL 公開を管理]** 上部のアクションバー
   1. デフォルトの **公開** アクションをそのまま、タップ **[!UICONTROL 次へ]** 上部のアクションバー
   1. 選択 **すべて** コンテンツフラグメント
   1. タップ **[!UICONTROL 公開]** 上部のアクションバー
      * *この [!DNL Events] コンテンツフラグメントモデルと参照イベント画像は、コンテンツフラグメントと共に自動的に公開されます。*

1. を公開します。 **[!DNL Events API]ページ**.
   1. に移動します。 **[!UICONTROL AEM] > [!UICONTROL サイト] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. を選択します。 **[!DNL Events]** ページ
   1. 次をタップします。 **[!UICONTROL 公開を管理]** 上部のアクションバー
   1. デフォルトの **公開** アクションをそのまま、タップ **[!UICONTROL 次へ]** 上部のアクションバー
   1. を選択します。 **[!DNL Events]** ページ
   1. タップ **[!DNL Publish]** 上部のアクションバー

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## AEM 公開を確認

1. 新しい Web ブラウザーで、AEM Publish からログアウトし、次の URL をリクエストします ( 代わりに `http://localhost:4503` （どのホストでも：port AEM Publish が実行されている場合）。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   これらのリクエストでは、対応する AEM オーサーエンドポイントがレビューされた場合と同じ JSON 応答を返す必要があります。 成功しない場合は、すべてのパブリケーションが成功したことを確認します（レプリケーションキューを確認します）。 [!DNL WKND Mobile] `ui.apps` パッケージが AEM パブリッシュにインストールされ、 `error.log` （AEM パブリッシュ用）

## 次の手順

インストールする追加のパッケージはありません。 この節で説明しているコンテンツと設定が AEM パブリッシュに公開されていることを確認してください。そうしないと、後続の章は機能しません。

* [第 7 章 — モバイルアプリからのAEM Content Services の使用](./chapter-7.md)
