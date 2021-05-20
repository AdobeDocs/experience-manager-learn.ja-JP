---
title: 第6章 — JSONとしてAEMパブリッシュにコンテンツを公開する — コンテンツサービス
description: AEMヘッドレスチュートリアルの第6章では、モバイルアプリからの使用を許可するために、必要なすべてのパッケージ、設定およびコンテンツをAEM Publish上で確認する方法について説明します。
feature: コンテンツフラグメント、API
topic: ヘッドレス、コンテンツ管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 2%

---


# 第6章 — 配信用にAEMパブリッシュにコンテンツを公開する

AEMヘッドレスチュートリアルの第6章では、モバイルアプリでの使用を許可するために必要なすべてのパッケージ、設定およびコンテンツをAEM Publishで確認する方法について説明します。

## AEM Content Servicesのコンテンツの公開

モバイルアプリがアクセスできるように、AEM Content Servicesを通じてイベントを駆動するために作成された設定とコンテンツをAEM Publishに公開する必要があります。

AEM Content Servicesは設定（コンテンツフラグメントモデル、編集可能テンプレート）、アセット（コンテンツフラグメント、画像）およびページから構築されているので、これらすべての要素でAEMコンテンツ管理機能を自動的に利用できます。以下が含まれます。

* レビューと処理のワークフロー
* AEMパブリッシュのAEM Content Servicesエンドポイントからのコンテンツのプッシュおよびプルに対するアクティベーション/非アクティブ化

1. [第1章](./chapter-1.md#wknd-mobile-application-packages)に示す&#x200B;**[!DNL WKND Mobile]アプリケーションパッケージ**&#x200B;が、[!UICONTROL パッケージマネージャー]を使用して&#x200B;**AEMパブリッシュ**&#x200B;にインストールされていることを確認します。
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. **[!DNL WKND Mobile Events API]編集可能なテンプレート**&#x200B;を公開します。
   1. **[!UICONTROL AEM] / [!UICONTROL ツール] / [!UICONTROL 一般] / [!UICONTROL テンプレート] /[!DNL WKND Mobile]**&#x200B;に移動します。
   1. **[!DNL Event API]**&#x200B;テンプレートを選択します。
   1. 上部のアクションバーで「**[!UICONTROL 公開]**」をタップします。
   1. **テンプレート**&#x200B;と&#x200B;**すべての参照**（コンテンツポリシー、コンテンツポリシーのマッピング、テンプレート）を公開します

1. **[!DNL WKND Mobile Events]コンテンツフラグメント**&#x200B;を公開します。

イベントAPIはコンテンツフラグメントリストコンポーネントを使用し、コンテンツフラグメントを特別に参照しないので、これが必要です。
1. **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Files] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**に移動します。
1.すべての**[!DNL Event]**コンテンツフラグメントを選択します
1.上部のアクションバーの「**[!UICONTROL 公開を管理]**」をタップします
1.デフォルトの「**公開**」アクションをそのままにして、上部のアクションバーの「**[!UICONTROL 次へ]**」をタップします
1. **すべての**コンテンツフラグメントを選択します。
1.上部のアクションバーで「**[!UICONTROL 公開]**」をタップします
* *[!DNL Events]コンテンツフラグメントモデルと参照イベント画像は、コンテンツフラグメントと共に自動的に公開されます。*

1. **[!DNL Events API]ページ**&#x200B;を公開します。
   1. **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**&#x200B;に移動します。
   1. **[!DNL Events]**&#x200B;ページを選択します。
   1. 上部のアクションバーの「**[!UICONTROL 公開を管理]**」をタップします
   1. デフォルトの「****&#x200B;を公開」アクションをそのまま使用し、上部のアクションバーで「**[!UICONTROL 次へ]**」をタップします
   1. **[!DNL Events]**&#x200B;ページを選択します。
   1. 上部のアクションバーの&#x200B;**[!DNL Publish]**&#x200B;をタップします。

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## AEMパブリッシュの検証

1. 新しいWebブラウザーで、AEMパブリッシュからログアウトし、次のURLを要求します（ `http://localhost:4503`は、AEMパブリッシュが実行されているホスト：ポートに置き換えます）。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   これらのリクエストは、対応するAEMオーサーエンドポイントがレビューされた場合と同じJSON応答を返す必要があります。 そうでない場合は、すべてのパブリケーションが成功したことを確認し（レプリケーションキューを確認）、 [!DNL WKND Mobile] `ui.apps`パッケージがAEMパブリッシュにインストールされ、AEMパブリッシュの`error.log`を確認します。

## 次の手順

インストールする追加のパッケージはありません。 この節で説明するコンテンツと設定がAEMパブリッシュに公開されていることを確認してください。公開されていない場合、後続の章は機能しません。

* [第7章 — モバイルアプリからのAEM Content Servicesの使用](./chapter-7.md)
