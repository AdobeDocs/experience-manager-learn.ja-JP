---
title: 第 5 章 — コンテンツサービスのオーサリングページ — コンテンツサービス
description: AEMヘッドレスチュートリアルの第 5 章では、第 4 章で定義したテンプレートからのページの作成について説明します。 これらのページは、JSON HTTP エンドポイントとして機能します。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 1%

---

# 第 5 章 — コンテンツサービスページのオーサリング

AEMヘッドレスチュートリアルの第 5 章では、第 4 章で定義したテンプレートからページを作成する方法について説明します。 この章で作成するページは、モバイルアプリの JSON HTTP エンドポイントとして機能します。

>[!NOTE]
>
> のページコンテンツアーキテクチャ `/content/wknd-mobile/en/api` は事前にビルドされています。 のベースページ `en` および `api` 建築や組織の目的を果たしますが、厳密に必要とされるわけではありません。 API コンテンツをローカライズする場合は、通常の言語コピーおよび Multi-site Manager ページ組織のベストプラクティスに従うことをお勧めします。API ページは、AEM Sitesページのようにローカライズできるからです。

## イベント API ページの作成

1. に移動します。 **[!UICONTROL AEM] > [!UICONTROL サイト] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **API ページのラベルをタップします**&#x200B;次に、 **作成** ボタンをクリックし、API ページの下に新しいイベント API ページを作成します。
   1. タップ **作成** 上部のアクションバー
   1. 選択 **イベント API** テンプレート
   1. 内 **名前** フィールド入力 **イベント**
   1. 内 **タイトル** フィールド入力 **イベント API**
   1. タップ **作成** をクリックしてページを作成します。
   1. タップ **完了** AEM Sites管理者に戻るには

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## イベント API ページのオーサリング

>[!NOTE]
>
> プロジェクトには、作成者エクスペリエンスに基本的なスタイルを提供するための CSS が用意されています。

1. を編集します。 **イベント API** 次に移動してページに移動： **AEM /サイト/ WKND Mobile /英語/ API**、選択 **イベント API** ページとタップ **編集** 」をクリックします。
1. を追加します。 **ロゴ画像** をクリックし、アセットファインダーから画像コンポーネントプレースホルダーにアプリをドラッグ&amp;ドロップして表示します。
   * 次の場所にある指定されたロゴを使用します。 `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. 追加 **タグ行** イベントの上に表示されます。
   1. を編集します。 **テキスト** コンポーネント
   1. テキストを入力します。
      * `The WKND is here.`

1. を選択します。 **イベント** 表示する
   1. で次の設定をおこないます。 **プロパティ** タブ：
      * モデル： **イベント**
      * 親パス： **/content/dam/wknd-mobile/en/events**
      * タグ： **&lt;leave blank=&quot;&quot;>**
   1. で次の設定をおこないます。 **要素** タブ：
      * リストに表示されている要素を削除して、イベントコンテンツフラグメントのすべての要素が公開されるようにします。

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## API ページの JSON 出力を確認する

JSON 出力とその形式は、 `.model.json` セレクター。

この JSON 構造（またはスキーマ）は、この API の利用者によってよく理解されている必要があります。 API をご利用のお客様は、構造のどの側面が固定されているかを把握することが非常に重要です ( すなわち、 イベント API のロゴ（画像）とタグ（ライブ）（テキスト）は流動的 ( コンテンツフラグメントリストコンポーネントの下に表示されるイベント )。

公開した API でこの契約を解除すると、使用中のアプリで誤った動作が発生する可能性があります。

1. 新しいブラウザータブで、 `.model.json` セレクター。AEM Content Services の JSON Exporter を呼び出し、ページとコンポーネントを、正規化された適切に定義された JSON 構造にシリアル化します。

   これらのページで生成される JSON 構造は、アプリを使用する際に適合する必要のある構造です。

1. をリクエストします。 **イベント API** ページ **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   結果は次のようになります。

![AEM Content Services JSON 出力](assets/chapter-5/json-output.png)

>[!NOTE]
>
> この JSON は **整頓する** （フォーマット済み）人間が読みやすいように、 `.tidy` セレクター：
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 次の手順

オプションで、 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) を介して AEM オーサー上のコンテンツパッケージ [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). このパッケージには、チュートリアルのこの章および前の章で概要を説明する設定とコンテンツが含まれています。

* [第 6 章 — JSON として AEM パブリッシュにコンテンツを公開](./chapter-6.md)
