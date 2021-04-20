---
title: 第5章 — Content Servicesページのオーサリング — Content Services
description: AEMヘッドレスチュートリアルの第5章では、第4章で定義したテンプレートからページを作成する方法について説明します。 これらのページは、JSON HTTPエンドポイントとして機能します。
feature: コンテンツフラグメント、API
topic: ヘッドレス、コンテンツ管理
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 1%

---


# 第5章 — Content Servicesページのオーサリング

AEMヘッドレスチュートリアルの第5章では、第4章で定義したテンプレートからページを作成する方法について説明します。 この章で作成するページは、モバイルアプリのJSON HTTPエンドポイントとして機能します。

>[!NOTE]
>
> `/content/wknd-mobile/en/api`のページコンテンツアーキテクチャは事前に構築されています。 `en`と`api`の基本ページは、建築や組織の目的を果たしますが、厳密には必須ではありません。 APIコンテンツをローカライズする場合は、AEM SitesのページのようにAPIページをローカライズできるので、通常の言語コピーおよびマルチサイトマネージャーページの組織のベストプラクティスに従うことをお勧めします。

## イベントAPIページの作成

1. **[!UICONTROL AEM] > [!UICONTROL サイト] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**&#x200B;に移動します。
1. **APIページのラベルをタップし**、上部のアクションバーにある「 **** 作成」ボタンをタップして、APIページの下に新しいイベントAPIページを作成します。
   1. 上部のアクションバーにある「**作成**」をタップします
   1. **イベントAPI**&#x200B;テンプレートを選択
   1. 「**名前**」フィールドに、**イベント**&#x200B;と入力します。
   1. **タイトル**&#x200B;フィールドに、**イベントAPI**&#x200B;と入力します。
   1. 上部のアクションバーにある「**作成**」をタップして、ページを作成します
   1. 「**完了**」をタップして、AEM Sites管理者に戻ります

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## イベントAPIページのオーサリング

>[!NOTE]
>
> プロジェクトには、オーサリングエクスペリエンスに基本的なスタイルをいくつか提供するためのCSSが用意されています。

1. **イベントAPI**&#x200B;ページを編集します。それには、**AEM/サイト/WKND Mobile/英語/API**&#x200B;に移動し、イベントAPI **ページを選択し、上部のアクションバーの「**&#x200B;編集&#x200B;**」をタップします。**
1. ア追加セットファインダーから画像コンポーネントプレースホルダーにドラッグ&amp;ドロップして、アプリに表示する&#x200B;**ロゴ画像**。
   * `/content/dam/wknd-mobile/images/wknd-logo.png`にある提供されたロゴを使用します。

1. イベント追加の上に表示する&#x200B;**タグ行**。
   1. **テキスト**&#x200B;コンポーネントの編集
   1. テキストを入力します。
      * `The WKND is here.`

1. 表示する&#x200B;**イベント**&#x200B;を選択：
   1. 「**プロパティ**」タブで次の設定を行います。
      * モデル：**イベント**
      * 親パス：**/content/dam/wknd-mobile/en/イベント**
      * タグ：**&lt;空白を残す>**
   1. 「**エレメント**」タブで次の設定を行います。
      * リストに表示されているイベントを削除し、要素コンテンツフラグメントのすべての要素が公開されていることを確認します。

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## APIページのJSON出力の確認

JSONの出力とその形式は、`.model.json`セレクターを使用してページをリクエストすることで確認できます。

このJSON構造(またはスキーマ)は、このAPIの利用者によく理解されている必要があります。 これは、APIの重要な消費者が、構造のどの側面が固定されているかを把握する(つまり、 イベントAPIのロゴ（画像）とタグライブ（テキスト）の、流動的な コンテンツフラグメントリストコンポーネントの下に表示されるイベント)。

公開済みAPIに関するこの契約を解除すると、アプリを消費する際に誤った動作が発生する可能性があります。

1. 新しいブラウザータブで、`.model.json`セレクターを使用してイベントーAPIページをリクエストします。このセレクターは、AEM Content ServicesのJSONエクスポーターを呼び出し、ページとコンポーネントを正規化された適切に定義されたJSON構造にシリアル化します。

   これらのページで生成されるJSON構造は、アプリを整列させる必要のある構造です。

1. **イベントAPI**&#x200B;ページを&#x200B;**JSON**&#x200B;としてリクエストします。

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   結果は次のように表示されます。

![AEM Content Services JSON出力](assets/chapter-5/json-output.png)

>[!NOTE]
>
> このJSONは、`.tidy`セレクターを使用して人間が読みやすいように&#x200B;**tidy**（フォーマット済み）形式で出力できます。
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 次の手順

必要に応じて、[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)を介して、[com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)コンテンツパッケージをAEM Authorにインストールします。 このパッケージには、チュートリアルのこの章と前の章で概要を説明している設定と内容が含まれています。

* [第6章 — JSONとして公開するAEMのコンテンツの公開](./chapter-6.md)
