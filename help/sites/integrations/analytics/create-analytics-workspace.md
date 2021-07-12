---
title: Analysis Workspaceでのデータ分析
description: Adobe Experience Managerサイトからキャプチャされたデータを、Adobe Analyticsレポートスイートの指標およびディメンションにマッピングする方法について説明します。 Adobe AnalyticsのAnalysis Workspace機能を使用して、詳細なレポートダッシュボードを作成する方法を説明します。
feature: 分析
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6409
thumbnail: KT-6296.jpg
topic: 統合
role: User
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '2204'
ht-degree: 1%

---


# Analysis Workspaceでのデータ分析

Adobe Experience Managerサイトからキャプチャされたデータを、Adobe Analyticsレポートスイートの指標およびディメンションにマッピングする方法について説明します。 Adobe AnalyticsのAnalysis Workspace機能を使用して、詳細なレポートダッシュボードを作成する方法を説明します。

## 作成する内容

WKNDマーケティングチームは、ホームページで最も効果が高いコールトゥアクション(CTA)ボタンを把握したいと考えています。 このチュートリアルでは、Analysis Workspaceで新しいプロジェクトを作成し、様々なCTAボタンのパフォーマンスを視覚化し、サイトでのユーザーの行動を理解します。 次の情報は、ユーザーがWKNDホームページのコールトゥアクション(CTA)ボタンをクリックすると、Adobe Analyticsを使用して取得されます。

**Analytics変数**

現在追跡中のAnalytics変数を以下に示します。

* `eVar5` -   `Page template`
* `eVar6` -  `Page Id`
* `eVar7` -  `Page last modified date`
* `eVar8` -  `CTA Button Id`
* `eVar9` -  `Page Name`
* `event8` -  `CTA Button Click event`
* `prop8` -  `CTA Button Id`

![CTA Click Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### 目的 {#objective}

1. 新しいレポートスイートを作成するか、既存のレポートスイートを使用します。
1. レポートスイートで、 [コンバージョン変数(eVars)](https://docs.adobe.com/content/help/en/analytics/admin/admin-tools/conversion-variables/conversion-var-admin.html)と[成功イベント（イベント）](https://docs.adobe.com/help/en/analytics/admin/admin-tools/success-events/success-event.html)を設定します。
1. [Analysis Workspaceプロジェクト](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/home.html)を作成して、インサイトをすばやく作成、分析、共有できるツールを使用してデータを分析します。
1. Analysis Workspaceプロジェクトを他のチームメンバーと共有します。

## 前提条件

このチュートリアルは、Adobe Analytics](./track-clicked-component.md)を使用した[クリックされたコンポーネントの追跡の継続で、以下の点を前提としています。

* [Adobe Analytics拡張機能](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html)が有効な&#x200B;**Launchプロパティ**
* **Adobe** Analyticstest/devレポートスイートIDとトラッキングサーバー。[新しいレポートスイート](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)の作成については、次のドキュメントを参照してください。
* [Experience Platform](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) データレイヤーが有効なAEMサイトに読み込まれたLaunchプロパ [ティを使用して設定されたAdobeデバッガーブラウザー拡張機能。https://wknd.site/us/en](https://wknd.site/us/en.html) 

## コンバージョン変数(eVar)と成功イベント(event)

Custom Insightコンバージョン変数(またはeVar)は、サイトで選択したWebページのAdobeコードに配置されます。 その主な目的は、カスタムマーケティングレポートでコンバージョン成功指標をセグメント化することです。 eVarは訪問ベースで、cookieと同様に機能します。 eVar変数に渡された値は、所定の期間ユーザーに従います。

eVarを訪問者の値に設定すると、Adobeではその値が期限切れになるまで自動的に記憶されます。 eVar値がアクティブな間に訪問者が発生した成功イベントは、eVar値にカウントされます。

eVarは、次のような原因と効果の測定に最適です。

* 売上高に影響を与えた内部キャンペーン
* 最終的に登録につながったバナー広告
* 注文前に内部検索が使用された回数

成功イベントは、追跡可能なアクションです。 成功イベントを決定します。 例えば、訪問者がCTAボタンをクリックした場合、クリックイベントは成功イベントと見なすことができます。

### eVarの設定

1. Adobe Experience Cloudホームページから、組織を選択し、Adobe Analyticsを起動します。

   ![Analytics AEP](assets/create-analytics-workspace/analytics-aep.png)

1. Analyticsツールバーで、**管理者** / **レポートスイート**&#x200B;をクリックし、レポートスイートを見つけます。

   ![Analyticsレポートスイート](assets/create-analytics-workspace/select-report-suite.png)

1. レポートスイート/ **設定を編集** / **コンバージョン** / **コンバージョン変数**&#x200B;を選択します。

   ![Analyticsコンバージョン変数](assets/create-analytics-workspace/conversion-variables.png)

1. 「**新しい**&#x200B;を追加」オプションを使用して、次のようにスキーマをマッピングするコンバージョン変数を作成します。

   * `eVar5` -   `Page Template`
   * `eVar6` -  `Page ID`
   * `eVar7` -  `Last Modified Date`
   * `eVar8` -  `Button Id`
   * `eVar9` -  `Page Name`

   ![新しいeVarの追加](assets/create-analytics-workspace/add-new-evars.png)

1. 各eVarに適切な名前と説明を入力し、変更を保存&#x200B;**します。**&#x200B;次の節では、これらのeVarを使用してAnalysis Workspaceプロジェクトを作成します。 したがって、わかりやすい名前を付けると、変数を見つけやすくなります。

   ![eVar](assets/create-analytics-workspace/evars.png)

### 成功イベントの設定

次に、CTAボタンのクリックを追跡する偶数を作成します。

1. **Report Suite Manager**&#x200B;ウィンドウで「**レポートスイートID**」を選択し、「**設定を編集**」をクリックします。
1. **コンバージョン** / **成功イベント**&#x200B;をクリックします。
1. 「**新規追加**」オプションを使用して、新しいカスタム成功イベントを作成し、CTAボタンのクリックを追跡してから、変更を保存&#x200B;**します。**
   * `Event`：`event8`
   * `Name`:`CTA Click`
   * `Type`:`Counter`

   ![eVar](assets/create-analytics-workspace/add-success-event.png)

## Analysis Workspaceでの新しいプロジェクトの作成 {#workspace-project}

Analysis Workspaceは、分析を構築し、インサイトをすばやく共有できる、柔軟なブラウザーツールです。 ドラッグ&amp;ドロップインターフェイスを使用すると、分析を作成し、ビジュアライゼーションを追加して、データを有効にし、データセットをキュレーションし、組織内の任意のユーザーとプロジェクトを共有、スケジュールできます。

次に、新しい[プロジェクト](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/build-workspace-project/t-freeform-project.html)を作成し、ダッシュボードを作成してサイト全体でのCTAボタンのパフォーマンスを分析します。

1. Analyticsツールバーで「**Workspace**」を選択し、「**新しいプロジェクトを作成**」をクリックします。

   ![ワークスペース](assets/create-analytics-workspace/create-workspace.png)

1. **空白のプロジェクト**&#x200B;から開始するか、Adobeが提供する事前ビルドテンプレートか、組織が作成したカスタムテンプレートのいずれかを選択します。 想定している分析や使用事例に応じて、いくつかのテンプレートを使用できます。 [使用可能な](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html) 様々なテンプレートオプションについて詳しく説明します。

   Workspaceプロジェクトでは、左側のパネルからパネル、テーブル、ビジュアライゼーションおよびコンポーネントにアクセスします。 これらはプロジェクトの構成要素です。

   * **[コンポーネント](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html)**  — コンポーネントは、ディメンション、指標、セグメントまたは日付範囲で、すべてフリーフォームテーブルに組み合わせて、ビジネスの質問への回答を開始できます。分析を開始する前に、各コンポーネントのタイプについて理解しておく必要があります。 コンポーネントの用語を習得したら、フリーフォームテーブルでドラッグ&amp;ドロップを開始し、分析を構築します。
   * **[ビジュアライゼーション](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html)**  — 棒グラフや折れ線グラフなどのビジュアライゼーションをデータに追加して、データに命を吹き込みます。左端のパネルで、中央のビジュアライゼーションアイコンを選択し、使用可能なビジュアライゼーションの完全なリストを表示します。
   * **[パネル](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/panels/panels.html)**  — パネルは、テーブルとビジュアライゼーションのコレクションです。パネルには、Workspaceの左上のアイコンからアクセスできます。 パネルは、期間、レポートスイート、分析の使用例に従ってプロジェクトを整理する場合に役立ちます。 Analysis Workspaceでは、次のパネルタイプを使用できます。

   ![テンプレートの選択](assets/create-analytics-workspace/workspace-tools.png)

### Analysis Workspaceでのデータのビジュアライゼーションの追加

次に、WKNDサイトのホームページでユーザーがコールトゥアクション(CTA)ボタンとどのようにやり取りするかを視覚的に表現した表を作成します。 このような表現を作成するには、「[クリックされたコンポーネントを追跡](./track-clicked-component.md)」で収集したデータをAdobe Analyticsと共に使用します。 以下に、WKNDサイトの「コールトゥアクション」ボタンを使用したユーザーインタラクションで追跡されるデータの概要を示します。

* `eVar5` -   `Page template`
* `eVar6` -  `Page Id`
* `eVar7` -  `Page last modified date`
* `eVar8` -  `CTA Button Id`
* `eVar9` -  `Page Name`
* `event8` -  `CTA Button Click event`
* `prop8` -  `CTA Button Id`

1. **ページ**&#x200B;ディメンションコンポーネントをフリーフォームテーブルにドラッグ&amp;ドロップします。 これで、ページ名(eVar9)と対応するページビュー（回数）をテーブル内に表示するビジュアライゼーションを表示できます。

   ![ページDimension](assets/create-analytics-workspace/evar9-dimension.png)

1. **CTA Click**(event8)指標を回数指標にドラッグ&amp;ドロップし、置き換えます。 これで、ページ名(eVar9)と、対応するページ上のCTAクリックイベント数を表示するビジュアライゼーションを表示できます。

   ![ページ指標 — CTAクリック](assets/create-analytics-workspace/evar8-cta-click.png)

1. では、テンプレートタイプ別にページを分類します。 コンポーネントからページテンプレート指標を選択し、ページテンプレート指標を「ページ名」ディメンションにドラッグ&amp;ドロップします。 これで、テンプレートタイプ別に分類されたページ名を表示できます。

   * **前**

      ![eVar5](assets/create-analytics-workspace/evar5.png)

   * **後**

      ![eVar5指標](assets/create-analytics-workspace/evar5-metrics.png)

1. WKNDサイトページでユーザーがCTAボタンとどのようにやり取りするかを理解するには、ボタンID(eVar8)指標を追加して、ページテンプレート指標をさらに分類する必要があります。

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. 以下に、WKNDサイトをページテンプレート別に分類し、さらにWKNDサイトのクリックしてアクション(CTA)ボタンとのユーザーインタラクション別に分類した視覚的な表現を示します。

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. Adobe Analytics Classificationsを使用して、ボタンIDの値を、よりわかりやすい名前に置き換えることができます。 特定の指標の分類を作成する方法について詳しくは、[こちら](https://docs.adobe.com/content/help/en/analytics/components/classifications/c-classifications.html)を参照してください。 この場合、ボタンIDをユーザーにわかりやすい名前にマッピングする`Button Section (Button ID)`分類指標を`eVar8`用に設定します。

   ![ボタンセクション](assets/create-analytics-workspace/button-section.png)

## 分析変数への分類の追加

### コンバージョンの分類

Analytics分類は、Analytics変数データを分類し、レポートを生成する際に様々な方法でデータを表示する方法です。 Analytics WorkspaceレポートでボタンIDがどのように表示されるかを改善するために、ボタンID(eVar8)の分類変数を作成します。 分類する際に、変数とその変数に関連するメタデータとの間に関係を確立します。

次に、Analytics変数の分類を作成します。

1. **管理者**&#x200B;ツールバーメニューから&#x200B;**レポートスイート**&#x200B;を選択します。
1. **Report Suite Manager**&#x200B;ウィンドウで&#x200B;**レポートスイートID**&#x200B;を選択し、**設定を編集** / **コンバージョン** / **コンバージョンの分類**&#x200B;をクリックします。

   ![コンバージョンの分類](assets/create-analytics-workspace/conversion-classification.png)

1. **分類タイプ**&#x200B;の選択ドロップダウンリストから、変数(eVar8ボタンID)を選択して分類を追加します。
1. 「分類」セクションの下に表示される分類変数の右横にある矢印をクリックして、新しい分類を追加します。

   ![コンバージョンの分類タイプ](assets/create-analytics-workspace/select-classification-variable.png)

1. **分類の編集**&#x200B;ダイアログボックスで、テキスト分類に適した名前を指定します。 テキスト分類名を持つディメンションコンポーネントが作成されます。

   ![コンバージョンの分類タイプ](assets/create-analytics-workspace/new-classification.png)

1. **変更を保存します。**

### 分類インポーター

インポーターを使用して、分類をAdobe Analyticsにアップロードします。 インポートの前に、更新用にデータをエクスポートすることもできます。 インポートツールを使用してインポートするデータは、特定の形式にする必要があります。 Adobeには、適切なヘッダーの詳細をすべて含んだデータテンプレートをタブ区切りデータファイルにダウンロードするオプションが用意されています。 このテンプレートに新しいデータを追加し、FTPを使用してブラウザーにデータファイルをインポートできます。

#### 分類テンプレート

分類をマーケティングレポートにインポートする前に、分類データファイルの作成に役立つテンプレートをダウンロードできます。 データファイルでは、目的の分類を列見出しとして使用し、レポートデータセットを適切な分類見出しの下に整理します。

次に、ボタンID(eVar8)変数の分類テンプレートをダウンロードします。

1. **管理者** / **分類インポーター**&#x200B;に移動します。
1. コンバージョン変数の分類テンプレートを&#x200B;**「テンプレートをダウンロード** 」タブからダウンロードします。
   ![コンバージョンの分類タイプ](assets/create-analytics-workspace/classification-importer.png)

1. 「テンプレートのダウンロード」タブで、データテンプレートの設定を指定します。
   * **レポートスイートの選択** :テンプレートで使用するレポートスイートを選択します。レポートスイートとデータセットが一致する必要があります。
   * **分類するデータセット** :データファイルのデータのタイプを選択します。このメニューには、分類用に設定されているレポートスイート内のすべてのレポートが含まれます。
   * **エンコーディング** :データファイルの文字エンコーディングを選択します。デフォルトのエンコーディング形式はUTF-8です。

1. 「**ダウンロード**」をクリックし、テンプレートファイルをローカルシステムに保存します。 このテンプレートファイルは、タブ区切りのデータファイル（ファイル名の拡張子は.tab）で、ほとんどのスプレッドシートアプリケーションでサポートされています。
1. 任意のエディターを使用して、タブ区切りのデータファイルを開きます。
1. 「 」セクションの手順9の各eVar9値について、ボタンID(eVar9)と対応するボタン名をタブ区切りファイルに追加します。

   ![キー値](assets/create-analytics-workspace/key-value.png)

1. **** タブ区切りファイルを保存します。
1. 「**ファイルのインポート**」タブに移動します。
1. ファイルのインポート先を設定します。
   * **レポートスイートの選択** :WKNDサイトAEM（レポートスイート）
   * **分類するデータセット** :ボタンId(コンバージョン変数eVar8)
1. 「**ファイル**&#x200B;を選択」オプションをクリックして、タブ区切りのファイルをシステムからアップロードし、「**ファイルを読み込み**」をクリックします。

   ![ファイルインポーター](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > インポートが正常に完了すると、エクスポートに適切な変更が即座に表示されます。 ただし、ブラウザーでのインポートの場合は最大4時間、FTPでのインポートの場合は最大24時間かかります。

#### コンバージョン変数の分類変数への置き換え

1. Analyticsツールバーから&#x200B;**Workspace**&#x200B;を選択し、このチュートリアルの「Analysis Workspace](#workspace-project)での新しいプロジェクトの作成」で作成したワークスペースを開きます。[

   ![ワークスペースボタンID](assets/create-analytics-workspace/workspace-report-button-id.png)

1. 次に、ワークスペースの&#x200B;**ボタンID**&#x200B;指標を置き換えます。この指標には、コールトゥアクション(CTA)ボタンのIDが表示され、前の手順で作成した分類名が表示されます。

1. コンポーネントファインダーから&#x200B;**WKND CTA Buttons**&#x200B;を検索し、**WKND CTA Buttons (Button Id)**&#x200B;ディメンションをButton Id指標にドラッグ&amp;ドロップして置き換えます。

   * **前**

      ![前のワークスペースボタン](assets/create-analytics-workspace/wknd-button-before.png)
   * **後**

      ![後ろのワークスペースボタン](assets/create-analytics-workspace/wknd-button-after.png)

1. コールトゥアクション(CTA)ボタンのボタンIDを含むボタンID指標が、分類テンプレートで指定された対応する名前に置き換えられました。
1. ここでは、Analytics WorkspaceテーブルとWKNDホームページを比較し、CTAボタンのクリック数とその分析について説明します。 ワークスペースのフリーフォームテーブルのデータに基づいて、22回のユーザーが「**SKI NOW**」ボタンをクリックし、4回のWKNDホームページキャンピングが西オーストラリア&#x200B;**詳細を表示**」ボタンをクリックしたことが明らかです。

   ![CTAレポート](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. Adobe Analytics Workspaceプロジェクトを保存し、適切な名前と説明を指定してください。 オプションで、ワークスペースプロジェクトにタグを追加できます。

   ![プロジェクトを保存](assets/create-analytics-workspace/save-project.png)

1. プロジェクトを正常に保存したら、「共有」オプションを使用して、ワークスペースプロジェクトを他の同僚やチームメイトと共有できます。

   ![プロジェクトを共有](assets/create-analytics-workspace/share.png)

## おめでとうございます。

Adobe Experience Manager Siteから取り込んだデータをAdobe Analyticsのレポートスイートの指標やディメンションにマッピングする方法、指標の分類を実行する方法、Adobe AnalyticsのAnalysis Workspace機能を使用して詳細なレポートダッシュボードを作成する方法を学びました。

