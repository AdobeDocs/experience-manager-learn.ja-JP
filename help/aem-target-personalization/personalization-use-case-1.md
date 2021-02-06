---
title: AEMエクスペリエンスフラグメントとAdobe Targetを使用したパーソナライゼーション
seo-title: Adobe Experience Manager(AEM)エクスペリエンスフラグメントとAdobe Targetを使用したパーソナライズ
description: Adobe Experience ManagerのエクスペリエンスフラグメントとAdobe Targetを使用して、パーソナライズされたエクスペリエンスを作成し、配信する方法を示すエンドツーエンドのチュートリアルです。
seo-description: Adobe Experience ManagerのエクスペリエンスフラグメントとAdobe Targetを使用して、パーソナライズされたエクスペリエンスを作成し、配信する方法を示すエンドツーエンドのチュートリアルです。
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '1729'
ht-degree: 1%

---


# AEMエクスペリエンスフラグメントとAdobe Targetを使用したパーソナライゼーション

AEMエクスペリエンスフラグメントをHTMLオファーとしてAdobe Targetに書き出す機能により、AEMの使いやすさと能力を、強力なAutomated Intelligence(AI)およびMachine Learning(ML)機能と組み合わせて、エクスペリエンスを大規模にテストおよびパーソナライズできます。

AEMは、すべてのコンテンツとアセットを一元的に集約し、パーソナライズ戦略を強化します。 AEMを使用すると、コードを記述することなく、デスクトップ、タブレットおよびモバイルデバイス用のコンテンツを1か所で簡単に作成できます。 すべてのデバイスにページを作成する必要はありません。AEMでは、コンテンツを使用して各エクスペリエンスを自動的に調整します。

ターゲットを使用すると、行動、コンテキストおよびオフラインの変数を組み込んだ、ルールベースの学習アプローチとAI主導の機械学習アプローチを組み合わせて、スケールを合わせたパーソナライズされたエクスペリエンスを提供できます。  ターゲットを使用すると、A/Bおよび多変量分析(MVT)アクティビティを簡単に設定および実行して、最適なオファー、コンテンツおよびエクスペリエンスを決定できます。

エクスペリエンスフラグメントは、ターゲットを使用してビジネスの成果を引き起こしているマーケティング担当者とコンテンツ制作者を結び付けるための大きな一歩となります。

## シナリオの概要

WKNDサイトは、自社のウェブサイトを通じて、アメリカ全土に&#x200B;**SkateFestチャレンジ**&#x200B;を発表し、各州で行われるオーディションにサイトユーザを新規登録させる予定です。 マーケティング担当者には、WKNDサイトホームページでキャンペーンを実行するタスクが割り当てられており、そのユーザーの場所に関連するバナーメッセージと、イベントの詳細ページへのリンクが含まれています。 WKNDサイトのホームページを調査し、現在の場所に基づいてパーソナライズされたエクスペリエンスを作成し、配信する方法を学びます。

### 関係するユーザー

この練習では、次のユーザが関与し、管理者アクセスが必要となるタスクを実行する必要があります。

* **コンテンツプロデューサー/コンテンツエディタ** (Adobe Experience Manager)
* **マーケティング担当者** (Adobe Target/最適化チーム)

### 前提条件

* **AEM**
   * [AEM authorおよびpublish](./implementation.md#getting-aem) インスタンスは、それぞれlocalhost 4502および4503で実行されます。
* **Experience Cloud**
   * 組織へのアクセス：Adobe Experience Cloud- <https://>`<yourcompany>`.experiencecloud.adobe.com
   * 次のソリューションでプロビジョニングされたExperience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

### WKNDサイトホームページ

![AEMターゲットシナリオ1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. マーケティング担当者が、AEMコンテンツエディターとWKND SkateFestキャンペーンの話し合いを開始し、要件の詳細を説明します。
   * ***要件***:米国の各州の訪問者向けにパーソナライズされたコンテンツを使用して、WKNDサイトホームページのWKND SkateFestキャンペーンを推進します。ホームページカルーセルの下に追加、背景画像、テキスト、ボタンを含む新しいコンテンツブロックが追加されました。
      * **背景画像**:画像は、ユーザがWKNDサイトページを訪問した元の状態に関連する必要があります。
      * **テキスト**:「Auditionへのサインアップ」
      * **ボタン**:「イベントの詳細」WKND SkateFestページ
      * **WKND SkateFestページ**:オーディションの開催場所、日時など、イベントの詳細を含む新しいページ。
1. 要件に基づいて、AEM Content Editorはコンテンツブロックのエクスペリエンスフラグメントを作成し、オファーとしてAdobe Targetに書き出します。 コンテンツの作成者は、米国のすべての州向けにパーソナライズされたコンテンツを提供するために、1つのエクスペリエンスフラグメントマスターバリエーションを作成し、その他50種類のバリエーションを州ごとに1つずつ作成できます。 各状態のバリエーションのコンテンツと関連する画像およびテキストを手動で編集できます。 エクスペリエンスフラグメントをオーサリングする場合、コンテンツエディターは、アセットファインダーオプションを使用して、AEM Assets内で使用可能なすべてのアセットにすばやくアクセスできます。 エクスペリエンスフラグメントがAdobe Targetに書き出されると、そのすべてのバリエーションもオファーとしてAdobe Targetに押し出されます。

1. エクスペリエンスフラグメントをAEMからオファーとしてAdobe Targetに書き出した後、マーケターは、これらのオファーを使用してターゲット内にアクティビティを作成できます。 WKNDサイトのSkateFestキャンペーンに基づき、マーケティング担当者は、各州のWKNDサイト訪問者に対して、パーソナライズされたエクスペリエンスを作成し、配信する必要があります。 エクスペリエンスのターゲット設定アクティビティを作成するには、マーケターはオーディエンスを識別する必要があります。 WKND SkateFestキャンペーンの場合、WKNDのウェブサイトを訪問している訪問者の所在地に基づいて、50のオーディエンスを作成する必要があります。
   * [オーディエンスは、アクティビティのターゲットを定義し、ターゲティングが可能なあらゆる場所で使用されます。](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) ターゲットオーディエンスは、定義された訪問者条件のセットです。 オファーは、特定のオーディエンス（またはセグメント）をターゲットに設定できます。 そのオーディエンスに属する訪問者のみが、対象となるエクスペリエンスを閲覧できます。  例えば、特定のブラウザーを使用するオーディエンスや特定の地域の訪問者で構成されるオファーに対してブラウザーを提供できます。
   * [オファー](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9)は、キャンペーン中やアクティビティ中にWebページに表示されるコンテンツです。 Webページをテストする場合は、場所の異なるオファーを使用して各エクスペリエンスの成功を測定します。 オファーには、次のような様々なタイプのコンテンツを含めることができます。
      * 画像
      * テキスト
      * **HTML**
         * *HTMLオファーは、このシナリオのアクティビティに使用されます*
      * リンク
      * ボタン

## コンテンツエディタのアクティビティ

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>エクスペリエンスフラグメントを公開してから、Adobe Targetに書き出します。

## マーケティング担当者のアクティビティ

### 地域ターゲット設定を使用したオーディエンスの作成{#marketer-audience}

1. 組織[Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.experiencecloud.adobe.com)に移動します。
1. Adobe IDを使用してログインし、正しい組織に属していることを確認します。
1. ソリューションの切り替えボタンで、**ターゲット**&#x200B;をクリックし、**** Adobe Targetを起動します。

   ![Experience Cloud-Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. 「**オファー**」タブに移動し、「WKND」オファーを検索します。 AEMからHTMLオファーとして書き出された、エクスペリエンスフラグメントのバリエーションのリストを確認できるはずです。 各オファーは、状態に対応します。 例えば、*WKND SkateFest California*&#x200B;は、カリフォルニア州のWKNDサイト訪問者に提供されるオファーです。

   ![Experience Cloud-Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. メインナビゲーションで&#x200B;**オーディエンス**&#x200B;をクリックします。

   マーケティング担当者は、米国の州ごとに、WKNDのサイト訪問者用に50のオーディエンスを作成する必要があります。

1. オーディエンスを作成するには、「**オーディエンスを作成**」ボタンをクリックし、オーディエンスの名前を入力します。

   **オーディエンス名の形式：WKND-\&lt;>state *\>***

   ![Experience Cloud-Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. **ルール/追加地域**&#x200B;をクリックします。
1. 「**選択**」をクリックし、次のいずれかのオプションを選択します。
   * 国
   * **状態** *(WKNDサイトのSkateFestキャンペーンの状態を選択)*
   * 市区町村
   * 郵便番号
   * 緯度
   * 経度
   * DMA
   * 携帯電話会社

   **地域**  — 国、都道府県、市区町村、郵便番号、DMA、携帯電話会社など、地域に基づいてターゲットを行うオーディエンスを使用します。位置情報パラメーターを使用すると、訪問者の地域情報に基づいてアクティビティやエクスペリエンスをターゲットできます。 このデータは、ターゲットリクエストのたびに送信され、訪問者のIPアドレスに基づきます。 他のターゲット値と同様に、これらのパラメーターを選択します。

   >[!NOTE]
   >訪問者のIPアドレスは、mboxリクエストと共に訪問（セッション）ごとに1回渡され、その訪問者の地域ターゲティングパラメーターが解決されます。

1. 演算子を「**一致**」として選択し、適切な値(例：カリフォルニア州)と&#x200B;**変更を保存**&#x200B;します。 この場合は、状態名を入力します。

   ![Adobe Target — 地域ルール](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >1つのオーディエンスに複数のルールを割り当てることができます。

1. 手順6 ～ 9を繰り返して、他の状態のオーディエンスを作成します。

   ![Adobe TargetWKNDオーディエンス](assets/personalization-use-case-1/adobe-target-audiences-50.png)

この時点で、米国の様々な州のすべてのWKNDサイト訪問者用のオーディエンスの作成に成功しました。また、州ごとに対応するHTMLオファーも作成されています。 次に、エクスペリエンスのターゲット設定アクティビティを作成して、WKNDサイトホームページの対応するオファーとオーディエンスをターゲットします。

### 地域ターゲティングを使用したアクティビティの作成

1. Adobe Targetのウィンドウで、**アクティビティ**&#x200B;タブに移動します。
1. 「アクティビティを作成&#x200B;**」をクリックし、「**&#x200B;エクスペリエンスのターゲット設定&#x200B;**」アクティビティタイプを選択します。**
1. 「**Web**」チャネルを選択し、「**Visual Experience Composer**」を選択します。
1. **アクティビティURL**&#x200B;を入力し、「**次へ**」をクリックしてVisual Experience Composerを開きます。

   WKNDサイトホームページ公開URL:http://localhost:4503/content/wknd/en.html

   ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/target-activity.png)

1. **Visual Experience Composer**&#x200B;を読み込むには、ブラウザーで「安全でないスクリプトを読み込むことを許可&#x200B;**」を有効にし、ページを再読み込みします。**

   ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. Visual Experience ComposerエディターでWKNDサイトホームページが開きます。

   ![VEC](assets/personalization-use-case-1/vec.png)

1. VECにオーディエンスを追加するには、「オーディエンス」の下の「追加&#x200B;**エクスペリエンスのターゲット設定**」をクリックし、WKND-Californiaオーディエンスを選択して「**次へ**」をクリックします。

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. VEC内のWKNDサイトページをクリックし、HTML要素を選択してWKND-Californiaオーディエンス用のオファーを追加します。次に、「****&#x200B;で置換」を選択し、「**HTMLオファー**」を選択します。

   ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/vec-selecting-div.png)

1. オファーから&#x200B;**WKND-California**&#x200B;オーディエンスの&#x200B;**WKND SkateFest California** HTMLオファーを選択し、「UIを選択」をクリックし、「**完了**」をクリックします。
1. これで、WKND-CaliforniaオーディエンスのWKNDサイトページに追加された&#x200B;**WKND SkateFest California** HTMLオファーを確認できるはずです。
1. 手順7 ～ 10を繰り返して、他の状態のエクスペリエンスターゲット設定を追加し、対応するHTMLオファーを選択します。
1. 「**次へ**」をクリックして続行すると、オーディエンスとエクスペリエンスとのマッピングを確認できます。
1. 「**次へ**」をクリックして、「目標と設定」に移動します。
1. レポートソースを選択し、アクティビティの主な目標を特定します。 このシナリオでは、レポートソースを&#x200B;**Adobe Target**&#x200B;として選択し、アクティビティを&#x200B;**コンバージョン**&#x200B;として測定し、ページを表示したアクション、WKND SkateFest詳細ページを示すURLを選択します。

   ![目標とターゲット設定 —ターゲット](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >また、レポートソースとしてAdobe Analyticsを選択することもできます。

1. 現在のアクティビティ名の上にカーソルを置くと、**WKND SkateFest - USA**&#x200B;に名前を変更し、**変更を保存して**&#x200B;閉じることができます。
1. アクティビティの詳細画面で、**アクティビティを**&#x200B;アクティブにします。

   ![アクティビティのアクティブ化](assets/personalization-use-case-1/activate-activity.png)

1. WKND SkateFestキャンペーンは、すべてのWKNDサイト訪問者に提供されます。
1. [WKNDサイトホームページ](http://localhost:4503/content/wknd/en.html)に移動すると、地域(*状態：カリフォルニア*)。

   ![アクティビティQA](assets/personalization-use-case-1/wknd-california.png)

### ターゲットアクティビティQA

1. 「**アクティビティの詳細/概要**」タブで、「**アクティビティQA**」ボタンをクリックすると、すべてのエクスペリエンスに直接QAリンクを設定できます。

   ![アクティビティQA](assets/personalization-use-case-1/activity-qa.png)

1. [WKNDサイトホームページ](http://localhost:4503/content/wknd/en.html)に移動すると、地域（状態）に基づいたWKND SkateFestオファーを表示できます。
1. 以下のビデオを見て、オファーがページに配信される方法、応答トークンのカスタマイズ方法、品質チェックの実行方法を理解してください。

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## 概要

この章では、コンテンツエディターで、Adobe Experience Manager内でWKND SkateFestキャンペーンをサポートするすべてのコンテンツを作成し、HTMLオファーとしてAdobe Targetに書き出して、ユーザーの地域に基づくエクスペリエンスターゲット設定を作成できました。
