---
title: Analysis Workspaceでデータを分析する
description: Adobe Experience Managerサイトから収集したデータを、Adobe Analyticsのレポートスイートの指標およびディメンションにマップする方法について説明します。 Adobe AnalyticsのAnalysis Workspace機能を使用して、詳細なレポートダッシュボードを作成する方法を学びます。
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6409
thumbnail: KT-6296.jpg
translation-type: tm+mt
source-git-commit: 55beee99b91c44f96cd37d161bb3b4ffe38d2687
workflow-type: tm+mt
source-wordcount: '2202'
ht-degree: 1%

---


# Analysis Workspaceでデータを分析する

Adobe Experience Managerサイトから収集したデータを、Adobe Analyticsのレポートスイートの指標およびディメンションにマップする方法について説明します。 Adobe AnalyticsのAnalysis Workspace機能を使用して、詳細なレポートダッシュボードを作成する方法を学びます。

## 作成する内容

WKNDマーケティングチームは、ホームページで最もパフォーマンスの高い誘い文句(CTA)ボタンを把握したいと考えています。 このチュートリアルでは、Analysis Workspaceで新しいプロジェクトを作成し、様々なCTAボタンのパフォーマンスを視覚化して、サイトでのユーザの行動を把握します。 次の情報は、ユーザーがWKNDホームページのCall to Action(CTA)ボタンをクリックしたときに、Adobe Analyticsを使用して取得されます。

**Analytics変数**

以下に、現在追跡中のAnalytics変数を示します。

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

![CTAクリックAdobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### 目的 {#objective}

1. 新しいレポートスイートを作成するか、既存のレポートスイートを使用します。
1. レポートスイートで [コンバージョン変数(eVar)](https://docs.adobe.com/content/help/en/analytics/admin/admin-tools/conversion-variables/conversion-var-admin.html) および [成功イベント(イベント)](https://docs.adobe.com/help/en/analytics/admin/admin-tools/success-events/success-event.html) を設定します。
1. インサイトをすばやく作成、分析、共有できるツールの助けを借りて [、](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/home.html) Analysis Workspaceプロジェクトを作成し、データを分析します。
1. Analysis Workspaceプロジェクトを他のチームメンバーと共有します。

## 前提条件

このチュートリアルは、 [クリックを](./track-clicked-component.md) 追跡コンポーネントのAdobe Analyticsとの継続で、次の点が想定されています。

* **Adobe Analytics拡張機能が有効な** 起動プロパティ [](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html)
* **Adobe Analytics** test/devレポートスイートIDとトラッキングサーバー。 新しいレポートスイートを [作成するには、次のドキュメントを参照してください](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)。
* [https://wknd.site/us/en.htmlに読み込まれた起動プロパティを使用して設定されたExperience Platformデバッガ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) ーのブラウザー拡張機能、 [](https://wknd.site/us/en.html) またはAEMデータレイヤーが有効なAdobeサイト。

## コンバージョン変数(eVar)と成功イベント(イベント)

Custom Insightコンバージョン変数(またはeVar)は、サイトの選択したWebページのAdobeコードに配置されます。 その主な目的は、カスタムマーケティングレポートでコンバージョン成功指標をセグメント化することです。 eVarは訪問ベースにでき、cookieと同様に機能します。 eVar変数に渡される値は、ユーザーに従って所定の期間が経過します。

eVarを訪問者の値に設定すると、Adobeはその値を自動的に記憶し、期限切れになります。 eVar値がアクティブな間に訪問者が発生した成功イベントは、すべてeVar値にカウントされます。

eVarは、次のような原因と結果を測定するのに最適です。

* 売上高に影響を与えた内部キャンペーン
* 最終的に登録につながったバナー広告
* 注文前に内部検索が使用された回数

成功イベントとは、追跡できるアクションです。 成功イベントは何かを判断します。 例えば、訪問者がCTAボタンをクリックした場合、clickイベントは成功イベントと見なすことができます。

### eVarの設定

1. Adobe Experience Cloudホームページから組織を選択し、Adobe Analyticsを起動します。

   ![Analytics AEP](assets/create-analytics-workspace/analytics-aep.png)

1. Analyticsツールバーで、 **管理者** / **** レポートスイートの順にクリックし、レポートスイートを探します。

   ![Analyticsレポートスイート](assets/create-analytics-workspace/select-report-suite.png)

1. レポートスイート/設定を **編集** / **コンバージョン** /コンバージョン **変数を選択します。**

   ![Analyticsコンバージョン変数](assets/create-analytics-workspace/conversion-variables.png)

1. **** 追加新しいオプションを使用して、コンバージョン変数を作成し、スキーマを次のようにマッピングします。

   * `eVar5` -  `Page Template`
   * `eVar6` - `Page ID`
   * `eVar7` - `Last Modified Date`
   * `eVar8` - `Button Id`
   * `eVar9` - `Page Name`

   ![追加新しいeVar](assets/create-analytics-workspace/add-new-evars.png)

1. 各eVarに適切な名前と説明を入力し、変更を **保存します** 。 次の節では、これらのeVarを使用してAnalysis Workspaceプロジェクトを作成します。 したがって、わかりやすい名前を付けると、変数が見つけやすくなります。

   ![eVar](assets/create-analytics-workspace/evars.png)

### 成功イベントの設定

次に、CTAボタンのクリックを追跡する偶数を作成します。

1. 「 **Report Suite Manager** 」ウィンドウで「 **Report Suite Id** 」を選択し、「 **Edit Settings**」をクリックします。
1. コン **バージョン** / **成功イベントをクリックします。**
1. 「 **追加 New** 」オプションを使用して、新しいカスタム成功イベントを作成し、CTAボタンのクリックを追跡して、変更を **** 保存します。
   * `Event`：`event8`
   * `Name`:`CTA Click`
   * `Type`:`Counter`

   ![eVar](assets/create-analytics-workspace/add-success-event.png)

## Analysis Workspaceで新しいプロジェクトを作成 {#workspace-project}

Analysis Workspaceは柔軟なブラウザツールで、分析を作成し、インサイトを素早く共有できます。 ドラッグ&amp;ドロップインターフェイスを使用して、分析の作成、ビジュアライゼーションの追加を行い、データを有効に活用したり、データセットのキュレーション、組織内の任意のユーザーとの共有、プロジェクトのスケジュールを行うことができます。

次に、新しい [プロジェクトを作成し](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/build-workspace-project/t-freeform-project.html) 、サイト全体のCTAボタンのパフォーマンスを分析するダッシュボードを作成します。

1. Analyticsツールバーで、「 **Workspace** 」を選択し、をクリックして「 **新しいプロジェクトを作成**」を選択します。

   ![ワークスペース](assets/create-analytics-workspace/create-workspace.png)

1. **空のプロジェクトから開始を選択するか** 、Adobeが提供する事前設計テンプレートまたは組織が作成したカスタムテンプレートのいずれかを選択します。 使用する分析や用途に応じて、いくつかのテンプレートを使用できます。 [使用可能な様々なテンプレートオプションについて詳しく説明します](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html) 。

   Workspaceプロジェクトでは、パネル、テーブル、ビジュアライゼーションおよびコンポーネントに左側のレールからアクセスします。 これらはプロジェクトの構成要素です。

   * **[コンポーネント](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html)** — コンポーネントは、ディメンション、指標、セグメントまたは日付範囲で、すべてフリーフォームテーブルで組み合わせて、開始の質問に答えることができます。 分析に飛び込む前に、各コンポーネントのタイプについて理解しておいてください。 コンポーネントの用語を習得したら、ドラッグ&amp;ドロップを開始して、フリーフォームテーブルに分析を作成できます。
   * **[ビジュアライゼーション](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html)** — 棒グラフや折れ線グラフなどのビジュアライゼーションをデータの上に追加して、視覚的に使いやすくします。 左端のレールで、中央のビジュアライゼーションアイコンを選択して、使用可能なビジュアライゼーションの完全なリストを表示します。
   * **[パネル](https://docs.adobe.com/content/help/en/analytics/analyze/analysis-workspace/panels/panels.html)** — パネルは、テーブルとビジュアライゼーションの集まりです。 パネルには、ワークスペースの左上のアイコンからアクセスできます。 パネルは、期間、レポートスイート、分析の使用例に従ってプロジェクトを整理する場合に便利です。 Analysis Workspaceでは、次のパネルタイプを利用できます。

   ![テンプレートの選択](assets/create-analytics-workspace/workspace-tools.png)

### Analysis Workspaceでの追加データ可視化

次に、WKNDサイトホームページの誘い文句(CTA)ボタンとのユーザーのやり取りを視覚的に示す表を作成します。 このような表現を作成するには、 [Track clickedコンポーネントで収集されたデータをAdobe Analyticsと共に使用します](./track-clicked-component.md)。 以下に、WKNDサイトの「誘い文句（CTA：コールトゥアクション）」ボタンを使用したユーザーの操作に対して追跡されるデータの概要を簡単に示します。

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

1. フリーフォームテーブルに **ページ** ディメンションコンポーネントをドラッグ&amp;ドロップします。 これで、テーブル内に表示されるページ名(eVar9)と対応するページ表示（回数）を表示するビジュアライゼーションを表示できるはずです。

   ![ページDimension](assets/create-analytics-workspace/evar9-dimension.png)

1. 「 **CTA Click** (イベント8)」指標を回数指標にドラッグ&amp;ドロップし、それを置き換えます。 ページ名(eVar9)と、ページ上のCTAクリックイベントの対応する数を表示するビジュアライゼーションを表示できるようになりました。

   ![ページ指標 — CTAクリック](assets/create-analytics-workspace/evar8-cta-click.png)

1. テンプレートタイプ別にページを分類します。 コンポーネントからページテンプレート指標を選択し、「ページテンプレート」指標を「ページ名」ディメンションにドラッグ&amp;ドロップします。 これで、テンプレートタイプ別にページ名を表示できます。

   * **前**

      ![eVar5](assets/create-analytics-workspace/evar5.png)

   * **後**

      ![eVar5指標](assets/create-analytics-workspace/evar5-metrics.png)

1. WKNDサイトページ上でユーザーがCTAボタンとどのようにやり取りするかを理解するには、ボタンID(eVar8)指標を追加して、ページテンプレート指標をさらに分類する必要があります。

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. 以下に、WKNDサイトがページテンプレート別に視覚的に表示され、WKNDサイトのクリックしてアクション(CTA)ボタンとのユーザーインタラクション別にさらに分類されています。

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. 「ボタンID」の値を、Adobe Analytics分類を使用して、よりわかりやすい名前に置き換えることができます。 特定の指標の分類を作成する方法について詳しくは、 [ここを参照してください](https://docs.adobe.com/content/help/en/analytics/components/classifications/c-classifications.html)。 この場合、ボタンIDをユーザーにわかりやすい名前にマップする分類指標 `Button Section (Button ID)` を `eVar8` 設定しています。

   ![ボタンセクション](assets/create-analytics-workspace/button-section.png)

## 分析追加変数の分類

### コンバージョンの分類

Analytics分類は、Analytics変数データを分類し、レポートを生成する際に様々な方法でデータを表示する方法です。 Analytics WorkspaceレポートでのボタンIDの表示を改善するには、ボタンID(eVar8)の分類変数を作成します。 分類を行うときは、変数とその変数に関連するメタデータとの間に関係を確立します。

次に、Analytics変数の分類を作成します。

1. [ **管理** ]ツールバーメニューで[ **レポートスイート]を選択します。**
1. Report Suite Manager **(** Report Suite Manager **)ウィンドウで** Report Suite Id **（レポートスイートID）を選択し、設定の** 編集/コンバージョン/コンバージョン ******コンバージョンの分類の順にクリックします。**

   ![コンバージョンの分類](assets/create-analytics-workspace/conversion-classification.png)

1. 「分類タイプの **選択** 」ドロップダウンリストから、変数(eVar8ボタンID)を選択して分類を追加します。
1. 「分類」セクションの下に表示される分類変数の右にある矢印をクリックして、新しい分類を追加します。

   ![コンバージョンの分類タイプ](assets/create-analytics-workspace/select-classification-variable.png)

1. 分類の **編集ダイアログボックスで、「テキスト分類** 」に適切な名前を指定します。 テキスト分類名を持つディメンションコンポーネントが作成されます。

   ![コンバージョンの分類タイプ](assets/create-analytics-workspace/new-classification.png)

1. **変更を保存します。**

### 分類インポーター

インポーターを使用して、分類をAdobe Analyticsにアップロードします。 また、インポートの前に、更新用にデータをエクスポートすることもできます。 読み込みツールを使用して読み込むデータは、特定の形式にする必要があります。 Adobeには、すべての適切なヘッダーの詳細を含むデータテンプレートをタブ区切りのデータファイルとしてダウンロードするオプションが用意されています。 このテンプレートに新しいデータを追加し、FTPを使用してブラウザーにデータファイルをインポートできます。

#### 分類テンプレート

分類をマーケティングレポートにインポートする前に、分類データファイルの作成に役立つテンプレートをダウンロードできます。 データファイルでは、目的の分類を列見出しとして使用し、レポートデータセットを適切な分類見出しの下に整理します。

次に、ボタンID(eVar8)変数の分類テンプレートをダウンロードします。

1. **管理者** / **分類インポーターに移動します。**
1. コンバージョン変数の分類テンプレートを[テンプレートの **ダウンロード** ]タブからダウンロードします。
   ![コンバージョンの分類タイプ](assets/create-analytics-workspace/classification-importer.png)

1. 「テンプレートのダウンロード」タブで、データテンプレートの設定を指定します。
   * **レポートスイートの選択** :テンプレートで使用するレポートスイートを選択します。 レポートスイートとデータセットが一致する必要があります。
   * **分類するデータセット** :データファイルのデータの種類を選択します。 このメニューには、分類用に設定されたレポートスイート内のすべてのレポートが含まれます。
   * **エンコーディング** :データファイルの文字エンコーディングを選択します。 デフォルトのエンコーディング形式はUTF-8です。

1. 「 **ダウンロード** 」をクリックし、テンプレートファイルをローカルシステムに保存します。 テンプレートファイルはタブ区切りのデータファイル（ファイル名の拡張子は.tab）で、ほとんどのスプレッドシートアプリケーションでサポートされています。
1. 任意のエディターを使用して、タブ区切りデータファイルを開きます。
1. セ追加クションの手順9の各eVar9値のボタンID(eVar9)とタブ区切りファイルに対応するボタン名。

   ![キー値](assets/create-analytics-workspace/key-value.png)

1. **タブ区切りファイルを保存します** 。
1. 「 **ファイルのインポート** 」タブに移動します。
1. ファイルのインポート先を設定します。
   * **レポートスイートの選択** :WKNDサイトAEM（レポートスイート）
   * **分類するデータセット** :ボタンId(コンバージョン変数eVar8)
1. 「ファイルを **選択** 」オプションをクリックして、タブ区切りファイルをシステムからアップロードし、「ファイルの **読み込み」をクリックします**

   ![ファイルインポーター](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > インポートが正常に完了すると、エクスポートで適切な変更がすぐに表示されます。 ただし、ブラウザーでインポートした場合、レポート内のデータ変更には最大4時間かかり、FTPインポートでは最大24時間かかります。

#### コンバージョン変数を分類変数に置き換える

1. Analyticsツールバーで「 **Workspace** 」を選択し、このチュートリアルの「Analysis Workspaceで新しいプロジェクトを [作成する](#workspace-project) 」セクションで作成したワークスペースを開きます。

   ![ワークスペースボタンID](assets/create-analytics-workspace/workspace-report-button-id.png)

1. 次に、アクションの呼び出し(CTA)ボタンのIDを表示するワークスペースの **ボタンID** 指標を、前の手順で作成した分類名に置き換えます。

1. コンポーネントファインダーで、 **WKND CTA Buttons** を検索し、 **WKND CTA Buttons(Button Id)** ディメンションをButton Id指標にドラッグ&amp;ドロップして置き換えます。

   * **前**

      ![前のワークスペースボタン](assets/create-analytics-workspace/wknd-button-before.png)
   * **後**

      ![後ろのワークスペースボタン](assets/create-analytics-workspace/wknd-button-after.png)

1. 「行動喚起」(CTA)ボタンのボタンIDを含むボタンID指標が、分類テンプレートで提供された対応する名前に置き換えられました。
1. Analytics WorkspaceテーブルとWKNDホームページを比較し、CTAボタンクリック数とその分析を把握します。 ワークスペースのフリーフォームテーブルデータに基づいて、22回のユーザーが「 **SKI NOW** 」ボタンをクリックし、4回のWKNDホームページキャンピングが「 **詳細を** 読む」ボタンをクリックしたことが明らかです。

   ![CTAレポート](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. 必ず、Adobe Analyticsワークスペースプロジェクトを保存し、正しい名前と説明を入力してください。 必要に応じて、ワークスペースプロジェクトにタグを追加できます。

   ![プロジェクトを保存](assets/create-analytics-workspace/save-project.png)

1. プロジェクトが正常に保存されたら、「共有」オプションを使用して、ワークスペースプロジェクトを他の同僚やチームメートと共有できます。

   ![プロジェクトを共有](assets/create-analytics-workspace/share.png)

## バリデーターが

ここでは、Adobe Experience Managerサイトから収集したデータをAdobe Analyticsのレポートスイートの指標やディメンションにマッピングし、指標の分類を実行し、Adobe AnalyticsのAnalysis Workspace機能を使用して詳細なレポートダッシュボードを作成する方法を学びました。

