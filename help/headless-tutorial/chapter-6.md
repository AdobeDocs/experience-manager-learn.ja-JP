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

1. 第1章に示す **[!DNL WKND Mobile]Application Packages**( [Package](./chapter-1.md#wknd-mobile-application-packages)1)が、Package Managerを使用して **AEM Publish** にインストールされていることを確認し ます。
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 編集可能なテンプレートの **[!DNL WKND Mobile Events API]公開**
   1. **[!UICONTROL AEM]/[!UICONTROL ツール]/一般[!UICONTROL /]テンプレート/テンプレートに移動します。[!DNL WKND Mobile]**
   1. Select the **[!DNL Event API]** template
   1. 上部のアクションバーで **[!UICONTROL 「公開]** 」をタップします
   1. テン **プレート** と **すべての参照** （コンテンツポリシー、コンテンツポリシーのマッピング、テンプレート）の公開

1. コンテンツフラグメントを発行 **[!DNL WKND Mobile Events]します**。

イベントAPIがコンテンツフラグメントリストコンポーネントを使用する場合は、コンテンツフラグメントを特に参照しないので、これが必要になることに注意してください。
AEM **[!UICONTROL > Assets]>[!UICONTROL Files]> All[!UICONTROL Files >]Select 1に移動します。 Select 1Select Top 1はTop 1Select Top 1Select Top 1はTop PupSelect Pun Pun Bat[!DNL WKND Mobile]Pun Bat Pun 1.SentはSenSenSenSenSenSent1に管理T1. TheSenSentSen[!DNL English]TheT1.TSer The The The Enter[!DNL Events]** TTT1.TSenTp.P.Tp.P.Sen The The TheTap top action bar* top action bar **[!DNL Event]***********************[!DNL Events]Fragment Modelをタップすると、コンテンツフラグメントと共にイベント画像が自動的に公開されます。*

1. Publish the **[!DNL Events API]page**.
   1. **[!UICONTROL AEM]/[!UICONTROL サイト]/[!DNL WKND Mobile]/[!DNL English]/[!DNL API]**
   1. ページを選択し **[!DNL Events]** ます
   1. 上部のアクションバーで **[!UICONTROL 「文書の]** 管理」をタップします
   1. デフォルトの **公開** 操作をそのままにして、上部のアクションバーの「 **[!UICONTROL 次へ]** 」をタップします
   1. ページを選択し **[!DNL Events]** ます
   1. 上部 **[!DNL Publish]** のアクションバーでをタップします

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## AEM Publishの検証

1. 新しいWebブラウザーで、AEM Publishからログアウトし、次のURLをリクエストします（AEM Publishが実行されているホスト：portを代わりに使用）。 `http://localhost:4503`

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   これらのリクエストは、対応するAEM Authorエンドポイントがレビューされた場合と同じJSON応答を返す必要があります。 これらのパブリケーションが存在しない場合は、すべてのパブリケーションが正常に完了したことを確認（レプリケーションキューを確認）し、 [!DNL WKND Mobile] パッケージがAEM Publishにインストールされて、AEM Publish `ui.apps``error.log` 用のパブリケーションを確認します。

## 次の手順

インストールする追加のパッケージはありません。 このセクションで説明するコンテンツと設定がAEM Publishに公開されていることを確認してください。そうしないと、後続のチャプターは機能しません。

* [第7章 — モバイルアプリからのAEM Content Servicesの利用](./chapter-7.md)
