---
title: 第 5 章 - コンテンツサービスページのオーサリング - コンテンツサービス
description: AEM ヘッドレスチュートリアルの第 5 章では、第 4 章で定義したテンプレートからページを作成する方法について説明します。これらのページは、JSON HTTP エンドポイントとして機能します。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
duration: 189
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '570'
ht-degree: 100%

---

# 第 5 章 - コンテンツサービスページのオーサリング

AEM ヘッドレスチュートリアルの第 5 章では、第 4 章で定義したテンプレートからページを作成する方法について説明します。この章で作成するページは、モバイルアプリの JSON HTTP エンドポイントとして機能します。

>[!NOTE]
>
> `/content/wknd-mobile/en/api` のページコンテンツアーキテクチャは事前に定義されています。`en` と `api` のベースページは、アーキテクチャと組織に関する目的に役立ちますが、厳密には必須ではありません。API コンテンツをローカライズする場合、API ページはその他の AEM Sites ページと同様にローカライズできるため、通常の言語コピーや Multi-site Manager ページ組織のベストプラクティスに従うことが推奨されています。

## Event API ページの作成

1. **[!UICONTROL AEM]／[!UICONTROL Sites]／[!DNL WKND Mobile]／[!DNL English]／[!DNL API]**&#x200B;に移動します。
1. **API ページのラベルをタップ**&#x200B;し、上部のアクションバーにある「**作成**」ボタンをタップして、API ページの下に新規 Events API ページを作成します。
   1. 上部のアクションバーで「**作成**」 をタップします
   1. **Events API** テンプレートを選択します
   1. 「**名前**」フィールドに「**events**」と入力します
   1. 「**タイトル**」フィールドに「**Events API**」と入力します
   1. 上部のアクションバーで「**作成**」をクリックしてページを作成します
   1. 「**完了**」をクリックして、AEM Sites 管理者に戻ります

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## Events API ページのオーサリング

>[!NOTE]
>
> このプロジェクトでは、オーサーエクスペリエンスに基本的なスタイルをいくつか適用するために CSS を提供しています。

1. **AEM／Sites／WKND Mobile／英語／API** に移動し、「**Events API**」ページを選択し、上部のアクションバーで「**編集**」をクリックして、「**Events API**」ページを編集します。
1. **ロゴ画像**&#x200B;をアセットファインダーから画像コンポーネントプレースホルダーにドラッグ＆ドロップして、アプリに表示する画像を追加します。
   * `/content/dam/wknd-mobile/images/wknd-logo.png` にある指定されたロゴを使用します。

1. **タグ行**&#x200B;を追加して、イベントの上に表示します。
   1. **テキスト**&#x200B;コンポーネントを編集します
   1. 次のテキストを入力します
      * `The WKND is here.`

1. 表示する&#x200B;**イベント**&#x200B;を選択します
   1. 「**プロパティ**」タブで次の設定を行います
      * モデル：**イベント**
      * 親パス：**/content/dam/wknd-mobile/ja/events**
      * タグ：**&lt;Leave blank>**
   1. 「**要素**」タブで次の設定を行います
      * リストに表示されている要素をすべて削除して、イベントコンテンツフラグメントのすべての要素が公開されるようにします。

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## API ページの JSON 出力の確認

JSON 出力とその形式は、`.model.json` セレクターを使用してページを要求することで確認できます。

この API の利用者は、この JSON 構造（またはスキーマ）をよく理解しておく必要があります。API をご利用のお客様は、構造のどの側面が固定されているかを把握することが非常に重要です（流動的な Event API のロゴ（画像）とタグライブ（テキスト）（つまり、コンテンツフラグメントリストコンポーネントの下にリスト表示されるイベント）。

公開済み API でこの契約を解除すると、使用中のアプリで誤った動作が発生する可能性があります。

1. 新規ブラウザータブで、`.model.json` セレクターを使用してイベント API ページをリクエストします。これにより、AEM Content Services の JSON エクスポーターが呼び出され、ページとコンポーネントを正規化済みの適切に定義された JSON 構造にシリアル化します。

   これらのページで生成される JSON 構造は、アプリケーションを使用する構造に合わせる必要があります。

1. **Events API** ページを **JSON** としてリクエストします。

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   結果は次のようになります。

![AEM Content Services JSON 出力](assets/chapter-5/json-output.png)

>[!NOTE]
>
> この JSON は、`.tidy` セレクターを使用して、人間が読みやすいように&#x200B;**整えられた**（フォーマットされた）形で出力できます。
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

## 次の手順

必要に応じて、[com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) コンテンツパッケージを [AEM のパッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を通じて AEM オーサーにインストールします。このパッケージには、チュートリアルのこの章および前の章で概要を説明する設定とコンテンツが含まれています。

* [第 6 章 - コンテンツを JSON として AEM パブリッシュに公開する](./chapter-6.md)
