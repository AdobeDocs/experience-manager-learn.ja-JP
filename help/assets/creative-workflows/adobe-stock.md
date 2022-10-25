---
title: AEM AssetsでのAdobe Stock Assets の使用
description: AEMを使用すると、Adobe StockアセットをAEMから直接検索、プレビュー、保存、ライセンスを取得できます。 Adobe Stock Enterprise プランとAEM Assetsを統合して、AEMの強力なアセット管理機能を使用して、ライセンスが必要なアセットをクリエイティブプロジェクトやマーケティングプロジェクトに幅広く活用できるようになりました。
feature: Adobe Stock
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-26T00:00:00Z
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
source-git-commit: f37483f90f2a707c906e1e206795fdebb5f698e9
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 12%

---

# AEM AssetsでのAdobe Stockの使用{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 では、ユーザーは、AEMから直接Adobe Stockアセットの検索、プレビュー、保存およびライセンスを取得できます。 Adobe Stock Enterprise プランとAEM Assetsを統合して、AEMの強力なアセット管理機能を使用して、ライセンスが必要なアセットをクリエイティブプロジェクトやマーケティングプロジェクトに幅広く活用できるようになりました。

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=12&learn=on)

>[!NOTE]
>
>この統合を利用するには、[Adobe Stock エンタープライズプラン](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)と、最新の Service Pack 2 を展開した AEM 6.4 が必要です。AEM 6.4 サービスパックについて詳しくは、[リリースノート](https://helpx.adobe.com/jp/experience-manager/6-4/release-notes/sp-release-notes.html)を参照してください。

Adobe StockとAEM Assetsの統合により、コンテンツ作成者とマーケターは、クリエイティブやマーケティングを目的として、Stock アセットのライセンスを簡単に取得し、使用することができます。 オムニサーチを使用し、場所フィルターをAdobe Stockとして追加するか、AEM Assetsのメインナビゲーションを移動してAdobe Stock Coral UI を検索アイコンをクリックすることで、Stock アセット検索を実行できます。

## 機能

### 検索と保存

* AEM Workspace を離れることなく、Adobe Stockのアセット検索を実行する。
* Adobe Stockのアセットをプレビュー用に保存します（アセットのライセンスを取得しません）。
* Adobe Stock Assets のライセンスを取得してAEM Assetsに保存する機能
* AEM Assets UI 内でAdobe Stockから類似のアセットを検索する機能
* Adobe Stock Web サイトのAEM Assets内での Stock 検索から選択したアセットを表示します。
* ライセンスが必要なアセットファイルには、識別しやすい青いライセンス済みバッジが付きます

### アセットメタデータ

* ライセンスが必要なアセットはAEM Assetsに保存されます。 アセットのプロパティには、別個のアセットメタデータタブの下にある Stock メタデータが含まれます
* アセットのメタデータにライセンス参照を追加する機能

### アセット在庫プロファイル

* ユーザーは、以下でAdobe Stockプロファイルを選択できます。 *ユーザー/環境設定/Stock 設定*
* Asset Licensing ウィンドウに、必須の参照とオプションの参照を追加できます。
* 地域に基づいて、アセットライセンスウィンドウの言語設定を選択できます。

### フィルター

* ユーザーは、アセットタイプ、回転角度、類似表示に基づいて在庫アセットをフィルタリングできます
* アセットタイプには、写真、イラスト、ベクトル、ビデオ、テンプレート、3D、プレミアム、編集用が含まれます
* [ 方向 ] には、[ 水平 ]、[ 垂直 ]、[ 四角形 ] が含まれます。
* 類似したフィルターを表示するには、Adobe Stockファイル番号が必要です

### アクセス制御

* 管理者は、Adobe Stock Cloud Service の設定時に、Stock アセットのライセンスを取得する権限を特定のユーザーやグループに与えることができます。
* 特定のユーザーまたはグループが Stock アセットのライセンスを取得する権限を持っていない場合、 *Stock アセット検索/アセットライセンス* の機能が無効になります。

## AEM AssetsでのAdobe Stockの設定{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 では、ユーザーは、AEMから直接Adobe Stockアセットの検索、プレビュー、保存およびライセンスを取得できます。 このビデオでは、Adobe I/Oコンソールを使用してAEM AssetsでAdobe在庫を設定する方法の簡単な説明を示します。

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>Adobe Stock Cloud Service 設定の場合、実稼動環境とライセンスが必要なアセットのパスを選択して、 `/content/dam`. AEMで環境フィールドが削除されました。

>[!NOTE]
>
>この統合を利用するには、[Adobe Stock エンタープライズプラン](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)と、最新の Service Pack 2 を展開した AEM 6.4 が必要です。[](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aem%2F6-4&amp;3_group.propertyvalues.property=.property=%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fdc%3Atitleorderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24)AEM 6.4 サービスパックについて詳しくは、[リリースノート](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)を参照してください。また、次の操作を行うためには管理者権限も必要です。 [Adobe I/Oコンソール](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) とAdobe Experience Managerを使用して統合を設定します。

### インストール {#installations}

* AEM 6.4 では、 [AEM Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aem%2F6-4&amp;3_group.propertyvalues.property=.property=%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fdc%3Atitleorderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) 次に、cq-dam-stock-integration-content-1.0.4.zip ファイルを再インストールします。
* に対する管理者権限があることを確認します。 [Adobe I/Oコンソール](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) とAdobe Experience Managerを使用して統合を設定します。

#### Adobe IMSコンソールを使用したAdobe I/O設定 {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. でAdobe IMSテクニカルアカウント設定を作成する **ツール/セキュリティ**
2. を選択します。 *クラウドソリューション* as *Adobe Stock* 新しい証明書を作成するか、既存の証明書を設定に再使用します。
3. Adobe I/Oコンソールに移動し、次の新しいサービスアカウント統合を作成します。 *Adobe Stock*.
4. 手順 2 からAdobe Stock Service アカウント統合に証明書をアップロードします。
5. 必要なAdobe Stockプロファイル設定を選択し、サービス統合を完了します。
6. 統合の詳細を使用して、統合テクニカルアカウントのAdobe IMSを完了します
7. テクニカルアカウントでアクセストークンを受け取れることをAdobe IMSにします。

![Adobe IMS テクニカルアカウント](assets/screen_shot_2018-10-22at12219pm.png)

#### Adobe StockCloud Servicesの設定 {#set-up-adobe-stock-cloud-services}

1. 以下にAdobe Stock用の新しいクラウドサービス設定を作成します。 **ツール/Cloud Services**
2. を選択します。 *Adobe IMS設定* 上記のセクションで、 *Adobe Stock Cloud* 設定

3. 必ず **環境** PROD.
4. **ライセンス済みアセットのパス** は以下の任意のディレクトリを指すことができます `/content/dam`.
5. ロケールを選択し、設定を完了します。
6. また、Adobe Stock Cloud Service にユーザー/グループを追加して、特定のユーザーまたはグループに対するアクセスを有効にすることもできます。

![AdobeAssets Stock 設定](assets/screen_shot_2018-10-22at12425pm.png)

### その他のリソース

* [エンタープライズストックプラン](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [AEM 6.4 Service Pack 2 リリースノート](https://experienceleague.adobe.com/docs/experience-manager-64/release-notes/sp-release-notes.html?lang=ja)
* [AEM と Adobe Stock の統合](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html)
* [Adobe I/Oコンソール統合 API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock API ドキュメント](https://www.adobe.io/apis/creativecloud/stock/docs.html)
