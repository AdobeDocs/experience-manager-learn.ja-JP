---
title: 第4章 — Content Servicesのテンプレートの定義
seo-title: AEMヘッドレス使用の手引き — 第4章 — Content Servicesテンプレートの定義
description: AEM Headless Tutorialの第4章では、AEM Content ServicesでのAEM編集可能テンプレートの役割について説明します。 編集可能なテンプレートは、AEM Content Servicesが最終的に公開するJSONコンテンツ構造を定義するために使用されます。
seo-description: AEM Headless Tutorialの第4章では、AEM Content ServicesでのAEM編集可能テンプレートの役割について説明します。 編集可能なテンプレートは、AEM Content Servicesが最終的に公開するJSONコンテンツ構造を定義するために使用されます。
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '835'
ht-degree: 1%

---


# 第4章 — Content Servicesのテンプレートの定義

AEM Headless Tutorialの第4章では、AEM Content ServicesでのAEM編集可能テンプレートの役割について説明します。 編集可能なテンプレートは、Content Servicesが有効になったAEMコンポーネントの構成を介してAEM Content Servicesがクライアントに公開するJSONコンテンツ構造を定義するために使用されます。

## AEM Content Servicesのテンプレートの役割について

AEMの編集可能なテンプレートは、イベントのコンテンツをJSONとして公開するためにアクセスされるHTTPエンドポイントを定義するために使用されます。

従来、AEMの編集可能なテンプレートはWebページの定義に使用されていましたが、単なる慣習です。 編集可能なテンプレートは、 **任意の** 1組のコンテンツの構成に使用できます。コンテンツへのアクセス方法：をブラウザー内のHTMLとして使用する場合、JavaScript(AEM SPA Editor)やモバイルアプリで使用されるJSONは、そのページのリクエスト方法を示す関数です。

AEM Content Servicesでは、編集可能なテンプレートを使用して、JSONデータの公開方法が定義されます。

アプリの場合は、単一のAPIエンドポイントを駆動するために使用する、単一の編集可能なテンプレートを作成します。 [!DNL WKND Mobile] この例ではAEMヘッドレスの概念を簡単に説明できますが、異なるコンテンツのセットを公開する複数のページ（またはエンドポイント）を作成して、より複雑で組織化されたAPIを作成できます。

## APIエンドポイントについて

APIエンドポイントの作成方法、およびアプリに公開されるコンテンツを理解するには、デザインを再度訪問しま [!DNL WKND Mobile] す。

![イベントAPIページ分解](./assets/chapter-4/design-to-component-mapping.png)

見てのとおり、モバイルアプリに提供するコンテンツは3つの論理的なセットです。

1. The **Logo**
2. タグ **行**
3. **イベントのリスト**

これを行うには、必要なコンテンツをJSONとして公開するために、これらの要件をAEMコンポーネント(およびAEM WCMコアコンポーネント)にマッピングします。

1. ロ **ゴは** 、 **画像コンポーネントを通して表示されます。**
2. タ **グ行** は、 **テキストコンポーネントを介して表示されます**
3. **イベントのリストは** 、 **コンテンツフラグメントリストコンポーネントを介して表示されます。このコンポーネントは** 、一連のイベントコンテンツフラグメントを参照します。

>[!NOTE]
>
>AEM Content ServiceでのページとコンポーネントのJSONエクスポートをサポートするには、ページとコンポーネントがAEM WCMコアコンポーネントから **派生している必要があります**。
>
>[AEM WCM Core Components](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) （ WCMコアコンポーネント）には、書き出したページとコンポーネントの正規化されたJSONスキーマをサポートする組み込み機能があります。 このチュートリアルで使用するすべてのWKND Mobileコンポーネント(ページ、画像、テキストおよびコンテンツフラグメントリスト)は、AEM WCMコアコンポーネントから派生しています。

## イベントAPIテンプレートの定義

1. **[!UICONTROL ツール]/[!UICONTROL 一般]/[!UICONTROL テンプレート]/に移動します[!DNL WKND Mobile]**。

1. Create the **[!DNL Events API]** template:

   1. 上部のアクションバーで **[!UICONTROL 「]** 作成」をタップします
   1. Select the **[!DNL WKND Mobile - Empty Page]** template
   1. 上部のアクションバーで **[!UICONTROL 「次へ]** 」をタップします
   1. 「テン **[!DNL Events API]** プレートタイトル  」フィールドにと入力します。
   1. 上部のアクションバーで **[!UICONTROL 「]** 作成」をタップします
   1. 「 **[!UICONTROL 開く]** 」をタップして、編集用に新しいテンプレートを開きます。

1. まず、ルート [!UICONTROL レイアウトコンテナの] ポリシーを編集して、コンテンツをモデル化する必要がある3つのAEMコンポーネントを許可し ます。 「 **[!UICONTROL 構造]** 」モードがアクティブになっていることを確認し、を選択 **[!DNL Layout Container \[Root\]]**&#x200B;して「 **[!UICONTROL ポリシー]** 」ボタンをタップします。
1. 「 **[!UICONTROL プロパティ]/[!UICONTROL 許可されているコンポーネント]** 」で、を検索しま **[!DNL WKND Mobile]**&#x200B;す。 コンポーネントグループから以下のコンポ [!DNL WKND Mobile] ーネントを許可し、 [!DNL Events] APIページで使用できるようにします。

   * **[!DNL WKND Mobile > Image]**

      * アプリのロゴ
   * **[!DNL WKND Mobile > Text]**

      * アプリの紹介テキスト
   * **[!DNL WKND Mobile > Content Fragment List]**

      * アプリで表示できるイベントカテゴリのリスト



1. 完了したら、右上隅の **[!UICONTROL 「完了]** 」チェックマークをタップします。
1. **ブラウザーウィンドウを更新し** 、左側のレールに新しく [!UICONTROL 許可されたコンポーネント] リストを表示します。
1. 左側のレールのコンポーネントファインダーで、次のAEMコンポーネントをドラッグします。
   1. **[!DNL Image]** ロゴ
   2. **[!DNL Text]** タグ行
   3. **[!DNL Content Fragment List]** イベント
1. **上記の各コンポーネントに対して**、それらを選択し、「 **ロック解除** 」ボタンを押します。
1. ただし、他のコンポーネントが追加されないように **レイアウトコンテナ** が **ロックされているか** 、これらの3つのコンポーネントが削除されないようにしてください。
1. 管理者の **[!UICONTROL ページ情報]/[!UICONTROL 表示をタップして]** 、 [!DNL WKND Mobile] テンプレートのリストに戻ります。 新しく作成した **[!DNL Events API]** テンプレートを選択し、上部のアクションバーで「 **[!UICONTROL 有効]** 」をタップします。

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> コンテンツの表示に使用されるコンポーネントは、テンプレート自体に追加され、ロックダウンされます。 これは、事前定義されたコンポーネントの編集を作成者に許可するためですが、API自体を変更するとJSON構造の前提が崩れ、消費するアプリが壊れる可能性があるので、コンポーネントの追加や削除を任意に行うことはできません。 すべてのAPIは安定している必要があります。

## 次の手順

必要に応じて、AEM Package Managerを使用してAEM Authorに [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) contentパッケージをインストールします [](http://localhost:4502/crx/packmgr/index.jsp)。 このパッケージには、チュートリアルのこの章と前の章で概要を説明している設定と内容が含まれています。

* [第5章 — Content Servicesページのオーサリング](./chapter-5.md)
