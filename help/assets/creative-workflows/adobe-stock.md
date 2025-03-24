---
title: AEM Assets での Adobe Stock アセットの使用
description: AEM は、Adobe Stock アセットを AEM から直接検索、プレビュー、保存およびライセンス供与する機能をユーザーに提供します。組織は、Adobe Stock エンタープライズプランを AEM Assets と統合して、AEM の強力なアセット管理機能により、ライセンスされたアセットをクリエイティブおよびマーケティングプロジェクトで広く利用できるようになりました。
feature: Adobe Stock
version: Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-26T00:00:00Z
doc-type: Feature Video
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
duration: 1079
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 100%

---

# AEM Assets での Adobe Stock の使用{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 では、Adobe Stock アセットを AEM から直接検索、プレビュー、保存およびライセンス供与する機能がユーザーに提供されます。組織は、Adobe Stock エンタープライズプランを AEM Assets と統合して、AEM の強力なアセット管理機能により、ライセンスされたアセットをクリエイティブおよびマーケティングプロジェクトで広く利用できるようになりました。

>[!VIDEO](https://video.tv.adobe.com/v/24678?quality=12&learn=on)

>[!NOTE]
>
>この統合を利用するには、[Adobe Stock エンタープライズプラン](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)と、最新のサービスパック 2 をデプロイした AEM 6.4 が必要です。AEM 6.4 サービスパックについて詳しくは、[リリースノート](https://helpx.adobe.com/jp/experience-manager/6-4/release-notes/sp-release-notes.html)を参照してください。

Adobe Stock と AEM Assets の統合により、コンテンツ作成者とマーケターは、クリエイティブまたはマーケティングの目的で、簡単にストックアセットのライセンスを取得して使用できるようになります。オムニサーチを使用して、場所フィルターを Adobe Stock として追加するか、AEM Assets のメインナビゲーションをナビゲートして Adobe Stock Coral UI を検索アイコンをクリックすることにより、ストックアセット検索を実行できます。

## 機能

### 検索と保存

* AEM ワークスペースを離れることなく、Adobe Stock アセットの検索を実行します。
* アセットのライセンスを取得することなく、Adobe Stock アセットをプレビュー用に保存します。
* Adobe Stock アセットのライセンスを取得して AEM Assets に保存する機能。
* AEM Assets UI 内で Adobe Stock から類似のアセットを検索する機能。
* Adobe Stock web サイトの AEM Assets 内での Stock 検索から選択したアセットを表示します。
* ライセンス済みアセットファイルには、識別しやすい青いライセンス済みバッジが付きます。

### アセットメタデータ

* ライセンス済みアセットは AEM Assets に保存されます。アセットのプロパティには、別個のアセットメタデータタブの下にある Stock メタデータが含まれます。
* アセットのメタデータにライセンス参照を追加する機能。

### アセット Stock プロファイル

* ユーザーは、*ユーザー／環境設定／Stock 設定*&#x200B;で Adobe Stock プロファイルを選択できます。
* アセットライセンスウィンドウに、必須の参照とオプションの参照を追加できます。
* 地域に基づいて、アセットライセンスウィンドウの言語設定を選択できます。

### フィルター

* ユーザーは、アセットタイプ、向き、類似表示に基づいてストックアセットをフィルタリングできます。
* アセットタイプには、写真、イラスト、ベクトル、ビデオ、テンプレート、3D、プレミアム、エディトリアルが含まれます。
* 向きには、水平、垂直、正方形が含まれます。
* 類似したフィルターを表示するには、Adobe Stock ファイル番号が必要です。

### アクセス制御

* 管理者は、Adobe Stock クラウドサービス設定時に、ストックアセットのライセンスを取得する権限を特定のユーザーやグループに与えることができます。
* 特定のユーザーまたはグループがストックアセットのライセンスを取得する権限を持っていない場合、*ストックアセット検索／アセットライセンス*&#x200B;の機能が無効になります。

## AEM Assets で Adobe Stock を設定{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 では、Adobe Stock アセットを AEM から直接検索、プレビュー、保存およびライセンス供与する機能がユーザーに提供されます。このビデオでは、Adobe I/O Console を使用して AEM Assets で Adobe Stock を設定する方法の簡単な説明を示します。

>[!VIDEO](https://video.tv.adobe.com/v/25043?quality=12&learn=on)

>[!NOTE]
>
>Adobe Stock クラウドサービス設定の場合、実稼働環境を選択し、ライセンス済みのパスが `/content/dam` を指している必要があります。AEM で環境フィールドが削除されました。

>[!NOTE]
>
>この統合を利用するには、[Adobe Stock エンタープライズプラン](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)と、最新の[サービスパック 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) を展開した AEM 6.4 が必要です。AEM 6.4 サービスパックについて詳しくは、[リリースノート](https://helpx.adobe.com/jp/experience-manager/6-4/release-notes/sp-release-notes.html)を参照してください。統合を設定するには、[Adobe I/O Console](https://console.adobe.io/)、[Adobe Admin Console](https://adminconsole.adobe.com/) および Adobe Experience Manager に対する管理者権限も必要です。

### インストール {#installations}

* AEM 6.4 の場合、[AEM サービスパック 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) をインストールしてから、cq-dam-stock-integration-content-1.0.4.zip ファイルを再インストールする必要があります。
* 統合を設定するには、[Adobe I/O Console](https://console.adobe.io/)、[Adobe Admin Console](https://adminconsole.adobe.com/) および Adobe Experience Manager に対する管理者権限があることを確認してください。

#### Adobe I/O Console を使用して Adobe IMS 設定を行う {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. **ツール／セキュリティ**&#x200B;で Adobe IMS テクニカルアカウント設定を作成します。
2. *クラウドソリューション*&#x200B;を *Adobe Stock* として選択し、新しい証明書を作成するか、設定に既存の証明書を再利用します。
3. Adobe I/O コンソールに移動し、*Adobe Stock* の新しいサービスアカウント統合を作成します。
4. 手順 2 の証明書を Adobe Stock サービスアカウント統合にアップロードします。
5. 必要な Adobe Stock プロファイル設定を選択し、サービス統合を完了します。
6. 統合の詳細を使用して、Adobe IMS テクニカルアカウントの設定を完了します
7. Adobe IMS テクニカルアカウントを使用してアクセストークンを受信できることを確認します。

![Adobe IMS テクニカルアカウント](assets/screen_shot_2018-10-22at12219pm.png)

#### Adobe Stock クラウドサービスの設定 {#set-up-adobe-stock-cloud-services}

1. **ツール／クラウドサービス**&#x200B;の下に、Adobe Stock 用の新しいクラウドサービス設定を作成します。
2. *Adobe Stock クラウド*&#x200B;を設定するために、上記のセクションで作成した *Adobe IMS 設定*&#x200B;を選択します。

3. 必ず「**環境**」に「実稼動環境」を選択します。
4. **ライセンス済みアセットのパス**&#x200B;は、`/content/dam` の下の任意のディレクトリを指すことができます。
5. ロケールを選択し、設定を完了します。
6. Adobe Stock クラウドサービスにユーザー／グループを追加して、特定のユーザーまたはグループに対するアクセスを有効にすることもできます。

![Adobe Assets Stock 設定](assets/screen_shot_2018-10-22at12425pm.png)

### その他のリソース

* [Stock エンタープライズプラン](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [AEM 6.4 サービスパック 2 リリースノート](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/release-notes.html?lang=ja)
* [AEM と Adobe Stock の統合](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html?lang=ja)
* [Adobe I/O Console Integration API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock API ドキュメント](https://www.adobe.io/apis/creativecloud/stock/docs.html)
