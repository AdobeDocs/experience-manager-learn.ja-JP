---
title: AEM エクスペリエンスフラグメントと Adobe Target を使用したパーソナライゼーション
description: Adobe Experience Manager エクスペリエンスフラグメントと Adobe Target を使用してパーソナライズされたエクスペリエンスを作成して配信する方法を説明するエンドツーエンドのチュートリアル。
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 47446e2a-73d1-44ba-b233-fa1b7f16bc76
duration: 1088
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1663'
ht-degree: 100%

---

# AEM エクスペリエンスフラグメントと Adobe Target を使用したパーソナライゼーション

AEM エクスペリエンスフラグメントを HTML オファーとして Adobe Target に書き出すことができるので、AEM の使いやすさと機能を、Target の強力な自動インテリジェンス（AI）および機械学習（ML）機能と組み合わせて、エクスペリエンスを大規模にテストしパーソナライズできます。

AEM では、すべてのコンテンツとアセットを一元化して、パーソナライゼーション戦略を促進します。AEM を使用すると、デスクトップ、タブレットおよびモバイルデバイス用のコンテンツを、コードを記述せずに 1 か所で簡単に作成できます。デバイスごとにページを作成する必要はありません。AEM が、コンテンツを使用して各エクスペリエンスを自動的に調整します。

Target を使用すると、行動、コンテキストおよびオフラインの変数を組み込んだルールベースアプローチと AI 主導の機械学習アプローチを組み合わせて、パーソナライズされたエクスペリエンスを大規模に提供できます。Target では、A/B テストや多変量分析テスト（MVT）を簡単にセットアップおよび実行して、最適なオファー、コンテンツおよびエクスペリエンスを見極めることができます。

エクスペリエンスフラグメントは、コンテンツ作成者を、Target を使用してビジネス成果を促進しているマーケターにリンクするための大きな一歩となります。

## シナリオの概要

WKND サイトでは、web サイトを通じて米国全土での **SkateFest Challenge** を発表する予定で、サイトユーザーに各州で実施されるオーディションに参加登録してもらいたいと考えています。マーケターには、ユーザーの場所に関連するバナーメッセージとイベント詳細ページへのリンクを含んだ、WKND サイトのホームページでキャンペーンを実行するタスクが割り当てられています。WKND サイトのホームページを調べ、ユーザーの現在の場所に基づいてパーソナライズされたエクスペリエンスを作成してユーザーに提供する方法を学びましょう。

### 関係するユーザー

この演習では、次のユーザーが関与し、管理者アクセスが必要なタスクを実行する必要があります。

* **コンテンツプロデューザー／コンテンツ編集者**（Adobe Experience Manager）
* **マーケター**（Adobe Target／最適化チーム）

### 前提条件

* **AEM**
   * それぞれがローカルホスト 4502 と 4503 で実行中の [AEM オーサーインスタンスとパブリッシュインスタンス](./implementation.md#getting-aem)。
* **Experience Cloud**
   * 組織の Adobe Experience Cloud にアクセス - `https://<yourcompany>.experiencecloud.adobe.com`
   * 次のソリューションをプロビジョニングされた Experience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

### WKND サイトのホームページ

![AEM Target シナリオ 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. マーケターは、AEM コンテンツエディターと WKND SkateFest キャンペーンに関するディスカッションを開始し、要件の詳細を説明します。
   * ***要件***：米国の各州からの訪問者向けにパーソナライズされたコンテンツを含んだ WKND サイトホームページで、WKND SkateFest キャンペーンを宣伝します。背景画像、テキストおよびボタンを含んだ新しいコンテンツブロックをホームページカルーセルの下に追加します。
      * **背景画像**：画像は、WKND サイトページを訪問しているユーザーの現在の州に関連するものにする必要があります。
      * **テキスト**：「Sign Up for the Auditions」
      * **ボタン**：WKND SkateFest ページを指す「Event Details」
      * **WKND SkateFest ページ**：オーディション会場、日付、時刻などのイベントの詳細が記載された新しいページ。
1. 要件に基づいて、AEM コンテンツエディターはコンテンツブロックのエクスペリエンスフラグメントを作成して、オファーとして Adobe Target に書き出します。米国のすべての州向けにパーソナライズされたコンテンツを提供するには、コンテンツ作成者がエクスペリエンスフラグメントのプライマリバリエーションを 1 つ作成した後、州ごとに 1 つずつ、他の 50 のバリエーションを作成します。関連する画像やテキストを含んだ各州のバリエーションのコンテンツは、手動で編集できます。エクスペリエンスフラグメントを作成する際、コンテンツエディターは、アセットファインダーオプションを使用して、AEM Assets 内の使用可能なすべてのアセットにすばやくアクセスできます。エクスペリエンスフラグメントが Adobe Target に書き出されると、そのすべてのバリエーションも Adobe Target にオファーとしてプッシュされます。

1. エクスペリエンスフラグメントを AEM から Adobe Target にオファーとして書き出した後、マーケターはこれらのオファーを使用して Target でアクティビティを作成できます。WKND サイトの SkateFest キャンペーンに基づいて、マーケターは、パーソナライズされたエクスペリエンスを作成して各州からの WKND サイト訪問者に提供する必要があります。エクスペリエンスのターゲット設定アクティビティを作成するには、マーケターはオーディエンスを特定する必要があります。WKND SkateFest キャンペーンでは、WKND web サイトの訪問元の場所に基づいて、50 個の個別のオーディエンスを作成する必要があります。
   * [オーディエンス](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html?lang=ja#section_3F32DA46BDF947878DD79DBB97040D01)は、アクティビティのターゲットを定義し、ターゲティングが使用可能な場所であればどこでも使用されます。ターゲットオーディエンスは、訪問者条件の定義済みセットです。オファーは、特定のオーディエンス（すなわちセグメント）をターゲットにすることができます。そのオーディエンスに属する訪問者のみに、自分をターゲットにしたエクスペリエンスが表示されます。例えば、特定のブラウザーを使用する訪問者や特定の位置情報を持つ訪問者で構成されるオーディエンスにオファーを提供できます。
   * [オファー](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html?lang=ja#section_973D4CC4CEB44711BBB9A21BF74B89E9)は、キャンペーンまたはアクティビティ中に web ページに表示されるコンテンツです。Web ページをテストする際は、各場所で様々なオファーを使用して各エクスペリエンスの成功を測定します。オファーには、次のような様々なタイプのコンテンツを含めることができます。
      * 画像
      * テキスト
      * **HTML**
         * *このシナリオのアクティビティでは、HTML オファーを使用します*
      * リンク
      * ボタン

## コンテンツエディターアクティビティ

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>Adobe Target に書き出す前に、エクスペリエンスフラグメントを公開します。

## マーケターアクティビティ

### geotargeting を使用したオーディエンスの作成 {#marketer-audience}

1. 組織の [Adobe Experience Cloud](https://experiencecloud.adobe.com/) に移動（`<https://<yourcompany>.experiencecloud.adobe.com`）します。
1. Adobe ID を使用してログインし、正しい組織に属していることを確認します。
1. ソリューションスイッチャーで、「**Target**」をクリックして Adobe Target を&#x200B;**起動**&#x200B;します。

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. 「**オファー**」タブに移動し、「WKND」オファーを検索します。AEM から HTML オファーとして書き出されたエクスペリエンスフラグメントバリエーションのリストが表示されます。各オファーは 1 つの州に対応します。例えば、*WKND SkateFest California* は、カリフォルニア州からの WKND サイト訪問者に提供されるオファーです。

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. メインナビゲーションで、「**オーディエンス**」をクリックします。

   マーケターは、米国の各州からの WKND サイト訪問者向けに 50 個のオーディエンスを個別に作成する必要があります。

1. オーディエンスを作成するには、「**オーディエンスを作成**」ボタンをクリックし、オーディエンスの名前を入力します。

   **オーディエンス名の形式：WKND-\&lt;*州名*\>**

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. **ルールを追加／地域**&#x200B;をクリックします。
1. 「**選択**」をクリックして、次のオプションのいずれかを選択します。
   * 国
   * **州** *（WKND サイトの SkateFest キャンペーンの州を選択）*
   * 市区町村
   * 郵便番号
   * 緯度
   * 経度
   * DMA
   * 携帯電話会社

   **地域** - オーディエンスを使用して、国、州／県、市区町村、郵便番号、DMA、携帯電話会社などの位置情報に基づいてユーザーをターゲットにします。位置情報パラメーターを使用すると、訪問者の地域に基づいてアクティビティやエクスペリエンスのターゲットを絞ることができます。このデータは、訪問者の IP アドレスに基づいており、各 Target リクエストと共に送信されます。これらのパラメーターは、他のターゲティング値と同様に選択します。

   >[!NOTE]
   >訪問者の IP アドレスは、訪問（セッション）ごとに 1 回、mbox リクエストと共に渡されて、その訪問者の geotargeting パラメーターを解決します。

1. **一致**&#x200B;の演算子を選択し、適切な値（例：California）を入力して、変更内容を&#x200B;**保存**&#x200B;します。ここでは、州名を入力します。

   ![Adobe Target - 地域ルール](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >1 つのオーディエンスに複数のルールを割り当てることができます。

1. 手順 6～9 を繰り返して、他の州のオーディエンスを作成します。

   ![Adobe Target - WKND オーディエンス](assets/personalization-use-case-1/adobe-target-audiences-50.png)

現時点では、米国の様々な州にわたるすべての WKND サイト訪問者向けのオーディエンスを問題なく作成し、州ごとに対応する HTML オファーも用意できました。それでは、WKND サイトホームページの対応するオファー使用してオーディエンスをターゲティングする「エクスペリエンスのターゲット設定」アクティビティを作成しましょう。

### geotargeting を使用したアクティビティの作成

1. Adobe Target ウィンドウから「**アクティビティ**」タブに移動します。
1. 「**アクティビティを作成**」をクリックし、**エクスペリエンスのターゲット設定**&#x200B;アクティビティタイプを選択します。
1. **Web** チャネルを選択し、**Visual Experience Composer** を選択します。
1. **アクティビティ URL** を入力し、「**次へ**」をクリックして Visual Experience Composer を開きます。

   WKND サイトホームページのパブリッシュ URL：http://localhost:4503/content/wknd/en.html

   ![「エクスペリエンスのターゲット設定」アクティビティ](assets/personalization-use-case-1/target-activity.png)

1. **Visual Experience Composer** を読み込むには、ブラウザーで「**安全でないスクリプトの読み込みを許可する**」を有効にして、ページをリロードします。

   ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. WKND サイトのホームページが Visual Experience Composer エディターで開いていることに注意してください。

   ![VEC](assets/personalization-use-case-1/vec.png)

1. VEC にオーディエンスを追加するには、オーディエンスで「**エクスペリエンスのターゲット設定を追加**」をクリックし、WKND-California オーディエンスを選択して、「**次へ**」をクリックします。

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. VEC 内の WKND サイトページをクリックし、HTML 要素を選択して WKND-California オーディエンス向けのオファーを追加し、「**置換文字列**」オプションを選択して「**HTML オファー**」を選択します。

   ![「エクスペリエンスのターゲット設定」アクティビティ](assets/personalization-use-case-1/vec-selecting-div.png)

1. オファー選択 UI から **WKND-California** オーディエンス向けの **WKND SkateFest California** HTML オファーを選択し、「**完了**」をクリックします。
1. これで、WKND-California オーディエンス向けに WKND サイトページに **WKND SkateFest California** HTML オファーが追加されたことが確認できるようになります。
1. 手順 7～10 を繰り返して、他の州の「エクスペリエンスのターゲット設定」を追加し、それぞれに対応する HTML オファーを選択します。
1. 「**次へ**」をクリックして続行すると、オーディエンスからエクスペリエンスへのマッピングが表示されます。
1. 「**次へ**」をクリックして、目標と設定に移動します。
1. レポートソースを選択し、アクティビティの主な目標を特定します。このシナリオでは、レポートソースとして **Adobe Target** を、アクティビティの測定対象として&#x200B;**コンバージョン**&#x200B;を、アクションとして「ページを閲覧」をそれぞれ選択し、WKND SkateFest Details ページを指す URL を指定します。

   ![目標とターゲティング - Target](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >レポートソースとして Adobe Analytics を選択することもできます。

1. 現在のアクティビティ名にポインタを合わせて、名前を **WKND SkateFest - USA** に変更し、変更内容を&#x200B;**保存して閉じ**&#x200B;ます。
1. アクティビティの詳細画面から、アクティビティを&#x200B;**アクティベート**&#x200B;してください。

   ![アクティビティをアクティベート](assets/personalization-use-case-1/activate-activity.png)

1. これで、WKND SkateFest キャンペーンが、すべての WKND サイト訪問者に公開されました。
1. [WKND サイトのホームページ](http://localhost:4503/content/wknd/en.html)に移動すると、位置情報（*州：California*）に基づいて WKND SkateFest オファーが表示されます。

   ![アクティビティ QA](assets/personalization-use-case-1/wknd-california.png)

### Target アクティビティ QA

1. **アクティビティの詳細／概要**&#x200B;タブで、「**アクティビティ QA**」ボタンをクリックすると、すべてのエクスペリエンスへの直接 QA リンクを取得できます。

   ![アクティビティ QA](assets/personalization-use-case-1/activity-qa.png)

1. [WKND サイトのホームページ](http://localhost:4503/content/wknd/en.html)に移動すると、位置情報（州）に基づいて WKND SkateFest オファーが表示されます。
1. オファーがページに提供される仕組み、応答トークンをカスタマイズする方法および品質チェックを実行する方法については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## まとめ

この章では、コンテンツ編集者が WKND SkateFest キャンペーンをサポートするすべてのコンテンツを Adobe Experience Manager 内で作成し、それを HTML オファーとして Adobe Target に書き出して、ユーザーの位置情報に基づいてエクスペリエンスのターゲット設定を作成できました。
