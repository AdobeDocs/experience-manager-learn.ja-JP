---
title: 第2章 —イベントコンテンツフラグメントモデルの定義
seo-title: AEM Content Servicesの概要 — 第2章 —イベントコンテンツフラグメントモデルの定義
description: AEMヘッドレスチュートリアルの第2章では、イベントを作成するための正規化されたデータ構造とオーサリングインターフェイスを定義するために使用するコンテンツフラグメントモデルの有効化と定義を行います。
seo-description: AEMヘッドレスチュートリアルの第2章では、イベントを作成するための正規化されたデータ構造とオーサリングインターフェイスを定義するために使用するコンテンツフラグメントモデルの有効化と定義を行います。
translation-type: tm+mt
source-git-commit: 885e30dea2a21dff789c98bdc5beb2f758b806f3
workflow-type: tm+mt
source-wordcount: '968'
ht-degree: 9%

---


# 第2章 — コンテンツフラグメントモデルの使用

AEMコンテンツフラグメントモデルは、AEM作成者が生コンテンツの作成をテンプレート化するために使用できるコンテンツスキーマを定義します。 この方法は、足場またはフォームベースのオーサリングに似ています。 コンテンツフラグメントの主な概念は、コンテンツを作成する場合はプレゼンテーションにとらわれないという点です。つまり、AEM、単一ページアプリ、モバイルアプリなど、消費するアプリで、コンテンツの表示方法を制御する複数チャネル向けのコンテンツです。

コンテンツフラグメントの主な問題は、次の事項を確実に行うことです。

1. 作成者から正しいコンテンツが収集されます
2. コンテンツは、アプリケーションを利用する際に、分かりやすい構造化された形式で表示できます。

この章では、「イベント」のモデリングと作成のための正規化されたデータ構造とオーサリングインターフェイスの定義に使用するコンテンツフラグメントモデルの有効化と定義について説明します。

## コンテンツフラグメントモデルの有効化

コンテンツフラグメントモデル **は** 、AEM **Configuration Browserを使用して有効にする必要があります**。

コンテンツフラグメントモデルが設定に対して **有効になっていない場合** 、 **[!UICONTROL 作成]/[!UICONTROL コンテンツフラグメント]** ボタンは、関連するAEM設定に対しては表示されません。

>[!NOTE]
>
>AEM設定は、に格納される [コンテキスト対応テナント設定](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) のセット `/conf`です。 通常、AEMの設定は、AEM Sitesで管理される特定のウェブサイトや、コンテンツのサブセット（アセット、ページなど）を担当する事業部門に関連しています。 aemで
>
>設定がコンテンツ階層に影響を与えるには、そのコンテンツ階層の `cq:conf` プロパティを介して設定を参照する必要があります。 (これは、 [!DNL WKND Mobile] 次の手順5 **** の設定で行います)。
>
>設定を使用する場合、設定はすべてのコンテンツに適用され、設定する必要 `global``cq:conf` はありません。

1. 適切な設定を変更する権限を持つユーザーとしてAEM Authorにログインします。
   * このチュートリアルでは、 **admin** userを使用できます。
1. **[!UICONTROL ツール]/[!UICONTROL 一般]/[!UICONTROL 設定ブラウザに移動します。]**
1. 選択する横の **フォルダーアイコン****[!DNL WKND Mobile]** をタップし、左上の **[!UICONTROL 「編集]** 」ボタンをタップします。
1. 「 **[!UICONTROL コンテンツフラグメントモデル]**」を選択し、右上の「 **[!UICONTROL 保存して閉じる]** 」をタップします。

   これにより、 [!DNL WKND Mobile] 設定が適用されたアセットフォルダーコンテンツツリーのコンテンツフラグメントモデルが有効になります。

   >[!NOTE]
   >
   >この構成の変更は、 [!UICONTROL AEM Configuration] Web UIから元に戻すことはできません。 この設定を元に戻すには：
   >    
   >    1. Open [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. `/conf/wknd-mobile/settings/dam/cfm` に移動します。
   >    1. Delete the `models` node

   >    
   >この設定で作成された既存のコンテンツフラグメントモデルは、定義と共に削除され、その定義もに保存され `/conf/wknd-mobile/settings/dam/cfm/models`ます。

1. 設定を **[!DNL WKND Mobile]** アセットフォルダーに適用して、コンテンツフラグメントモデルのコンテンツフラグメントをアセットフォルダー階層内に作成できるようにします **[!DNL WKND Mobile]** 。

   1. **[!UICONTROL AEM]/[!UICONTROL アセット]/[!UICONTROL ファイルに移動します。]**
   1. WKND Mobile **フォルダーの選択**
   1. 上部のアクションバーにある「 **[!UICONTROL プロパティ]** 」ボタンをタップして、「 [!UICONTROL フォルダーのプロパティ」を開きます]
   1. 「 [!UICONTROL フォルダーのプロパティ]」で、「 **[!UICONTROL Cloud Services]** 」タブをタップします
   1. 「 **[!UICONTROL Cloud Configuration]** 」フィールドが **/conf/wknd-mobileに設定されていることを確認します。**
   1. 右上の「 **[!UICONTROL 保存して閉じる]** 」をタップして変更を保持

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

## 作成するコンテンツフラグメントモデルについて

コンテンツフラグメントモデルを定義する前に、導入するエクスペリエンスを確認し、必要なデータポイントをすべて取り込むことを確認します。 ここでは、モバイルアプリのデザインをレビューし、デザイン要素をコンテンツにマッピングして収集します。

イベントを定義するデータポイントは、次のように分類できます。

![コンテンツフラグメントモデルの作成](assets/chapter-2/design-to-model-mapping.png)

マッピングを利用して、イベントデータを収集し最終的に公開するために使用されるコンテンツフラグメントを定義できます。

## コンテンツフラグメントモデルの作成

1. **[!UICONTROL ツール]/[!UICONTROL アセット]/**&#x200B;コンテンツフラグメントモデルに移動します。
1. フォルダーをタップして開き **[!DNL WKND Mobile]** ます。
1. 「 **[!UICONTROL 作成]** 」をタップして、コンテンツフラグメントモデル作成ウィザードを開きます。
1. 「 **[!DNL Event]** モデルタイトル **** （説明はオプションです） *」と入力し、「* 作成 **** 」をタップして保存します。

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## コンテンツフラグメントモデルの構造の定義

1. **[!UICONTROL ツール]/[!UICONTROL アセット]/[!UICONTROL コンテンツフラグメントモデル]/に移動します[!DNL WKND]**。
1. コン **[!DNL Event]** テンツフラグメントモデルを選択し、上部のアクションバーで「 **[!UICONTROL 編集]** 」をタップします。
1. 右側の「 **[!UICONTROL Data Types](** データタイプ) **[!UICONTROL 」タブで]** 、 **[!DNL Question]** 1行のテキスト入力を左のドロップゾーンにドラッグして、フィールドを定義します。
1. 左側で「 **[!UICONTROL 1行」の新しいテキスト入力]** が選択され、右側で「 **[!UICONTROL プロパティ]** 」タブが選択されていることを確認します。 次のように、プロパティフィールドに入力します。

   * [!UICONTROL レンダリング時の名前] : `textfield`
   * [!UICONTROL フィールドラベル] : `Event Title`
   * [!UICONTROL プロパティ名] : `eventTitle`
   * [!UICONTROL 最大長] :25
   * [!UICONTROL 必須] : `Yes`

以下に定義する入力定義を使用してこれらの手順を繰り返し、残りのイベントコンテンツフラグメントモデルを作成します。

>[!NOTE]
>
> Androidアプリケーションが **これらの名前をキーオフにするようにプログラムされているので、「** プロパティ名」フィールドは完全に一致する必要があります。

### イベントの説明

* [!UICONTROL データタイプ] : `Multi-line text`
* [!UICONTROL フィールドラベル] : `Event Description`
* [!UICONTROL プロパティ名] : `eventDescription`
* [!UICONTROL デフォルトの種類] : `Rich text`

### イベント日時

* [!UICONTROL データタイプ] : `Date and time`
* [!UICONTROL フィールドラベル] : `Event Date and Time`
* [!UICONTROL プロパティ名] : `eventDateAndTime`
* [!UICONTROL 必須] : `Yes`

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
* [!UICONTROL 型]：`Integer`
* [!UICONTROL 必須] : `Yes`

### イベント画像

* [!UICONTROL データタイプ] : `Content Reference`
* [!UICONTROL レンダリング時の名前] : `contentreference`
* [!UICONTROL フィールドラベル] : `Event Image`
* [!UICONTROL プロパティ名] : `eventImage`
* [!UICONTROL ルートパス] : `/content/dam/wknd-mobile/images`
* [!UICONTROL 必須] : `Yes`

### ベニュー名

* [!UICONTROL データタイプ] : `Single-line text`
* [!UICONTROL レンダリング時の名前] : `textfield`
* [!UICONTROL フィールドラベル] : `Venue Name`
* [!UICONTROL プロパティ名] : `venueName`
* [!UICONTROL 最大長] :20
* [!UICONTROL 必須] : `Yes`

### ベニューシティ

* [!UICONTROL データタイプ] : `Enumeration`
* [!UICONTROL フィールドラベル] : `Venue City`
* [!UICONTROL プロパティ名] : `venueCity`
* [!UICONTROL オプション] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>**[!UICONTROL プロパティ名は]** 、JSONファイル内のキーと共にこの値が格納されるJCRプロパティ名の **** 両方を表します。 これは、コンテンツフラグメントモデルの存続期間にわたって変更されないセマンティック名にする必要があります。

コンテンツフラグメントモデルの作成が完了したら、次のような定義になるはずです。


![イベントコンテンツフラグメントモデル](assets/chapter-2/event-content-fragment-model.png)

## 次の手順

必要に応じて、AEM Package Managerを使用してAEM Authorに [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) contentパッケージをインストールします [](http://localhost:4502/crx/packmgr/index.jsp)。 このパッケージには、チュートリアルのこの部分で概要を説明する設定と内容が含まれています。

* [第3章 —イベントコンテンツフラグメントのオーサリング](./chapter-3.md)
