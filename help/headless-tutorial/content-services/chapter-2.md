---
title: 第 2 章 — イベントコンテンツフラグメントモデルの定義 — コンテンツサービス
seo-title: Getting Started with AEM Content Services - Chapter 2 - Defining Event Content Fragment Models
description: AEMヘッドレスチュートリアルの第 2 章では、イベントを作成するための正規化されたデータ構造とオーサリングインターフェイスの定義に使用するコンテンツフラグメントモデルの有効化と定義について説明します。
seo-description: Chapter 2 of the AEM Headless tutorial covers enabling and defining Content Fragment Models used to define a normalized data structure and authoring interface for creating Events.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 11%

---

# 第 2 章 — コンテンツフラグメントモデルの使用

AEMコンテンツフラグメントモデルは、AEM作成者が生コンテンツの作成をテンプレート化するために使用できるコンテンツスキーマを定義します。 この方法は、基礎モードやフォームベースのオーサリングに似ています。 コンテンツフラグメントの主な概念は、作成されたコンテンツがプレゼンテーションに依存しないことです。つまり、AEM、単一ページアプリ、モバイルアプリなど、利用中のアプリがコンテンツをユーザーに表示する方法を制御するマルチチャネルでの使用を目的としています。

コンテンツフラグメントの主な懸念事項は、次の点を確実にすることです。

1. 作成者から正しいコンテンツが収集されます
2. コンテンツは、使用するアプリケーションに対して、構造化され、よく理解された形式で公開できます。

この章では、「イベント」のモデリングと作成のための正規化されたデータ構造とオーサリングインターフェイスの定義に使用するコンテンツフラグメントモデルの有効化と定義について説明します。

## コンテンツフラグメントモデルの有効化

コンテンツフラグメントモデル **必須** を介して有効にされる **[AEM [!UICONTROL 設定ブラウザー]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=ja)**.

コンテンツフラグメントモデルが **not** 設定に対して有効にする場合、 **[!UICONTROL 作成] > [!UICONTROL コンテンツフラグメント]** ボタンは、関連するAEM設定には表示されません。

>[!NOTE]
>
>AEM設定は、 [コンテキスト対応テナント設定](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) ～の下に保存される `/conf`. 通常、AEMの設定は、AEM Sitesで管理される特定の Web サイトや、コンテンツのサブセット（アセット、ページなど）を担当するビジネスユニットと関連があります。 AEMの
>
>設定がコンテンツ階層に影響を与えるには、設定を `cq:conf` プロパティを設定します。 ( これは、 [!DNL WKND Mobile] の設定 **手順 5** を参照 )。
>
>次の場合に `global` 設定を使用し、設定をすべてのコンテンツに適用し、 `cq:conf` を設定する必要はありません。
>
>詳しくは、[[!UICONTROL 設定ブラウザー]のドキュメントを参照してください。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)

1. 適切な権限を持つユーザーとして AEM オーサーにログインし、関連する設定を変更します。
   * このチュートリアルでは、 **admin** ユーザーを使用できます。
1. に移動します。 **[!UICONTROL ツール] > [!UICONTROL 一般] > [!UICONTROL 設定ブラウザー]**
1. 次をタップします。 **フォルダーアイコン** 次の **[!DNL WKND Mobile]** をクリックし、 **[!UICONTROL 編集] ボタン** を左上に配置します。
1. 選択 **[!UICONTROL コンテンツフラグメントモデル]**&#x200B;をタップし、 **[!UICONTROL 保存して閉じる]** をクリックします。

   これにより、 [!DNL WKND Mobile] 設定が適用されました。

   >[!NOTE]
   >
   >この設定の変更は、 [!UICONTROL AEM設定] Web UI この設定を元に戻すには：
   >    
   >    1. 開く [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. `/conf/wknd-mobile/settings/dam/cfm` に移動します。
   >    1. を削除します。 `models` ノード

   >    
   >この設定で作成された既存のコンテンツフラグメントモデルはすべて削除され、定義は次の場所に保存されます。 `/conf/wknd-mobile/settings/dam/cfm/models`.

1. を適用します。 **[!DNL WKND Mobile]** 設定を **[!DNL WKND Mobile]アセットフォルダー** コンテンツフラグメントモデルのコンテンツフラグメントをそのアセットフォルダー階層内に作成できるようにするには：

   1. に移動します。 **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL ファイル]**
   1. を選択します。 **[!UICONTROL WKND Mobile] フォルダー**
   1. 次をタップします。 **[!UICONTROL プロパティ]** ボタンをクリックして開く [!UICONTROL フォルダーのプロパティ]
   1. In [!UICONTROL フォルダーのプロパティ]、 **[!UICONTROL Cloud Services]** タブ
   1. を確認します。 **[!UICONTROL クラウド設定]** フィールドが **/conf/wknd-mobile**
   1. タップ **[!UICONTROL 保存して閉じる]** 右上で変更を保持する

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

>[!WARNING]
>
> __コンテンツフラグメントモデル__ ～から引っ越される __ツール/アセット__ から __ツール/一般__.

## 作成するコンテンツフラグメントモデルについて

コンテンツフラグメントモデルを定義する前に、エクスペリエンスを確認し、必要なデータポイントをすべて取り込んでいることを確認しましょう。 ここでは、モバイルアプリケーションのデザインをレビューし、デザイン要素をコンテンツから収集へとマッピングします。

次のように、イベントを定義するデータポイントを分類できます。

![コンテンツフラグメントモデルの作成](assets/chapter-2/design-to-model-mapping.png)

マッピングを準備して、イベントデータを収集し最終的に公開するために使用されるコンテンツフラグメントを定義できます。

## コンテンツフラグメントモデルの作成

1. に移動します。 **[!UICONTROL ツール] > [!UICONTROL 一般] > [!UICONTROL コンテンツフラグメントモデル]**.
1. 次をタップします。 **[!DNL WKND Mobile]** 開くフォルダー。
1. タップ **[!UICONTROL 作成]** をクリックして、コンテンツフラグメントモデル作成ウィザードを開きます。
1. 入力 **[!DNL Event]** を **[!UICONTROL モデルタイトル]** *（説明はオプション）* とタップします。 **[!UICONTROL 作成]** 保存します。

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## コンテンツフラグメントモデルの構造の定義

1. に移動します。 **[!UICONTROL ツール] > [!UICONTROL 一般] > [!UICONTROL コンテンツフラグメントモデル] >[!DNL WKND]**.
1. を選択します。 **[!DNL Event]** コンテンツフラグメントモデルをタップし、 **[!UICONTROL 編集]** 」をクリックします。
1. 次の **[!UICONTROL データタイプ] タブ** 右側で、 **[!UICONTROL 1 行のテキスト入力]** を左側のドロップゾーンに追加し、 **[!DNL Question]** フィールドに入力します。
1. 新しい **[!UICONTROL 1 行のテキスト入力]** が左側で選択され、 **[!UICONTROL プロパティ] タブ** が右で選択されている。 「プロパティ」フィールドに以下のように値を入力します。

   * [!UICONTROL レンダリング時の名前] : `textfield`
   * [!UICONTROL フィールドラベル] : `Event Title`
   * [!UICONTROL プロパティ名] : `eventTitle`
   * [!UICONTROL 最大長] :25
   * [!UICONTROL 必須] : `Yes`

以下に定義する入力定義を使用して、これらの手順を繰り返し、残りのイベントコンテンツフラグメントモデルを作成します。

>[!NOTE]
>
> この **プロパティ名** Android アプリケーションがこれらの名前のキーをオフにするようにプログラムされているので、フィールドは完全に一致する必要があります。

### イベントの説明

* [!UICONTROL データタイプ] : `Multi-line text`
* [!UICONTROL フィールドラベル] : `Event Description`
* [!UICONTROL プロパティ名] : `eventDescription`
* [!UICONTROL デフォルトの種類] : `Rich text`

### イベントの日時

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
* [!UICONTROL 型] : `Integer`
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
>この **[!UICONTROL プロパティ名]** はを示します **両方** この値が格納される JCR プロパティ名と JSON ファイルのキー。 これは、コンテンツフラグメントモデルの存続期間中は変更されないセマンティック名にする必要があります。

コンテンツフラグメントモデルの作成が完了したら、最終的に次のような定義にする必要があります。


![イベントコンテンツフラグメントモデル](assets/chapter-2/event-content-fragment-model.png)

## 次の手順

オプションで、 [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) を介して AEM オーサー上のコンテンツパッケージ [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). このパッケージには、チュートリアルのこのパートで概要を説明する設定とコンテンツが含まれています。

* [第 3 章 — イベントコンテンツフラグメントのオーサリング](./chapter-3.md)
