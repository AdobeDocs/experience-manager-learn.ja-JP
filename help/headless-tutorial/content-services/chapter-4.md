---
title: 第 4 章 — Content Services テンプレートの定義 — Content Services
description: AEMヘッドレスチュートリアルの第 4 章では、AEM Content Services のコンテキストでのAEM編集可能テンプレートの役割について説明します。 編集可能テンプレートは、最終的に公開されるAEM Content Services の JSON コンテンツ構造を定義するために使用されます。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 1%

---

# 第 4 章 — Content Services テンプレートの定義

AEMヘッドレスチュートリアルの第 4 章では、AEM Content Services のコンテキストでのAEM編集可能テンプレートの役割について説明します。 編集可能なテンプレートは、コンテンツサービスが有効なAEMコンポーネントの構成を通じて、AEM Content Services がクライアントに公開する JSON コンテンツ構造を定義するために使用されます。

## AEM Content Services でのテンプレートの役割について

AEM編集可能テンプレートは、イベントコンテンツを JSON として公開するためにアクセスされる HTTP エンドポイントを定義するために使用されます。

従来のAEM編集可能テンプレートは、Web ページの定義に使用されていましたが、この使用は単なる慣例です。 編集可能なテンプレートを使用して、 **任意** 一連のコンテンツそのコンテンツへのアクセス方法：をブラウザーのHTMLとして使用する場合、JavaScript(AEM SPA Editor) やモバイルアプリは、そのページがリクエストされる方法の関数です。

AEM Content Services では、編集可能なテンプレートを使用して、JSON データの公開方法を定義します。

の [!DNL WKND Mobile] アプリ、単一の API エンドポイントを駆動するために使用される単一の編集可能なテンプレートを作成します。 この例ではAEMヘッドレスの概念を簡単に説明しますが、異なるコンテンツのセットをそれぞれ公開する複数のページ（またはエンドポイント）を作成して、より複雑で整理された API を作成できます。

## API エンドポイントについて

アドビの API エンドポイントの作成方法と、アドビの API エンドポイントに公開するコンテンツを理解する [!DNL WKND Mobile] アプリ、デザインを再度見てみましょう。

![イベント API ページの分解](./assets/chapter-4/design-to-component-mapping.png)

このように、モバイルアプリに提供するコンテンツの論理セットは 3 つあります。

1. この **ロゴ**
2. この **タグライン**
3. リスト **イベント**

これをおこなうには、必要なコンテンツを JSON として公開するために、これらの要件をAEMコンポーネント ( この場合はAEM WCM コアコンポーネント ) にマッピングします。

1. この **ロゴ** は、 **画像コンポーネント**
2. この **タグライン** が **テキストコンポーネント**
3. リスト **イベント** が **コンテンツフラグメントリストコンポーネント** 次に、は一連のイベントコンテンツフラグメントを参照します。

>[!NOTE]
>
>AEM Content Service のページとコンポーネントの JSON 書き出しをサポートするには、ページとコンポーネントを使用する必要があります **AEM WCM コアコンポーネントから派生**.
>
>[AEM WCM コアコンポーネント](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) には、書き出されたページとコンポーネントの正規化された JSON スキーマをサポートする組み込み機能があります。 このチュートリアルで使用される WKND Mobile コンポーネント（ページ、画像、テキスト、コンテンツフラグメントリスト）はすべて、AEM WCM コアコンポーネントから派生したものです。

## イベント API テンプレートの定義

1. に移動します。 **[!UICONTROL ツール] > [!UICONTROL 一般] > [!UICONTROL テンプレート] >[!DNL WKND Mobile]**.

1. を作成します。 **[!DNL Events API]** テンプレート：

   1. タップ **[!UICONTROL 作成]** 上部のアクションバー
   1. を選択します。 **[!DNL WKND Mobile - Empty Page]** テンプレート
   1. タップ **[!UICONTROL 次へ]** 上部のアクションバー
   1. 入力 **[!DNL Events API]** 内 [!UICONTROL テンプレートタイトル] フィールド
   1. タップ **[!UICONTROL 作成]** 上部のアクションバー
   1. タップ **[!UICONTROL 開く]** 編集用に新しいテンプレートを開く

1. まず、識別された 3 つのAEMコンポーネントを許可し、 [!UICONTROL ポリシー] ルートの [!UICONTROL レイアウトコンテナ]. 次を確認します。 **[!UICONTROL 構造]** モードがアクティブな場合は、 **[!DNL Layout Container \[Root\]]**&#x200B;をクリックし、 **[!UICONTROL ポリシー]** 」ボタンをクリックします。
1. の下 **[!UICONTROL プロパティ] > [!UICONTROL 許可されたコンポーネント]** 検索 **[!DNL WKND Mobile]**. 次のコンポーネントを [!DNL WKND Mobile] コンポーネントグループを作成し、 [!DNL Events] API ページを参照してください。

   * **[!DNL WKND Mobile > Image]**

      * アプリのロゴ
   * **[!DNL WKND Mobile > Text]**

      * アプリの紹介テキスト
   * **[!DNL WKND Mobile > Content Fragment List]**

      * アプリで表示できるイベントカテゴリのリスト



1. 次をタップします。 **[!UICONTROL 完了]** 完了したら、右上隅にチェックマークを付けます。
1. **更新** 新しく [!UICONTROL 許可されたコンポーネント] リストを左側のパネルに表示します。
1. 左側のパネルのコンポーネントファインダーから、次のAEMコンポーネントにドラッグします。
   1. **[!DNL Image]** ロゴ
   2. **[!DNL Text]** （タグ行）
   3. **[!DNL Content Fragment List]** イベントの
1. **上記の各コンポーネント**&#x200B;をクリックし、 **ロック解除** 」ボタンをクリックします。
1. ただし、 **レイアウトコンテナ** が **ロック済み** 他のコンポーネントが追加されないようにするか、これらの 3 つのコンポーネントが削除されないようにする。
1. タップ **[!UICONTROL ページ情報] > [!UICONTROL 管理で表示]** に戻る [!DNL WKND Mobile] テンプレートのリスト。 新しく作成した **[!DNL Events API]** テンプレートをタップします。 **[!UICONTROL 有効にする]** 」をクリックします。

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> コンテンツの表示に使用されるコンポーネントがテンプレート自体に追加され、ロックダウンされていることに注意してください。 これは、API 自体を変更すると JSON 構造の前提が崩れ、使用中のアプリが壊れる可能性があるので、作成者が事前定義済みのコンポーネントを編集できる一方で、コンポーネントの追加や削除は恣意的にはおこなえないようにするためです。 すべての API は安定している必要があります。

## 次の手順

オプションで、 [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) を介して AEM オーサー上のコンテンツパッケージ [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). このパッケージには、チュートリアルのこの章および前の章で概要を説明する設定とコンテンツが含まれています。

* [第 5 章 — コンテンツサービスページのオーサリング](./chapter-5.md)
