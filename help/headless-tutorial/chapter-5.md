---
title: 第5章 — Content Servicesページのオーサリング
description: AEMヘッドレスチュートリアルの第5章では、第4章で定義したテンプレートからページを作成する方法について説明します。 これらのページは、JSON HTTPエンドポイントとして機能します。
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 1%

---


# 第5章 — Content Servicesページのオーサリング

AEMヘッドレスチュートリアルの第5章では、第4章で定義したテンプレートからページを作成する方法について説明します。 この章で作成するページは、モバイルアプリのJSON HTTPエンドポイントとして機能します。

>[!NOTE]
>
> のページコンテンツアーキテクチャ `/content/wknd-mobile/en/api` は事前に構築されています。 の基本ページ `en``api` とは、建築および組織の目的を果たすものですが、厳密には必要とされません。 APIコンテンツをローカライズする場合は、AEM SitesのページのようにAPIページをローカライズできるので、通常の言語コピーおよびマルチサイトマネージャーページの組織のベストプラクティスに従うことをお勧めします。

## イベントAPIページの作成

1. **[!UICONTROL AEM] / [!UICONTROL サイト] / [!DNL WKND Mobile] / [!DNL English] /に移動しま[!DNL API]**&#x200B;す。
1. **APIページのラベルをタップし**、上部アクションバーの「 **作成** 」ボタンをタップして、APIページの下に新しいイベントAPIページを作成します。
   1. 上部のアクションバーで **「** 作成」をタップします
   1. Select **イベントAPI** template
   1. 「 **名前** 」フィールドに **イベントを入力します。**
   1. 「 **タイトル** 」フィールドに **イベントAPIと入力します。**
   1. 上部のアクションバーで「 **作成** 」をタップして、ページを作成します
   1. 「 **完了** 」をタップして、AEM Sites管理者に戻ります

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## イベントAPIページのオーサリング

>[!NOTE]
>
> プロジェクトには、オーサリングエクスペリエンスに基本的なスタイルをいくつか提供するためのCSSが用意されています。

1. **イベントAPI** ページを編集します。 **AEM/サイト/WKND Mobile/英語/APIに移動し、**&#x200B;イベントAPI **ページを選択し、トップアクションバーの****** Edit APIをタップして編集します。
1. ア追加セットファインダーから画像コンポーネントプレースホルダーにドラッグ&amp;ドロップして、アプリに表示するロゴ画像。 ****
   * 提供されているロゴは、にあり `/content/dam/wknd-mobile/images/wknd-logo.png`ます。

1. イベントの追加上に表示する **タグ行** 。
   1. 「 **テキスト** 」コンポーネントの編集
   1. テキストを入力します。
      * `The WKND is here.`

1. 表示する **イベント** :
   1. 「 **プロパティ** 」タブで次の設定を行います。
      * モデル： **イベント**
      * 親パス： **/content/dam/wknd-mobile/en/イベント**
      * タグ： **&lt;空白のままにしてください>**
   1. 「 **要素** 」タブで次の設定を行います。
      * リストに表示されているイベントを削除し、要素コンテンツフラグメントのすべての要素が公開されていることを確認します。

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## APIページのJSON出力の確認

JSONの出力とその形式は、 `.model.json` セレクターを使用してページをリクエストすることで確認できます。

このJSON構造(またはスキーマ)は、このAPIの利用者によく理解されている必要があります。 これは、APIの重要な消費者が、構造のどの側面が固定されているかを把握する(つまり、 イベントAPIのロゴ（画像）とタグライブ（テキスト）の、流動的な コンテンツフラグメントリストコンポーネントの下に表示されるイベント)。

公開済みAPIに関するこの契約を解除すると、アプリを消費する際に誤った動作が発生する可能性があります。

1. 新しいブラウザータブで、AEM Content ServicesのJSONエクスポーターを呼び出し、ページとコンポーネントを正規化された適切に定義されたJSON構造にシリアル化する `.model.json` セレクターを使用して、イベントーAPIページを要求します。

   これらのページで生成されるJSON構造は、アプリを整列させる必要のある構造です。

1. **イベントAPI** ページを **JSONとしてリクエストします**。

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   結果は次のように表示されます。

![AEM Content Services JSON出力](assets/chapter-5/json-output.png)

>[!NOTE]
>
> このJSONは、 **セレクターを使用して、人間が読みやすいように** Tidy `.tidy` （形式設定）形式で出力できます。
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 次の手順

必要に応じて、AEM Package Managerを使用してAEM Authorに [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) contentパッケージをインストールします [](http://localhost:4502/crx/packmgr/index.jsp)。 このパッケージには、チュートリアルのこの章と前の章で概要を説明している設定と内容が含まれています。

* [第6章 — JSONとして公開するAEMのコンテンツの公開](./chapter-6.md)
