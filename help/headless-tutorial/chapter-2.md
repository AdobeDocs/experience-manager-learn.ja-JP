---
title: 第2章 —イベントコンテンツフラグメントモデルの定義
seo-title: AEM Content Servicesの概要 — 第2章 —イベントコンテンツフラグメントモデルの定義
description: AEMヘッドレスチュートリアルの第2章では、イベントを作成するための正規化されたデータ構造とオーサリングインターフェイスを定義するために使用するコンテンツフラグメントモデルの有効化と定義を行います。
seo-description: AEMヘッドレスチュートリアルの第2章では、イベントを作成するための正規化されたデータ構造とオーサリングインターフェイスを定義するために使用するコンテンツフラグメントモデルの有効化と定義を行います。
translation-type: tm+mt
source-git-commit: 1faf22f2e664b775c11e16cb1dfa18b363a7316b
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 10%

---


# 第2章 — コンテンツフラグメントモデルの使用

AEMコンテンツフラグメントモデルは、AEM作成者が生コンテンツの作成をテンプレート化するために使用できるコンテンツスキーマを定義します。 この方法は、足場またはフォームベースのオーサリングに似ています。 コンテンツフラグメントの主な概念は、コンテンツを作成する場合はプレゼンテーションにとらわれないという点です。つまり、AEM、単一ページアプリ、モバイルアプリなど、消費するアプリで、コンテンツの表示方法を制御する複数チャネル向けのコンテンツです。

コンテンツフラグメントの主な問題は、次の事項を確実に行うことです。

1. 作成者から正しいコンテンツが収集されます
2. コンテンツは、アプリケーションを利用する際に、分かりやすい構造化された形式で表示できます。

この章では、「イベント」のモデリングと作成のための正規化されたデータ構造とオーサリングインターフェイスの定義に使用するコンテンツフラグメントモデルの有効化と定義について説明します。

## コンテンツフラグメントモデルの有効化

コンテンツフラグメントモデル&#x200B;**は、**[ AEM [!UICONTROL 設定ブラウザー]](https://docs.adobe.com/content/help/ja-JP/experience-manager-cloud-service/implementing/developing/configurations.html)**を介して**&#x200B;有効にする必要があります。

コンテンツフラグメントモデルが設定に対して&#x200B;**有効**&#x200B;でない場合、**[!UICONTROL 作成] > [!UICONTROL コンテンツフラグメント]**&#x200B;ボタンは、関連するAEM設定に対しては表示されません。

>[!NOTE]
>
>AEM構成は、`/conf`の下に保存された[コンテキスト対応テナント構成](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html)のセットを表します。 通常、AEMの設定は、AEM Sitesで管理される特定のウェブサイトや、コンテンツのサブセット（アセット、ページなど）を担当する事業部門に関連しています。 aemで
>
>設定がコンテンツ階層に影響を与えるには、そのコンテンツ階層の`cq:conf`プロパティを介して設定を参照する必要があります。 （これは、**下の手順5**&#x200B;の[!DNL WKND Mobile]設定に対して行われます）。
>
>`global`設定を使用する場合、設定はすべてのコンテンツに適用され、`cq:conf`を設定する必要はありません。
>
>詳しくは、[[!UICONTROL 設定ブラウザー]のドキュメント](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)を参照してください。

1. 適切な設定を変更する権限を持つユーザーとしてAEM Authorにログインします。
   * このチュートリアルでは、**admin**&#x200B;ユーザーを使用できます。
1. **[!UICONTROL ツール] > [!UICONTROL 一般] > [!UICONTROL 設定ブラウザー]**&#x200B;に移動します
1. **[!DNL WKND Mobile]**&#x200B;の横の&#x200B;**フォルダーアイコン**&#x200B;をタップして選択し、左上の&#x200B;**[!UICONTROL 編集]ボタン**&#x200B;をタップします。
1. 「**[!UICONTROL コンテンツフラグメントモデル]**」を選択し、右上の「**[!UICONTROL 保存して閉じる]**」をタップします。

   これにより、[!DNL WKND Mobile]設定が適用されたアセットフォルダーコンテンツツリーのコンテンツフラグメントモデルが有効になります。

   >[!NOTE]
   >
   >この構成の変更は、[!UICONTROL AEM構成] Web UIから元に戻すことはできません。 この設定を元に戻すには：
   >    
   >    1. [CRXDE Lite](http://localhost:4502/crx/de)を開く
   >    1. `/conf/wknd-mobile/settings/dam/cfm` に移動します。
   >    1. `models`ノードを削除します

   >    
   >この設定で作成された既存のコンテンツフラグメントモデルは削除され、定義も`/conf/wknd-mobile/settings/dam/cfm/models`に保存されます。

1. **[!DNL WKND Mobile]**&#x200B;設定を&#x200B;**[!DNL WKND Mobile]アセットフォルダー**&#x200B;に適用し、コンテンツフラグメントモデルのコンテンツフラグメントをそのアセットフォルダー階層内に作成できるようにします。

   1. **[!UICONTROL AEM] > [!UICONTROL アセット] > [!UICONTROL ファイル]**&#x200B;に移動します
   1. **[!UICONTROL WKND Mobile]フォルダー**&#x200B;を選択
   1. 上部のアクションバーの「**[!UICONTROL プロパティ]**」ボタンをタップして、「[!UICONTROL フォルダーのプロパティ]」を開きます
   1. [!UICONTROL フォルダーのプロパティ]で、**[!UICONTROL Cloud Services]**&#x200B;タブをタップします
   1. **[!UICONTROL Cloud Configuration]**&#x200B;フィールドが&#x200B;**/conf/wknd-mobile**&#x200B;に設定されていることを確認します。
   1. 右上の「**[!UICONTROL 保存して閉じる]**」をタップして変更を保持

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

## 作成するコンテンツフラグメントモデルについて

コンテンツフラグメントモデルを定義する前に、導入するエクスペリエンスを確認し、必要なデータポイントをすべて取り込むことを確認します。 ここでは、モバイルアプリのデザインをレビューし、デザイン要素をコンテンツにマッピングして収集します。

イベントを定義するデータポイントは、次のように分類できます。

![コンテンツフラグメントモデルの作成](assets/chapter-2/design-to-model-mapping.png)

マッピングを利用して、イベントデータを収集し最終的に公開するために使用されるコンテンツフラグメントを定義できます。

## コンテンツフラグメントモデルの作成

1. **[!UICONTROL ツール] > [!UICONTROL アセット] > [!UICONTROL コンテンツフラグメントモデル]**&#x200B;に移動します。
1. **[!DNL WKND Mobile]**&#x200B;フォルダーをタップして開きます。
1. 「**[!UICONTROL 作成]**」をタップして、コンテンツフラグメントモデル作成ウィザードを開きます。
1. **[!DNL Event]**&#x200B;を&#x200B;**[!UICONTROL モデルタイトル]** *（説明はオプション）*&#x200B;と入力し、**[!UICONTROL 作成]**&#x200B;をタップして保存します。

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## コンテンツフラグメントモデルの構造の定義

1. **[!UICONTROL ツール] > [!UICONTROL アセット] > [!UICONTROL コンテンツフラグメントモデル] >[!DNL WKND]**&#x200B;に移動します。
1. 「**[!DNL Event]**&#x200B;コンテンツフラグメントモデル」を選択し、上部のアクションバーで「**[!UICONTROL 編集]**」をタップします。
1. 右側の&#x200B;**[!UICONTROL データタイプ]タブ**&#x200B;から、**[!UICONTROL 1行のテキスト入力]**&#x200B;を左のドロップゾーンにドラッグして、**[!DNL Question]**&#x200B;フィールドを定義します。
1. 左側で新しい&#x200B;**[!UICONTROL 1行のテキスト入力]**&#x200B;が選択され、右側で&#x200B;**[!UICONTROL プロパティ]タブ**&#x200B;が選択されていることを確認します。 次のように、プロパティフィールドに入力します。

   * [!UICONTROL レンダリング時の名前] : `textfield`
   * [!UICONTROL フィールドラベル] : `Event Title`
   * [!UICONTROL プロパティ名] : `eventTitle`
   * [!UICONTROL 最大長] :25
   * [!UICONTROL 必須] :  `Yes`

以下に定義する入力定義を使用してこれらの手順を繰り返し、残りのイベントコンテンツフラグメントモデルを作成します。

>[!NOTE]
>
> **プロパティ名**&#x200B;フィールドは、Androidアプリケーションがこれらの名前をキーオフにするようにプログラムされているので、完全に一致する必要があります。

### イベントの説明

* [!UICONTROL データタイプ] : `Multi-line text`
* [!UICONTROL フィールドラベル] : `Event Description`
* [!UICONTROL プロパティ名] : `eventDescription`
* [!UICONTROL デフォルトの種類] : `Rich text`

### イベント日時

* [!UICONTROL データタイプ] : `Date and time`
* [!UICONTROL フィールドラベル] : `Event Date and Time`
* [!UICONTROL プロパティ名] : `eventDateAndTime`
* [!UICONTROL 必須] :  `Yes`

### イベントタイプ

* [!UICONTROL データタイプ] : `Enumeration`
* [!UICONTROL フィールドラベル] : `Event Type`
* [!UICONTROL プロパティ名] : `eventType`
* [!UICONTROL オプション] : `Art,Music,Performance,Photography`

### チケット価格

* [!UICONTROL データタイプ] : `Number`
* [!UICONTROL レンダリング時の名前] : `numberfield`
* [!UICONTROL フィールドラベル] : `Ticket Price`
* [!UICONTROL プロパティ名] : `eventPrice`
* [!UICONTROL 型] : `Integer`
* [!UICONTROL 必須] :  `Yes`

### イベント画像

* [!UICONTROL データタイプ] : `Content Reference`
* [!UICONTROL レンダリング時の名前] : `contentreference`
* [!UICONTROL フィールドラベル] : `Event Image`
* [!UICONTROL プロパティ名] : `eventImage`
* [!UICONTROL ルートパス] : `/content/dam/wknd-mobile/images`
* [!UICONTROL 必須] :  `Yes`

### ベニュー名

* [!UICONTROL データタイプ] : `Single-line text`
* [!UICONTROL レンダリング時の名前] : `textfield`
* [!UICONTROL フィールドラベル] : `Venue Name`
* [!UICONTROL プロパティ名] : `venueName`
* [!UICONTROL 最大長] :20
* [!UICONTROL 必須] :  `Yes`

### ベニューシティ

* [!UICONTROL データタイプ] : `Enumeration`
* [!UICONTROL フィールドラベル] : `Venue City`
* [!UICONTROL プロパティ名] : `venueCity`
* [!UICONTROL オプション] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>**[!UICONTROL プロパティ名]**&#x200B;は、JSONファイル内のキーと共にこの値が保存されるJCRプロパティ名の&#x200B;**両方**&#x200B;を表します。 これは、コンテンツフラグメントモデルの存続期間にわたって変更されないセマンティック名にする必要があります。

コンテンツフラグメントモデルの作成が完了したら、次のような定義になるはずです。


![イベントコンテンツフラグメントモデル](assets/chapter-2/event-content-fragment-model.png)

## 次の手順

必要に応じて、[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)を介して、&lt;a0/>com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)コンテンツパッケージをAEM Authorにインストールします。 [このパッケージには、チュートリアルのこの部分で概要を説明する設定と内容が含まれています。

* [第3章 —イベントコンテンツフラグメントのオーサリング](./chapter-3.md)
