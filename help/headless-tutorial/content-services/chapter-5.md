---
title: 第5章 — コンテンツサービスのオーサリングページ — コンテンツサービス
description: AEMヘッドレスチュートリアルの第5章では、第4章で定義したテンプレートからのページの作成について説明します。 これらのページは、JSON HTTPエンドポイントとして機能します。
feature: コンテンツフラグメント、API
topic: ヘッドレス、コンテンツ管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 1%

---


# 第5章 — コンテンツサービスページのオーサリング

AEMヘッドレスチュートリアルの第5章では、第4章で定義したテンプレートからページを作成する方法について説明します。 この章で作成するページは、モバイルアプリのJSON HTTPエンドポイントとして機能します。

>[!NOTE]
>
> `/content/wknd-mobile/en/api`のページコンテンツアーキテクチャは事前に構築されています。 `en`と`api`のベースページは、アーキテクチャと組織の目的を果たしますが、厳密に必要とは限りません。 APIコンテンツをローカライズできる場合は、通常の言語コピーおよびマルチサイトマネージャーページの組織のベストプラクティスに従うことをお勧めします。APIページは、AEM Sitesのページのようにローカライズできるからです。

## イベントAPIページの作成

1. **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**&#x200B;に移動します。
1. **APIページのラベルをタップし**、上部のアクションバーの「作 **** 成」ボタンをタップして、APIページの下に新しいイベントAPIページを作成します。
   1. 上部のアクションバーの「**作成**」をタップします。
   1. **イベントAPI**&#x200B;テンプレートを選択します。
   1. 「**名前**」フィールドに、**events**&#x200B;と入力します。
   1. 「**タイトル**」フィールドに、「**イベントAPI**」と入力します。
   1. 上部のアクションバーの「**作成**」をタップして、ページを作成します
   1. 「**完了**」をタップして、AEM Sites管理者に戻ります。

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## イベントAPIページのオーサリング

>[!NOTE]
>
> このプロジェクトでは、オーサーエクスペリエンスに基本的なスタイルを提供するためにCSSを提供しています。

1. **イベントAPI**&#x200B;ページを編集します。そのためには、**AEM/Sites/WKND Mobile/English/API**&#x200B;に移動し、**イベントAPI**&#x200B;ページを選択して、上部のアクションバーの&#x200B;**Edit**&#x200B;をタップします。
1. アセットファインダーから画像コンポーネントプレースホルダーに&#x200B;**ロゴ画像**&#x200B;をドラッグ&amp;ドロップして、アプリに表示する画像を追加します。
   * `/content/dam/wknd-mobile/images/wknd-logo.png`にあるロゴを使用します。

1. **タグ行**&#x200B;を追加して、イベントの上に表示します。
   1. **テキスト**&#x200B;コンポーネントを編集します
   1. テキストを入力します。
      * `The WKND is here.`

1. 表示する&#x200B;**イベント**&#x200B;を選択します。
   1. 「**プロパティ**」タブで次の設定を行います。
      * モデル：**イベント**
      * 親パス：**/content/dam/wknd-mobile/en/events**
      * タグ：**&lt;空白のまま>**
   1. 「**要素**」タブで次の設定を行います。
      * リストに表示されている要素を削除して、イベントコンテンツフラグメントのすべての要素が公開されるようにします。

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## APIページのJSON出力の確認

`.model.json`セレクターを使用してページを要求することで、JSON出力とその形式を確認できます。

このJSON構造（またはスキーマ）は、このAPIの利用者によってよく理解されている必要があります。 APIの消費者は、構造のどの側面が固定されているかを把握することが重要です(つまり、 イベントAPIのロゴ（画像）とタグライブ（テキスト）。 コンテンツフラグメントリストコンポーネントの下に表示されるイベント)。

公開済みAPIでこの契約を解除すると、利用中のアプリで誤った動作が発生する可能性があります。

1. 新しいブラウザータブで、`.model.json`セレクターを使用してイベントAPIページを要求します。このセレクターは、AEM Content ServicesのJSONエクスポーターを呼び出し、ページとコンポーネントを正規化された適切に定義されたJSON構造にシリアル化します。

   これらのページで生成されるJSON構造は、アプリを使用する構造に合わせる必要があります。

1. **イベントAPI**&#x200B;ページを&#x200B;**JSON**&#x200B;としてリクエストします。

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   結果は次のようになります。

![AEM Content Services JSON出力](assets/chapter-5/json-output.png)

>[!NOTE]
>
> このJSONは、 `.tidy`セレクターを使用して、人間が読みやすいように&#x200B;**整然と** （形式）出力できます。
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 次の手順

必要に応じて、[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)を使用して、[com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)コンテンツパッケージをAEMオーサーにインストールします。 このパッケージには、チュートリアルのこの章および前の章で概要を説明した設定とコンテンツが含まれています。

* [第6章 — JSONとしてAEMパブリッシュにコンテンツを公開する](./chapter-6.md)
