---
title: AEM AssetsでのAdobe Stock Assetsの使用
description: 'AEMでは、Adobe StockアセットをAEMから直接検索、プレビュー、保存、ライセンスを取得できます。 Adobe Stock EnterpriseプランとAEM Assetsを統合して、AEMの強力なアセット管理機能を使用して、ライセンスが必要なアセットをクリエイティブプロジェクトやマーケティングプロジェクトに幅広く活用できるようになりました。 '
feature: Adobe Stock
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1037'
ht-degree: 10%

---


# AEM AssetsでのAdobe Stockの使用{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2では、Adobe StockアセットをAEMから直接検索、プレビュー、保存およびライセンスを取得できます。 Adobe Stock EnterpriseプランとAEM Assetsを統合して、AEMの強力なアセット管理機能を使用して、ライセンスが必要なアセットをクリエイティブプロジェクトやマーケティングプロジェクトに幅広く活用できるようになりました。

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=12&learn=on)

>[!NOTE]
>
>この統合を利用するには、[Adobe Stock エンタープライズプラン](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)と、最新の Service Pack 2 を展開した AEM 6.4 が必要です。AEM 6.4 サービスパックについて詳しくは、[リリースノート](https://helpx.adobe.com/jp/experience-manager/6-4/release-notes/sp-release-notes.html)を参照してください。

Adobe StockとAEM Assetsの統合により、コンテンツ作成者やマーケターは、クリエイティブやマーケティングの目的で、Stockアセットのライセンスを容易に取得して使用できます。 オムニサーチを使用してStockアセット検索を実行するには、場所フィルターをAdobe Stockとして追加するか、AEM AssetsのメインナビゲーションからAdobe Stock Coral UIを検索アイコンをクリックします。

## 機能

### 検索と保存

* AEM Workspaceを離れることなく、Adobe Stockのアセット検索を実行する。
* Adobe Stockアセットをプレビュー用に保存し、アセットのライセンスを取得しない。
* Adobe Stockアセットのライセンスを取得してAEM Assetsに保存する機能
* AEM Assets UI内でAdobe Stockから類似のアセットを検索する機能
* Adobe Stock Webサイト上のAEM Assets内のStock検索で選択したアセットを表示する
* ライセンスが必要なアセットファイルには、識別しやすい青いライセンスのバッジが付いています

### アセットメタデータ

* ライセンスが必要なアセットはAEM Assetsに保存されます。 アセットのプロパティには、別個のアセットメタデータタブにあるStockメタデータが含まれます
* アセットメタデータにライセンス参照を追加する機能

### Asset Stockプロファイル

* ユーザーは、*User/My Preferences/Stock Configuration*&#x200B;でAdobe Stockプロファイルを選択できます。
* 必須参照とオプション参照をアセットライセンスウィンドウに追加できます。
* 地域に基づいて、アセットライセンスウィンドウの言語設定を選択できます。

### フィルター

* ユーザーは、アセットタイプ、向きおよび類似表示に基づいて在庫アセットをフィルタリングできます
* アセットタイプには、写真、イラスト、ベクトル、ビデオ、テンプレート、3D、プレミアム、編集が含まれます。
* 方向には、水平、垂直、四角形が含まれます。
* 表示類似フィルターにはAdobe Stockファイル番号が必要です

### アクセス制御

* 管理者は、Adobe Stock Cloud Serviceの設定時に、Stockアセットのライセンスを取得する権限を特定のユーザー/グループに付与できます。
* 特定のユーザー/グループにStockアセットのライセンス権限がない場合、*Stockアセットの検索/アセットのライセンス*&#x200B;機能が無効になります。

## AEM AssetsでのAdobe Stockの設定{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2では、Adobe StockアセットをAEMから直接検索、プレビュー、保存およびライセンスを取得できます。 このビデオでは、Adobe I/Oコンソールを使用してAEM AssetsでAdobe在庫を設定する方法の概要を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>Adobe Stock Cloudサービスの設定の場合、「PROD環境」と「ライセンスが必要」のアセットパスを選択し、/content/damを指す必要があります。 次回のAEMリリースでは環境フィールドが削除され、ライセンスが必要なアセットパスは今後の機能の一部となり、このフィールドのサポートは次回のAEMリリースで導入されます。

>[!NOTE]
>
>この統合を利用するには、[Adobe Stock エンタープライズプラン](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)と、最新の Service Pack 2 を展開した AEM 6.4 が必要です。[](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aem%2F6-4&amp;3_group.propertyvalues.property.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fdc%3Atitleorderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24)AEM 6.4 サービスパックについて詳しくは、[リリースノート](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)を参照してください。また、[Adobe I/Oコンソール](https://console.adobe.io/)、[Adobe Admin Console](https://adminconsole.adobe.com/)およびAdobe Experience Managerに対する管理者権限も必要です。

### インストール {#installations}

* AEM 6.4の場合は、[AEM Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aem%2F6-4&amp;3_group.propertyvalues.property.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fdc%3Atitleorderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24)をインストールしてから、cq-dam-stock-integration-content-1.0.4.zipファイルを再インストールする必要があります。
* [Adobe I/Oコンソール](https://console.adobe.io/)、[Adobe Admin Console](https://adminconsole.adobe.com/)およびAdobe Experience Managerの管理者権限を持っていることを確認して、統合を設定します。

#### Adobeコンソールを使用したAdobe I/OIMS設定 {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. **ツール/セキュリティ**&#x200B;でAdobeIMSテクニカルアカウント設定を作成します。
2. *Cloud Solution*&#x200B;として&#x200B;*Adobe Stock*&#x200B;を選択し、新しい証明書を作成するか、既存の証明書を設定に再使用します。
3. Adobe I/Oコンソールに移動し、*Adobe Stock*&#x200B;の新しいサービスアカウント統合を作成します。
4. 手順2からAdobe Stockサービスアカウント統合に証明書をアップロードします。
5. 必要なAdobe Stockプロファイル設定を選択し、サービス統合を完了します。
6. 統合の詳細を使用して、AdobeIMSテクニカルアカウント設定を完了します。
7. IMSテクニカルアカウントを使用してアクセストークンをAdobeで受け取れることを確認します。

![Adobe IMS テクニカルアカウント](assets/screen_shot_2018-10-22at12219pm.png)

#### Adobe StockCloud Servicesの設定 {#set-up-adobe-stock-cloud-services}

1. **ツール/Cloud ServicesでAdobe Stockの新しいクラウドサービス設定を作成します。**
2. *Adobe Stock Cloud*&#x200B;設定に対して、上記の節で作成した&#x200B;*AdobeIMS設定*&#x200B;を選択します。

3. 必ず&#x200B;**環境**&#x200B;をPRODとして選択してください。 ステージング環境はサポートされておらず、次回のリリースのAEMで削除されます。
4. **ライセンスが必要なアセットパス** は、/content/damの下の任意のディレクトリを指すことができます。このフィールドの機能のサポートは、次回のAEMリリースで追加されます
5. ロケールを選択し、設定を完了します。
6. また、Adobe Stock Cloudサービスにユーザー/グループを追加して、特定のユーザーまたはグループのアクセスを有効にすることもできます。

![Adobeアセットの在庫設定](assets/screen_shot_2018-10-22at12425pm.png)

### その他のリソース

* [エンタープライズストックプラン](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [AEM 6.4 Service Pack 2リリースノート](https://experienceleague.adobe.com/docs/experience-manager-64/release-notes/sp-release-notes.html?lang=en)
* [AEM と Adobe Stock の統合](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html)
* [Adobe I/Oコンソール統合API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock APIドキュメント](https://www.adobe.io/apis/creativecloud/stock/docs.html)