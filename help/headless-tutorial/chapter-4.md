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

従来、AEMの編集可能なテンプレートはWebページの定義に使用されていましたが、単なる慣習です。 編集可能なテンプレートは、**任意の**&#x200B;セットのコンテンツを構成するのに使用できます。コンテンツへのアクセス方法：をブラウザーでHTMLとして使用する場合、JavaScript(AEM SPA Editor)やモバイルアプリで使用されるJSONは、そのページのリクエスト方法を示す関数です。

AEM Content Servicesでは、編集可能なテンプレートを使用して、JSONデータの公開方法が定義されます。

[!DNL WKND Mobile]アプリの場合、単一のAPIエンドポイントを駆動するために使用する、単一の編集可能なテンプレートを作成します。 この例ではAEMヘッドレスの概念を簡単に説明できますが、異なるコンテンツのセットを公開する複数のページ（またはエンドポイント）を作成して、より複雑で組織化されたAPIを作成できます。

## APIエンドポイントについて

APIエンドポイントの作成方法と[!DNL WKND Mobile]アプリに公開するコンテンツを理解するには、デザインを再度訪問します。

![イベントAPIページ分解](./assets/chapter-4/design-to-component-mapping.png)

見てのとおり、モバイルアプリに提供するコンテンツは3つの論理的なセットです。

1. **ロゴ**
2. **タグ行**
3. **イベント**&#x200B;のリスト

これを行うには、必要なコンテンツをJSONとして公開するために、これらの要件をAEMコンポーネント(およびAEM WCMコアコンポーネント)にマッピングします。

1. **ロゴ**&#x200B;は、**画像コンポーネント**&#x200B;を介して表示されます
2. **タグ行**&#x200B;は、**テキストコンポーネント**&#x200B;を介して表示されます
3. **イベント**&#x200B;のリストは、**コンテンツフラグメントリストコンポーネント**&#x200B;を介して参照され、次に、一連のイベントコンテンツフラグメントを参照します。

>[!NOTE]
>
>AEM Content ServiceのページとコンポーネントのJSONエクスポートをサポートするには、ページとコンポーネントはAEM WCMコアコンポーネント&#x200B;**から派生する**&#x200B;必要があります。
>
>[AEM WCM Core ](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) Componentsには組み込みの機能があり、書き出したページとコンポーネントの正規化されたJSONスキーマをサポートします。このチュートリアルで使用するすべてのWKND Mobileコンポーネント(ページ、画像、テキストおよびコンテンツフラグメントリスト)は、AEM WCMコアコンポーネントから派生しています。

## イベントAPIテンプレートの定義

1. **[!UICONTROL ツール] > [!UICONTROL 一般] > [!UICONTROL テンプレート] >[!DNL WKND Mobile]**&#x200B;に移動します。

1. **[!DNL Events API]**&#x200B;テンプレートを作成します。

   1. 上部のアクションバーにある「**[!UICONTROL 作成]**」をタップします
   1. **[!DNL WKND Mobile - Empty Page]**&#x200B;テンプレートを選択
   1. 上部のアクションバーで「**[!UICONTROL 次へ]**」をタップします
   1. 「[!UICONTROL テンプレートタイトル]」フィールドに&#x200B;**[!DNL Events API]**&#x200B;と入力します。
   1. 上部のアクションバーにある「**[!UICONTROL 作成]**」をタップします
   1. 「**[!UICONTROL 開く]**」をタップして、新しいテンプレートを開いて編集します

1. まず、ルート[!UICONTROL レイアウトコンテナ]の[!UICONTROL ポリシー]を編集して、コンテンツのモデル化に必要な3つのAEMコンポーネントを許可します。 **[!UICONTROL 構造]**&#x200B;モードがアクティブであることを確認し、**[!DNL Layout Container \[Root\]]**&#x200B;を選択して、「**[!UICONTROL ポリシー]**」ボタンをタップします。
1. **[!UICONTROL プロパティ] > [!UICONTROL 許可されているコンポーネント]**&#x200B;で、**[!DNL WKND Mobile]**&#x200B;を検索します。 [!DNL WKND Mobile]コンポーネントグループから次のコンポーネントを許可し、[!DNL Events] APIページで使用できるようにします。

   * **[!DNL WKND Mobile > Image]**

      * アプリのロゴ
   * **[!DNL WKND Mobile > Text]**

      * アプリの紹介テキスト
   * **[!DNL WKND Mobile > Content Fragment List]**

      * アプリで表示できるイベントカテゴリのリスト



1. 完了したら、右上隅の「完了」**[!UICONTROL チェックマークをタップします。]**
1. **ブラウザーウィンドウを** 更新して、左側のレールに新しく [!UICONTROL 許可されたコンポー] ネントリストを表示します。
1. 左側のレールのコンポーネントファインダーで、次のAEMコンポーネントをドラッグします。
   1. **[!DNL Image]** ロゴ
   2. **[!DNL Text]** タグ行
   3. **[!DNL Content Fragment List]** イベント
1. **上記の各コンポーネントに対して**、それらを選択し、「 **** unlock」ボタンを押します。
1. ただし、他のコンポーネントが追加されないように、**レイアウトコンテナ**&#x200B;が&#x200B;**locked**&#x200B;であることを確認するか、これらの3つのコンポーネントが削除されないようにしてください。
1. **[!UICONTROL ページ表示]をタップして、管理者]**&#x200B;の[!UICONTROL 情報をタップし、[!DNL WKND Mobile]テンプレートの一覧に戻ります。 新しく作成した&#x200B;**[!DNL Events API]**&#x200B;テンプレートを選択し、上部のアクションバーで「**[!UICONTROL 有効にする]**」をタップします。

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> コンテンツの表示に使用されるコンポーネントは、テンプレート自体に追加され、ロックダウンされます。 これは、事前定義されたコンポーネントの編集を作成者に許可するためですが、API自体を変更するとJSON構造の前提が崩れ、消費するアプリが壊れる可能性があるので、コンポーネントの追加や削除を任意に行うことはできません。 すべてのAPIは安定している必要があります。

## 次の手順

必要に応じて、[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)を介して、&lt;a0/>com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)コンテンツパッケージをAEM Authorにインストールします。 [このパッケージには、チュートリアルのこの章と前の章で概要を説明している設定と内容が含まれています。

* [第5章 — Content Servicesページのオーサリング](./chapter-5.md)
