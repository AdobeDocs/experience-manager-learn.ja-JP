---
title: Analysis Workspace でのデータの分析
description: Adobe Experience Manager サイトから取得しデータを、Adobe Analytics レポートスイートの指標とディメンションにマッピングする方法について説明します。Adobe Analytics の Analysis Workspace 機能を使用して、詳細なレポートダッシュボードを作成する方法を説明します。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: User
level: Intermediate
jira: KT-6409
thumbnail: KT-6296.jpg
doc-type: Tutorial
exl-id: b5722fe2-93bf-4b25-8e08-4cb8206771cb
badgeIntegration: label="統合" type="positive"
last-substantial-update: 2022-06-15T00:00:00Z
duration: 443
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2072'
ht-degree: 100%

---

# Analysis Workspace でのデータの分析

Adobe Experience Manager サイトから取得しデータを、Adobe Analytics レポートスイートの指標とディメンションにマッピングする方法について説明します。Adobe Analytics の Analysis Workspace 機能を使用して、詳細なレポートダッシュボードを作成する方法を説明します。

## 作ろうとしているもの {#what-build}

WKND マーケティングチームは、どの `Call to Action (CTA)` ボタンがホームページで最も効果が高いのかを把握したいと考えています。このチュートリアルでは、**Analysis Workspace** でプロジェクトを作成して、様々な CTA ボタンの効果を視覚化し、サイトでのユーザーの行動を把握します。ユーザーが WKND ホームページのコールトゥアクション（CTA）ボタンをクリックすると、Adobe Analytics を使用して次の情報が取得されます。

**Analytics 変数**

現在追跡中の Analytics 変数を以下に示します。

* `eVar5` - `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

![CTA クリック Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### 目的 {#objective}

1. レポートスイートを作成するか、既存のレポートスイートを使用します。
1. レポート スイートで[コンバージョン変数（eVars）](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html?lang=ja)と[成功イベント（Events）](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html?lang=ja)を設定します。
1. [Analysis Workspace プロジェクト](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html?lang=ja)を作成し、インサイトをすばやく作成、分析および共有できるツールを使用してデータを分析します。
1. Analysis Workspace プロジェクトを他のチームメンバーと共有します。

## 前提条件

このチュートリアルは[クリックされたコンポーネントの Adobe Analytics による追跡](./track-clicked-component.md)の続きであり、以下を前提としています。

* [Adobe Analytics 拡張機能](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=ja)で&#x200B;**タグプロパティ**&#x200B;が有効になっている
* **Adobe Analytics** テスト／開発レポートスイート ID とトラッキングサーバーがある。[レポートスイートの作成](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html?lang=ja)については、次のドキュメントを参照してください。
* タグプロパティが設定されている [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=ja) ブラウザー拡張機能が [WKND サイト](https://wknd.site/us/en.html)または Adobe Data Layer が有効になっている AEM サイトに読み込まれている。

## コンバージョン変数（eVars）と成功イベント（Event）

Custom Insight コンバージョン変数（eVar）は、サイトで選択した web ページの Adobe コードに配置されます。その主な目的は、カスタムマーケティングレポートでコンバージョン成功指標をセグメント化することです。eVar は訪問ベースであり、Cookie と同様に機能します。eVar 変数に渡された値は、所定の期間、ユーザーを追跡します。

eVar をある訪問者の値に設定すると、その値は期限切れになるまで自動的にアドビに記憶されます。eVar 値がアクティブな間に訪問者が遭遇した成功イベントは、eVar 値にカウントされます。

eVar は、次のような原因と効果の測定に最適です。

* 売上高に影響を与えた内部キャンペーン
* 最終的に登録に至ったバナー広告
* 注文前に内部検索が使用された回数

成功イベントは、追跡可能なアクションです。 成功イベントの内容を決定します。 例えば、訪問者が CTA ボタンをクリックした場合、クリックイベントは成功イベントと見なすことができます。

### eVar の設定

1. Adobe Experience Cloud ホームページから、組織を選択し、Adobe Analytics を起動します。

   ![Analytics AEP](assets/create-analytics-workspace/analytics-aep.png)

1. Analytics ツールバーで、**管理者**／**レポートスイート**&#x200B;をクリックし、レポートスイートを見つけます。

   ![Analytics レポートスイート](assets/create-analytics-workspace/select-report-suite.png)

1. レポートスイート／**設定を編集**／**コンバージョン**／**コンバージョン変数**&#x200B;を選択します。

   ![Analytics コンバージョン変数](assets/create-analytics-workspace/conversion-variables.png)

1. 「**新規追加**」オプションで、コンバージョン変数を作成して、以下のようにスキーマをマッピングします。

   * `eVar5` - `Page Template`
   * `eVar6` - `Page ID`
   * `eVar7` - `Last Modified Date`
   * `eVar8` - `Button Id`
   * `eVar9` - `Page Name`

   ![新しい eVar を追加](assets/create-analytics-workspace/add-new-evars.png)

1. それぞれの eVar に適切な名前と説明を入力し、変更内容を&#x200B;**保存**&#x200B;します。Analysis Workspace プロジェクトでは、適切な eVar 名が使用されるので、わかりやすい名前を付けると、変数を見つけやすくなります。

   ![eVar](assets/create-analytics-workspace/evars.png)

### 成功イベントの設定

次に、CTA ボタンのクリックをトラックするイベントを作成しましょう。

1. **レポートスイート管理者**&#x200B;ウィンドウで、**レポートスイート ID** を選択し、**設定を編集**&#x200B;をクリックします。
1. **コンバージョン**／**成功イベント**&#x200B;をクリックします
1. 「**新規追加**」オプションを使用し、カスタム成功イベントを作成して CTA ボタンのクリックをトラックし、変更内容を「**保存**」します。
   * `Event`：`event8`
   * `Name`：`CTA Click`
   * `Type`：`Counter`

   ![eVar](assets/create-analytics-workspace/add-success-event.png)

## Analysis Workspace でのプロジェクトの作成 {#workspace-project}

Analysis Workspace は、分析をビルドしてインサイトを素早く共有できる、柔軟なブラウザーツールです。ドラッグ＆ドロップインターフェイスを使用して、分析を作成し、ビジュアライゼーションを追加してデータに活気を与え、データセットをキュレートし、組織内の誰とでもプロジェクトを共有し、スケジュールすることができます。

次に、[プロジェクト](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/freeform-overview.html?lang=ja#analysis-workspace)を作成して、サイト全体での CTA ボタンのパフォーマンスを分析するダッシュボードを作成します。

1. Analytics ツールバーから&#x200B;**ワークスペース**&#x200B;を選択し、「**プロジェクトを新規作成**」をクリックします。

   ![ワークスペース](assets/create-analytics-workspace/create-workspace.png)

1. **空白のプロジェクト**&#x200B;から開始するか、アドビが提供する事前定義済みテンプレートまたは組織が作成したカスタムテンプレートのいずれかを選択します。想定している分析やユースケースに応じて、いくつかのテンプレートを使用できます。 利用可能な様々なテンプレートオプションについて、[詳細をご覧ください](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html?lang=ja)。

   ワークスペースプロジェクトでは、パネル、テーブル、ビジュアライゼーションおよびコンポーネントに、左パネルからアクセスできます。 プロジェクトの構築ブロックを構成します。

   * **[コンポーネント](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html?lang=ja)** - コンポーネントは、寸法、指標、セグメントや日付範囲です。このすべてをフリーフォームテーブルに組み合わせて、ビジネスに関する質問への回答を開始できます。 分析を開始する前に、コンポーネントのそれぞれのタイプについて理解しておく必要があります。 コンポーネントの用語を習得したら、フリーフォームテーブルでドラッグ＆ドロップを開始し、分析をビルドします。
   * **[ビジュアライゼーション](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html?lang=ja)** - 棒グラフや折れ線グラフなどのビジュアライゼーションをデータに追加して、データに活気を与えます。左端のパネルで、中央の「ビジュアライゼーション」アイコンを選択し、使用可能なビジュアライゼーションの完全なリストを表示します。
   * **[パネル](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/panels.html?lang=ja)** - パネルは、テーブルとビジュアライゼーションのコレクションです。 パネルには、ワークスペースの左上のアイコンからアクセスできます。パネルは、期間、レポートスイート、分析のユースケースに従ってプロジェクトを整理する場合に役立ちます。 Analysis Workspace では、以下のパネルタイプを使用できます。

   ![テンプレートを選択](assets/create-analytics-workspace/workspace-tools.png)

### Analysis Workspace でのデータビジュアライゼーションの追加

次に、ユーザーが WKND サイトのホームページの `Call to Action (CTA)` ボタンとやり取りする方法を視覚的に表現するテーブルを作成します。[クリックされたコンポーネントの Adobe Analytics による追跡](./track-clicked-component.md)で収集したデータを使用して、このような表現を作成しましょう。WKND サイトのコールトゥアクションボタンを使用したユーザーインタラクションについて追跡されたデータの概要を、以下に示します。

* `eVar5` - `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

1. **ページ**&#x200B;ディメンションコンポーネントをドラッグして、フリーフォームテーブルにドロップします。これで、ページ名（eVar9）と、対応するページビュー（回数）をテーブル内に表示するビジュアライゼーションを表示できるようになります。

   ![ページディメンション](assets/create-analytics-workspace/evar9-dimension.png)

1. **CTA クリック**（event8）指標をドラッグして発生回数指標の上にドロップし、置き換えます。これで、ページ名（eVar9）と、ページで対応する CTA クリックイベントを表示するビジュアライゼーションを表示できます。

   ![ページ指標 - CTA クリック](assets/create-analytics-workspace/evar8-cta-click.png)

1. ページをテンプレートタイプ別に分類してみましょう。コンポーネントからページテンプレート指標を選択し、ページテンプレート指標をドラッグしてページ名ディメンションにドロップします。 これで、ページ名がさらにテンプレートタイプ別に分類されて表示されるようになりました。

   * **前**
     ![eVar5](assets/create-analytics-workspace/evar5.png)

   * **後**
     ![eVar5 指標](assets/create-analytics-workspace/evar5-metrics.png)

1. WKND サイトページ上でユーザーが CTA ボタンをどのように操作するかを理解するには、ボタン ID（eVar8）指標を追加して、さらに詳細を分析する必要があります。

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. 以下に、WKND サイトをページテンプレート別に分類し、さらに WKND サイトのクリックトゥアクション（CTA）ボタンによるユーザーインタラクション別に分類した視覚的表現を示します。

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. Adobe Analytics Classifications を使用して、ボタン ID の値を、よりわかりやすい名前に置き換えることができます。 特定の指標の分類を作成する方法について詳しくは、 [こちら](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html?lang=ja)を参照してください。この例では、ボタン ID をユーザーにわかりやすい名前にマッピングする `eVar8` に対して分類指標 `Button Section (Button ID)` が設定されています。

   ![ボタンセクション](assets/create-analytics-workspace/button-section.png)

## 分析変数への分類の追加

### コンバージョンの分類

Analytics 分類を使用すると、Analytics 変数データを分類し、レポートの生成時に様々な方法でそのデータを表示することができます。 Analytics Workspace レポートでのボタン ID の表示形式を改善するには、ボタン ID（eVar8）の分類変数を作成します。 分類を行う場合、変数とその変数に関連するメタデータとの間の関係を明確にします。

次に、Analytics 変数の分類を作成します。

1. **管理者**&#x200B;ツールバーメニューで、「**レポートスイート**」を選択します。
1. **Report Suite Manager** ウィンドウで&#x200B;**レポートスイート ID** を選択し、**設定を編集**／**コンバージョン**／**コンバージョン分類**&#x200B;をクリックします。

   ![コンバージョン分類](assets/create-analytics-workspace/conversion-classification.png)

1. **分類タイプを選択**&#x200B;ドロップダウンリストから、変数（eVar8 ボタン ID）を選択して分類を追加します。
1. 「分類」セクションの下に表示される分類変数の横の矢印をクリックして、新しい分類を追加します。

   ![コンバージョン分類タイプ](assets/create-analytics-workspace/select-classification-variable.png)

1. **分類を編集**&#x200B;ダイアログボックスで、テキスト分類に適した名前を指定します。テキスト分類名が設定されたディメンションコンポーネントが作成されます。

   ![コンバージョン分類タイプ](assets/create-analytics-workspace/new-classification.png)

1. 変更を&#x200B;**保存**&#x200B;します。

### 分類インポーター

インポーターを使用して、分類を Adobe Analytics にアップロードします。 読み込みの前に、更新用にデータを書き出すこともできます。 読み込みツールを使用して読み込むデータは、特定の形式にする必要があります。 データテンプレートと、ヘッダーの適切な詳細情報がすべて格納されたタブ区切りのデータファイルをダウンロードできるようになっています。新しいデータをこのテンプレートに追加した後、FTP を使用してブラウザーでデータファイルを読み込むことができます。

#### 分類テンプレート

分類をマーケティングレポートに読み込む前に、テンプレートをダウンロードすると、分類データファイルの作成に役立ちます。データファイルでは、目的の分類を列見出しとして使用し、レポートデータセットを適切な分類見出しの下に整理します。

次に、ボタン ID（eVar8）変数の分類テンプレートをダウンロードします。

1. **管理者**／**分類インポーター**&#x200B;に移動します。
1. 「**テンプレートのダウンロード**」タブで、コンバージョン変数の分類テンプレートをダウンロードします。
   ![コンバージョン分類タイプ](assets/create-analytics-workspace/classification-importer.png)

1. 「テンプレートのダウンロード」タブで、データテンプレートの設定を指定します。
   * **レポートスイートの選択**：テンプレートで使用するレポートスイートを選択します。レポートスイートとデータセットが一致している必要があります。
   * **分類するデータセット**：データファイルのデータのタイプを選択します。メニューには、分類用に設定されたレポートスイート内のすべてのレポートが含まれます。
   * **エンコード**：データファイルの文字エンコードを選択します。デフォルトのエンコード形式は UTF-8 です。

1. 「**ダウンロード**」をクリックして、テンプレートファイルをローカルシステムに保存します。テンプレートファイルは、タブ区切り形式のデータファイル（.tab ファイル名の拡張子）で、ほとんどのスプレッドシートアプリケーションに対応しています。
1. 任意のエディターを使用して、タブ区切り形式のデータファイルを開きます。
1. セクションの手順 9 の各 eVar9 値に対して、ボタン ID（eVar9）と対応するボタン名をタブ区切り形式のファイルに追加します。

   ![キー値](assets/create-analytics-workspace/key-value.png)

1. タブ区切り形式のファイルを&#x200B;**保存**&#x200B;します。
1. 「**ファイルを読み込み**」タブに移動します。
1. ファイルの読み込み先を設定します。
   * **レポートスイートの選択**：WKND サイト AEM（レポートスイート）
   * **分類するデータセット**：ボタン ID（コンバージョン変数 eVar8）
1. 「**ファイルを選択**」オプションをクリックして、タブ区切り形式のファイルをシステムからアップロードし、「**ファイルを読み込み**」をクリックします。

   ![ファイルインポーター](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > 読み込みが正常に完了すると、書き出しに適切な変更が直ちに表示されます。ただし、ブラウザーの読み込みを使用する場合はレポートのデータ変更に最大 4 時間かかり、FTP の読み込みを使用する場合は最大 24 時間かかります。

#### コンバージョン変数を分類変数に置き換える

1. Analytics ツールバーで、**ワークスペース**&#x200B;を選択して、このチュートリアルの [Analysis Workspace でのプロジェクトの作成](#create-a-project-in-analysis-workspace)の節で作成したワークスペースを開きます。

   ![ワークスペースボタン ID](assets/create-analytics-workspace/workspace-report-button-id.png)

1. 次に、コールトゥアクション（CTA）ボタンの ID を表示するワークスペースで、**ボタン ID** 指標を前の手順で作成した分類名に置き換えます。

1. コンポーネントファインダーから **WKND CTA ボタン**&#x200B;を検索して、**WKND CTA ボタン（ボタン ID）**&#x200B;ディメンションをボタン ID 指標にドラッグ＆ドロップして置き換えます。

   * **前**
     ![前のワークスペースボタン](assets/create-analytics-workspace/wknd-button-before.png)
   * **後**
     ![後のワークスペースボタン](assets/create-analytics-workspace/wknd-button-after.png)

1. コールトゥアクション（CTA）ボタンのボタン ID を含むボタン ID 指標が、分類テンプレートで提供された対応する名前に置き換えられました。
1. Analytics Workspace テーブルと WKND ホームページを比較し、CTA ボタンのクリック数とその分析について説明します。ワークスペースのフリーフォームテーブルデータに基づき、ユーザーが「**SKI NOW**」ボタンを 22 回、WKND ホームページで西オーストラリアでのキャンプに関する「**Read More**」ボタンを 4 回クリックしたことが明確にわかります。

   ![CTA レポート](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. Adobe Analytics のワークスペースプロジェクトを保存し、適切な名前と説明を指定してください。必要に応じて、ワークスペースプロジェクトにタグを追加できます。

   ![プロジェクトを保存](assets/create-analytics-workspace/save-project.png)

1. プロジェクトが正常に保存されたら、「共有」オプションを使用して、ワークスペースプロジェクトを他の同僚やチームメイトと共有できます。

   ![プロジェクトを共有](assets/create-analytics-workspace/share.png)

## おめでとうございます。

Adobe Experience Manager サイトから取得したデータを、Adobe Analytics レポートスイートの指標やディメンションにマッピングする方法を学びました。また、指標の分類を実行し、Adobe Analytics の Analysis Workspace 機能を使用して詳細なレポートダッシュボードを作成しました。
