---
title: AEM Experience Fragments とAdobe Targetを使用したパーソナライゼーション
seo-title: Personalization using Adobe Experience Manager (AEM) Experience Fragments and Adobe Target
description: Adobe Experience Manager Experience Fragments とAdobe Targetを使用してパーソナライズされたエクスペリエンスを作成し、配信する方法を示す、エンドツーエンドのチュートリアルです。
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Experience Manager Experience Fragments and Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 47446e2a-73d1-44ba-b233-fa1b7f16bc76
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1691'
ht-degree: 1%

---

# AEM Experience Fragments とAdobe Targetを使用したパーソナライゼーション

AEMエクスペリエンスフラグメントをHTMLオファーとしてAdobe Targetに書き出す機能を使用すると、AEMの使いやすさと機能を、Target の強力な Automated Intelligence(AI) 機能と機械学習 (ML) 機能と組み合わせて、エクスペリエンスを大規模にテストおよびパーソナライズできます。

AEMでは、パーソナライゼーション戦略に生かすために、すべてのコンテンツとアセットが一元的に統合されます。 AEMを使用すると、コードを記述することなく、1 か所でデスクトップ、タブレット、モバイルデバイス用のコンテンツを簡単に作成できます。 デバイスごとにページを作成する必要はありません。AEMは、コンテンツを使用して各エクスペリエンスを自動的に調整します。

Target では、行動、コンテキスト、オフラインの変数を組み込んだルールベースの手法と AI 駆動の機械学習手法を組み合わせ、それらを基にパーソナライズされたエクスペリエンスを大規模に提供できます。  Target では、A/B テストや多変量分析 (MVT) アクティビティを簡単に設定して実行し、最適なオファー、コンテンツ、エクスペリエンスを見極めることができます。

エクスペリエンスフラグメントは、Target を使用してビジネス成果を高めているマーケターとコンテンツ作成者を連携させる大きな一歩を踏み出します。

## シナリオの概要

WKND サイトは、 **SkateFest チャレンジ** ウェブサイトを通じてアメリカ全土で、各州で行われるオーディションに、サイトユーザーを新規登録してもらいたいと考えています。 マーケターには、WKND サイトのホームページでキャンペーンを実行するタスクが割り当てられ、ユーザーの場所に関連するバナーメッセージと、イベントの詳細ページへのリンクが割り当てられています。 WKND サイトのホームページを参照し、現在の場所に基づいてユーザーのパーソナライズされたエクスペリエンスを作成し、配信する方法を学びます。

### 関連するユーザー

この練習では、次のユーザーが関与し、管理者アクセスが必要なタスクを実行する必要があります。

* **コンテンツ作成者/コンテンツエディター** (Adobe Experience Manager)
* **マーケター** (Adobe Target/最適化チーム )

### 前提条件

* **AEM**
   * [AEMオーサーインスタンスとパブリッシュインスタンス](./implementation.md#getting-aem) localhost 4502 と 4503 でそれぞれ実行している
* **Experience Cloud**
   * 組織へのアクセスAdobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 次のソリューションでプロビジョニングされたExperience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

### WKND サイトホームページ

![AEM Target シナリオ 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. マーケターがAEM Content Editor で WKND SkateFest キャンペーンのディスカッションを開始し、要件の詳細を説明します。
   * ***要件***:米国の各州からの訪問者向けにパーソナライズされたコンテンツを使用して、WKND サイトホームページで WKND SkateFest キャンペーンを宣伝します。 背景画像、テキストおよびボタンを含む新しいコンテンツブロックをホームページカルーセルの下に追加します。
      * **背景画像**:画像は、ユーザーが WKND サイトページの訪問元の状態に関連している必要があります。
      * **テキスト**:&quot;新規登録してAudition&quot;
      * **ボタン**:WKND SkateFest ページを指す「イベントの詳細」
      * **WKND SkateFest ページ**:オーディションの会場、日時を含むイベントの詳細を含む新しいページ。
1. 要件に基づいて、AEMコンテンツエディターはコンテンツブロックのエクスペリエンスフラグメントを作成し、Adobe Targetにオファーとして書き出します。 米国のすべての状態用にパーソナライズされたコンテンツを提供するには、コンテンツ作成者がエクスペリエンスフラグメントのマスターバリエーションを 1 つ作成し、その後、状態ごとに 1 つずつ、他の 50 個のバリエーションを作成します。 状態のバリエーションごとに、関連する画像とテキストを含むコンテンツを手動で編集できます。 エクスペリエンスフラグメントをオーサリングする際、コンテンツエディターは、アセットファインダーオプションを使用して、AEM Assets内で使用可能なすべてのアセットにすばやくアクセスできます。 エクスペリエンスフラグメントがAdobe Targetに書き出されると、そのすべてのバリエーションもオファーとしてAdobe Targetに書き出されます。

1. エクスペリエンスフラグメントをAEMからAdobe Targetにオファーとして書き出した後、マーケターは、これらのオファーを使用して Target でアクティビティを作成できます。 WKND サイト SkateFest キャンペーンに基づき、マーケターは、各州から WKND サイト訪問者に対して、パーソナライズされたエクスペリエンスを作成して配信する必要があります。 エクスペリエンスのターゲット設定アクティビティを作成するには、マーケターがオーディエンスを特定する必要があります。 WKND SkateFest キャンペーンの場合、WKND Web サイトの訪問者の所在地に基づいて、50 人の個別のオーディエンスを作成する必要があります。
   * [オーディエンス](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) アクティビティのターゲットを定義し、ターゲティングを使用できる場所ならどこでも使用します。 ターゲットオーディエンスは、定義された訪問者条件のセットです。 オファーは、特定のオーディエンス（またはセグメント）をターゲットに設定できます。 そのオーディエンスに属する訪問者のみが、そのユーザーをターゲットとするエクスペリエンスを表示します。  例えば、特定のブラウザーを使用する訪問者や特定の地域からの訪問者で構成されるオーディエンスに対して、オファーを配信できます。
   * An [オファー](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9) は、キャンペーンまたはアクティビティの間に Web ページに表示されるコンテンツです。 Web ページをテストする場合、場所の様々なオファーを使用して、各エクスペリエンスの成功を測定します。 オファーには、次のような様々なタイプのコンテンツを含めることができます。
      * 画像
      * テキスト
      * **HTML**
         * *HTMLオファーは、このシナリオのアクティビティに使用されます*
      * リンク
      * ボタン

## コンテンツエディターアクティビティ

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>エクスペリエンスフラグメントをAdobe Targetに書き出す前に公開します。

## マーケターアクティビティ

### GeoTargeting を使用するオーディエンスの作成 {#marketer-audience}

1. 組織に移動 [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`<https://<yourcompany>.experiencecloud.adobe.com`)
1. Adobe IDを使用してログインし、正しい組織に属していることを確認します。
1. ソリューション切り替えボタンで、 **ターゲット** その後 **起動する** Adobe Target。

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. 次に移動： **オファー** 「 」タブに移動し、「WKND」オファーを検索します。 エクスペリエンスフラグメントのバリエーションのリストが、AEMからHTMLオファーとして書き出されます。 各オファーは、1 つの状態に対応します。 例： *WKND SkateFest California* は、カリフォルニア州から WKND サイトの訪問者に提供されるオファーです。

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. メインナビゲーションで、 **オーディエンス**.

   マーケターは、米国の各州から来た WKND サイト訪問者に対して、50 人の個別オーディエンスを作成する必要があります。

1. オーディエンスを作成するには、 **オーディエンスを作成** ボタンをクリックし、オーディエンスの名前を指定します。

   **オーディエンス名の形式：WKND-\&lt;*state*\>**

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. クリック **ルールを追加/地域**.
1. クリック **選択**&#x200B;次のいずれかのオプションを選択します。
   * 国
   * **都道府県** *（WKND サイト SkateFest キャンペーンの状態を選択）*
   * City（市区町村）
   * 郵便番号
   * 緯度
   * 経度
   * DMA
   * 携帯電話会社

   **地域**  — オーディエンスを使用して、国、都道府県、市区町村、郵便番号、DMA、携帯電話会社など、地理的な場所に基づいてユーザーをターゲットに設定します。 位置情報パラメーターを使用すると、訪問者の地域情報に基づいてアクティビティとエクスペリエンスをターゲット設定できます。 このデータは、訪問者の IP アドレスに基づいて、Target リクエストごとに送信されます。 他のターゲット値と同様に、これらのパラメーターを選択します。

   >[!NOTE]
   >訪問者の IP アドレスが mbox リクエスト ( 訪問（セッション）ごとに 1 回渡され、その訪問者の地域ターゲット設定パラメーターを解決します。

1. 演算子を次のように選択します。 **一致**、適切な値を指定します ( 例：（カリフォルニア州）および **保存** 変更内容。 ここでは、状態名を指定します。

   ![Adobe Target — 地域ルール](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >1 つのオーディエンスに複数のルールを割り当てることができます。

1. 手順 6～9 を繰り返して、他の状態のオーディエンスを作成します。

   ![Adobe Target - WKND オーディエンス](assets/personalization-use-case-1/adobe-target-audiences-50.png)

この時点で、米国の様々な州のすべての WKND サイト訪問者のオーディエンスを正常に作成し、各州の対応するHTMLオファーも用意しています。 次に、 WKND サイトのホームページに対応するオファーを使用してオーディエンスをターゲットにするエクスペリエンスのターゲット設定アクティビティを作成します。

### GeoTargeting を使用するアクティビティの作成

1. Adobe Targetウィンドウで、に移動します。 **アクティビティ** タブをクリックします。
1. クリック **アクティビティを作成** をクリックし、 **エクスペリエンスのターゲット設定** アクティビティタイプ。
1. を選択します。 **Web** チャネルと選択 **Visual Experience Composer**.
1. 次を入力します。 **アクティビティ URL** をクリックし、 **次へ** をクリックして Visual Experience Composer を開きます。

   WKND サイトホームページの公開 URL:http://localhost:4503/content/wknd/en.html

   ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/target-activity.png)

1. の場合 **Visual Experience Composer** 読み込むには、有効にするには **安全でないスクリプトの読み込みを許可** ブラウザーで、ページをリロードします。

   ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. WKND サイトのホームページが Visual Experience Composer エディターで開いていることに注意してください。

   ![VEC](assets/personalization-use-case-1/vec.png)

1. VEC にオーディエンスを追加するには、 **エクスペリエンスのターゲット設定を追加** 「オーディエンス」で、 WKND-California オーディエンスを選択し、「 **次へ**.

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. VEC 内の WKND サイトページをクリックし、WKND-California オーディエンス用のオファーを追加するHTML要素を選択して、 **置換文字列** オプションを選択し、 **HTMLオファー**.

   ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/vec-selecting-div.png)

1. を選択します。 **WKND SkateFest California** HTMLオファー **WKND-California** オファーからオーディエンスを選択し、 UI を選択して、 **完了**.
1. これで、 **WKND SkateFest California** HTMLオファーが WKND-California オーディエンス用に WKND サイトページに追加されました。
1. 手順 7～10 を繰り返して、他の状態のエクスペリエンスのターゲット設定を追加し、対応するHTMLオファーを選択します。
1. クリック **次へ** 続行すると、Audiences からエクスペリエンスへのマッピングを表示できます。
1. クリック **次へ** をクリックして、目標と設定に移動します。
1. レポートソースを選択し、アクティビティの主な目標を特定します。 このシナリオでは、レポートソースをとして選択します。 **Adobe Target**、アクティビティを **コンバージョン**、ページに表示されたアクション、WKND SkateFest の詳細ページを指す URL。

   ![目標とターゲティング — ターゲット](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >また、レポートソースとしてAdobe Analyticsを選択することもできます。

1. 現在のアクティビティ名の上にマウスポインターを置くと、名前をに変更できます。 **WKND SkateFest — 米国**、 **保存して閉じる** 変更内容。
1. アクティビティの詳細画面で、 **有効化** アクティビティを選択します。

   ![アクティビティを有効化](assets/personalization-use-case-1/activate-activity.png)

1. これで、WKND SkateFest キャンペーンがすべての WKND サイト訪問者に対して有効になります。
1. 次に移動： [WKND サイトホームページ](http://localhost:4503/content/wknd/en.html)に設定され、その地域 (*状態：カリフォルニア*) をクリックします。

   ![アクティビティ QA](assets/personalization-use-case-1/wknd-california.png)

### Target アクティビティ QA

1. の下 **アクティビティの詳細/概要** タブで、 **アクティビティ QA** 」ボタンをクリックし、すべてのエクスペリエンスへの直接 QA リンクを取得できます。

   ![アクティビティ QA](assets/personalization-use-case-1/activity-qa.png)

1. 次に移動： [WKND サイトホームページ](http://localhost:4503/content/wknd/en.html)を表示し、位置情報（状態）に基づいて WKND SkateFest オファーを表示できるようになります。
1. 以下のビデオを見て、オファーがページにどのように配信されるか、レスポンストークンをカスタマイズする方法、品質チェックの実行方法を理解してください。

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## 概要

この章では、コンテンツ編集者が、Adobe Experience Manager内で WKND SkateFest キャンペーンをサポートするすべてのコンテンツを作成し、ユーザーの地域に基づいてエクスペリエンスのターゲット設定を作成するためのHTMLオファーとしてAdobe Targetに書き出すことができました。
