---
title: AEMエクスペリエンスフラグメントとAdobe Targetを使用したパーソナライゼーション
seo-title: Adobe Experience Manager(AEM)エクスペリエンスフラグメントとAdobe Targetを使用したパーソナライゼーション
description: Adobe Experience ManagerエクスペリエンスフラグメントとAdobe Targetを使用してパーソナライズされたエクスペリエンスを作成し、配信する方法を示す、エンドツーエンドのチュートリアルです。
seo-description: Adobe Experience ManagerエクスペリエンスフラグメントとAdobe Targetを使用してパーソナライズされたエクスペリエンスを作成し、配信する方法を示す、エンドツーエンドのチュートリアルです。
feature: エクスペリエンスフラグメント
topic: パーソナライズ機能
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1732'
ht-degree: 1%

---


# AEMエクスペリエンスフラグメントとAdobe Targetを使用したパーソナライゼーション

AEMエクスペリエンスフラグメントをHTMLオファーとしてAdobe Targetに書き出す機能を使用すると、AEMの使いやすさと機能を、Targetの強力な自動インテリジェンス(AI)および機械学習(ML)機能と組み合わせて、エクスペリエンスを大規模にテストおよびパーソナライズできます。

AEMは、パーソナライゼーション戦略を強化するために、すべてのコンテンツとアセットを一元的に統合します。 AEMを使用すると、コードを記述することなく、デスクトップ、タブレットおよびモバイルデバイス用のコンテンツを1か所で簡単に作成できます。 各デバイスにページを作成する必要はありません。AEMは、コンテンツを使用して各エクスペリエンスを自動的に調整します。

Targetを使用すると、行動、コンテキスト、オフラインの変数を組み込んだ、ルールベースのアプローチとAI駆動の機械学習アプローチを組み合わせ、大規模にパーソナライズされたエクスペリエンスを提供できます。  Targetを使用すると、A/Bおよび多変量分析(MVT)アクティビティを簡単に設定して実行し、最適なオファー、コンテンツ、エクスペリエンスを決定できます。

エクスペリエンスフラグメントは、Targetを使用してビジネス成果を高めているマーケターとコンテンツ作成者を連携させる大きな一歩を踏み出します。

## シナリオの概要

WKNDサイトは、自社のウェブサイトを通じてアメリカ全土で&#x200B;**SkateFest Challenge**&#x200B;を発表し、各州で行われるオーディションにサイトユーザーにサインアップしてもらいたいと考えています。 マーケターには、WKNDサイトのホームページでキャンペーンを実行するタスクが割り当てられ、ユーザーの場所に関連するバナーメッセージと、イベントの詳細ページへのリンクが表示されます。 WKNDサイトのホームページを参照し、現在の場所に基づいてユーザーのパーソナライズされたエクスペリエンスを作成し、提供する方法を学びます。

### 関係するユーザー

この演習では、次のユーザーが関与し、管理アクセスが必要になる可能性のあるタスクを実行する必要があります。

* **コンテンツ作成者/コンテンツエディター** (Adobe Experience Manager)
* **マーケター** (Adobe Target/最適化チーム)

### 前提条件

* **AEM**
   * [AEMオーサーインスタンスとパブリッシュイン](./implementation.md#getting-aem) スタンスは、それぞれlocalhost 4502と4503で実行されます。
* **Experience Cloud**
   * 組織へのアクセスAdobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * 次のソリューションでプロビジョニングされたExperience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

### WKNDサイトのホームページ

![AEM Targetシナリオ1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. マーケターがAEM Content EditorでWKND SkateFestのキャンペーンディスカッションを開始し、要件の詳細を説明します。
   * ***要件***:WKNDサイトのホームページでWKND SkateFestキャンペーンを推進し、米国の各州の訪問者に合わせてパーソナライズされたコンテンツを提供します。背景画像、テキストおよびボタンを含む新しいコンテンツブロックを、ホームページカルーセルの下に追加します。
      * **背景画像**:画像は、WKNDサイトページのユーザーの訪問元の状態に関連している必要があります。
      * **テキスト**:「Auditionへの新規登録」
      * **ボタン**:WKND SkateFestページを指す「イベントの詳細」
      * **WKND SkateFestページ**:オーディション会場、日時など、イベントの詳細を含む新しいページ。
1. 要件に基づいて、AEMコンテンツエディターはコンテンツブロックのエクスペリエンスフラグメントを作成し、Adobe Targetにオファーとして書き出します。 米国のすべての状態に対してパーソナライズされたコンテンツを提供するには、コンテンツ作成者がエクスペリエンスフラグメントマスターバリエーションを1つ作成し、その後、状態ごとに1つずつ他の50個のバリエーションを作成します。 状態のバリエーションごとに、関連する画像とテキストを含むコンテンツを手動で編集できます。 エクスペリエンスフラグメントをオーサリングする際、コンテンツエディターは、アセットファインダーオプションを使用して、AEM Assets内で使用可能なすべてのアセットにすばやくアクセスできます。 エクスペリエンスフラグメントがAdobe Targetに書き出されると、そのすべてのバリエーションもオファーとしてAdobe Targetにプッシュされます。

1. エクスペリエンスフラグメントをAEMからAdobe Targetにオファーとして書き出した後、マーケターは、これらのオファーを使用してTargetでアクティビティを作成できます。 WKNDサイトのSkateFestキャンペーンに基づいて、マーケターは、各州からWKNDサイト訪問者に対して、パーソナライズされたエクスペリエンスを作成し、提供する必要があります。 エクスペリエンスのターゲット設定アクティビティを作成するには、マーケターがオーディエンスを特定する必要があります。 WKND SkateFestキャンペーンの場合、WKND Webサイトの訪問者の場所に基づいて、50人の個別のオーディエンスを作成する必要があります。
   * [](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) オーディエンスは、アクティビティのターゲットを定義し、ターゲティングを使用できる任意の場所で使用します。ターゲットオーディエンスは、定義された訪問者条件のセットです。 オファーは、特定のオーディエンス（セグメント）をターゲットに設定できます。 そのオーディエンスに属する訪問者のみが、そのオーディエンスのターゲットとなるエクスペリエンスを表示します。  例えば、特定のブラウザーを使用する訪問者や特定の地域からの訪問者で構成されるオーディエンスにオファーを配信できます。
   * [オファー](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9)は、キャンペーンまたはアクティビティの間にWebページに表示されるコンテンツです。 Webページをテストする場合は、場所の様々なオファーを使用して各エクスペリエンスの成功を測定します。 オファーには、次のような様々なタイプのコンテンツを含めることができます。
      * 画像
      * テキスト
      * **HTML**
         * *HTMLオファーは、このシナリオのアクティビティに使用されます*
      * リンク
      * ボタン

## コンテンツエディターのアクティビティ

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>エクスペリエンスフラグメントをAdobe Targetに書き出す前に公開します。

## マーケターアクティビティ

### GeoTargetingを使用するオーディエンスの作成{#marketer-audience}

1. 組織[Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.experiencecloud.adobe.com)に移動します。
1. Adobe IDを使用してログインし、正しい組織に属していることを確認します。
1. ソリューション切り替えボタンで、「**Target**」をクリックし、**Adobe Targetを起動します。**

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. 「**オファー**」タブに移動し、「WKND」オファーを検索します。 エクスペリエンスフラグメントのバリエーションのリストを、AEMからHTMLオファーとして書き出して表示できるようになります。 各オファーは、1つの状態に対応します。 例えば、*WKND SkateFest California*&#x200B;は、カリフォルニア州からWKNDサイト訪問者に提供されるオファーです。

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. メインナビゲーションで「**オーディエンス**」をクリックします。

   マーケターは、米国の各州から来たWKNDサイト訪問者に対して、50個の個別のオーディエンスを作成する必要があります。

1. オーディエンスを作成するには、「**オーディエンスを作成**」ボタンをクリックし、オーディエンスの名前を指定します。

   **オーディエンス名の形式：WKND-\&lt;>state *\>***

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. **ルールを追加/ Geo**&#x200B;をクリックします。
1. 「**選択**」をクリックし、次のいずれかのオプションを選択します。
   * country（国）
   * **状態** *（WKNDサイトSkateFestキャンペーンの状態の選択）*
   * City
   * 郵便番号
   * 緯度
   * 経度
   * DMA
   * 携帯電話会社

   **地域**  — オーディエンスを使用して、国、都道府県、市区町村、郵便番号、DMA、携帯電話会社など、地理的な場所に基づいてユーザーをターゲットに設定します。位置情報パラメーターを使用すると、訪問者の地域情報に基づいてアクティビティとエクスペリエンスをターゲット設定できます。 このデータは、訪問者のIPアドレスに基づいて、各Targetリクエストと共に送信されます。 他のターゲット値と同様に、これらのパラメーターを選択します。

   >[!NOTE]
   >訪問者のIPアドレスがmboxリクエストで訪問（セッション）ごとに1回渡され、その訪問者の地域ターゲットパラメーターが解決されます。

1. 演算子「**一致**」を選択し、適切な値を指定します(例：カリフォルニア州)と&#x200B;**変更を保存**&#x200B;します。 この例では、状態名を指定します。

   ![Adobe Target — 地域ルール](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >1つのオーディエンスに複数のルールを割り当てることができます。

1. 手順6～9を繰り返して、他の状態のオーディエンスを作成します。

   ![Adobe Target - WKNDオーディエンス](assets/personalization-use-case-1/adobe-target-audiences-50.png)

この時点で、米国の様々な州のすべてのWKNDサイト訪問者用のオーディエンスが正常に作成され、各州に対応するHTMLオファーも作成されました。 次に、エクスペリエンスのターゲット設定アクティビティを作成して、WKNDサイトのホームページに対応するオファーを使用してオーディエンスをターゲット設定します。

### GeoTargetingを使用するアクティビティの作成

1. Adobe Targetウィンドウで、「**アクティビティ**」タブに移動します。
1. 「**アクティビティを作成**」をクリックし、「**エクスペリエンスのターゲット設定**」アクティビティタイプを選択します。
1. **Web**&#x200B;チャネルを選択し、**Visual Experience Composer**&#x200B;を選択します。
1. **アクティビティURL**&#x200B;を入力し、「**次へ**」をクリックしてVisual Experience Composerを開きます。

   WKNDサイトホームページの発行URL:http://localhost:4503/content/wknd/en.html

   ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/target-activity.png)

1. **Visual Experience Composer**&#x200B;を読み込むには、ブラウザーで「**安全でないスクリプトを読み込む**&#x200B;を許可」を有効にして、ページを再読み込みします。

   ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. WKNDサイトのホームページがVisual Experience Composerエディターで開いていることに注意してください。

   ![VEC](assets/personalization-use-case-1/vec.png)

1. VECにオーディエンスを追加するには、「オーディエンス」の下の「**エクスペリエンスのターゲットを追加**」をクリックし、WKND-Californiaオーディエンスを選択して、「**次へ**」をクリックします。

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. VEC内のWKNDサイトページをクリックし、WKND-Californiaオーディエンス用のオファーを追加するHTML要素を選択し、「**置換**」オプションを選択して、「**HTMLオファー**」を選択します。

   ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/vec-selecting-div.png)

1. オファー選択UIから&#x200B;**WKND SkateFest California** WKND-California **オーディエンスのHTMLオファーを選択し、「**&#x200B;完了&#x200B;**」をクリックします。**
1. これで、WKND-CaliforniaオーディエンスのWKNDサイトページに追加された&#x200B;**WKND SkateFest California** HTMLオファーを確認できます。
1. 手順7～10を繰り返して、他の状態のエクスペリエンスのターゲット設定を追加し、対応するHTMLオファーを選択します。
1. 「**次へ**」をクリックして続行すると、オーディエンスとエクスペリエンスのマッピングが表示されます。
1. 「**次へ**」をクリックして、「目標と設定」に移動します。
1. レポートソースを選択し、アクティビティの主な目標を特定します。 このシナリオでは、レポートソースとして&#x200B;**Adobe Target**&#x200B;を選択し、アクティビティを&#x200B;**コンバージョン**&#x200B;として測定し、ページを表示したアクション、WKND SkateFest詳細ページを指すURLを選択します。

   ![目標とターゲティング — ターゲット](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >また、レポートソースとしてAdobe Analyticsを選択することもできます。

1. 現在のアクティビティ名の上にマウスポインターを置くと、名前を&#x200B;**WKND SkateFest - USA**&#x200B;に変更し、**保存して閉じる**&#x200B;操作を実行できます。
1. アクティビティの詳細画面で、「**アクティビティをアクティブ化**」を必ず選択します。

   ![アクティビティを有効化](assets/personalization-use-case-1/activate-activity.png)

1. これで、WKND SkateFestキャンペーンは、すべてのWKNDサイト訪問者に公開されます。
1. [WKNDサイトのホームページ](http://localhost:4503/content/wknd/en.html)に移動すると、位置情報(*)に基づくWKND SkateFestオファーが表示されます。カリフォルニア*)。

   ![アクティビティQA](assets/personalization-use-case-1/wknd-california.png)

### TargetアクティビティQA

1. 「**アクティビティの詳細/概要**」タブで、「**アクティビティQA**」ボタンをクリックすると、すべてのエクスペリエンスに直接QAリンクを表示できます。

   ![アクティビティQA](assets/personalization-use-case-1/activity-qa.png)

1. [WKNDサイトのホームページ](http://localhost:4503/content/wknd/en.html)に移動すると、地域（状態）に基づくWKND SkateFestオファーを確認できます。
1. 以下のビデオを見て、オファーがページに配信される方法、レスポンストークンのカスタマイズ方法、品質チェックの実行方法をご確認ください。

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## 概要

この章では、コンテンツエディターが、Adobe Experience Manager内でWKND SkateFestキャンペーンをサポートするすべてのコンテンツを作成し、HTMLオファーとしてAdobe Targetに書き出して、ユーザーの地域に基づいてエクスペリエンスのターゲット設定を作成しました。
