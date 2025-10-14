---
title: Adobe Experience Platformでのタグの統合
description: AEM as a Cloud ServiceをAdobe Experience Platformのタグと統合する方法について説明します。 この統合を使用すると、Adobe web SDKをデプロイし、データ収集およびパーソナライゼーションのためのカスタム JavaScriptをAEM ページに挿入できます。
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture, Content Management
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18719
thumbnail: null
source-git-commit: 70665c019f63df1e736292ad24c47624a3a80d49
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 3%

---


# Adobe Experience Platformでのタグの統合

AEM as a Cloud Service（AEMCS）をAdobe Experience Platformのタグと統合する方法を説明します。 タグ（Launch）統合を使用すると、Adobe web SDKをデプロイし、データ収集およびパーソナライゼーションのためのカスタム JavaScriptをAEM ページに挿入できます。

この統合により、マーケティングチームや開発チームは、AEM コードを再デプロイしなくても、パーソナライゼーションやデータ収集のためにJavaScriptを管理およびデプロイできます。

## 手順の概要

統合プロセスには、AEMと Tags の間のつながりを確立する 4 つの主な手順が含まれます。

1. **Adobe Experience Platformでタグプロパティを作成、設定および公開する**
2. **AEMのタグのAdobe IMS設定の確認**
3. **AEMでのタグ設定の作成**
4. **タグ設定をAEMページに適用する**

## Adobe Experience Platformでのタグプロパティの作成、設定および公開

まず、Adobe Experience Platformでタグプロパティを作成します。 このプロパティを使用すると、パーソナライゼーションやデータ収集に必要なAdobe web SDKやカスタム JavaScriptのデプロイメントを管理できます。

1. [Adobe Experience Platform](https://experience.adobe.com/platform) に移動し、Adobe IDでログインして、左側のメニューから **タグ** に移動します。\
   ![Adobe Experience Platform タグ &#x200B;](../assets/setup/aep-tags.png)

2. **新規プロパティ** をクリックして、新しいタグプロパティを作成します。\
   ![&#x200B; 新しいタグプロパティの作成 &#x200B;](../assets/setup/aep-create-tags-property.png)

3. **プロパティを作成** ダイアログで、以下を入力します。
   - **プロパティ名**：タグプロパティの名前
   - **プロパティタイプ**:**Web** を選択します
   - **ドメイン**：プロパティをデプロイするドメイン（例：`.adobeaemcloud.com`）

   「**保存**」をクリックします。

   ![Adobe タグプロパティ &#x200B;](../assets/setup/adobe-tags-property.png)

4. 新規プロパティを開きます。 **Core** 拡張機能は既に含まれている必要があります。 後で、**データストリーム ID** などの追加の設定が必要なので、実験のユースケースを設定する際に **Web SDK** 拡張機能を追加します。\
   ![Adobe タグコア拡張機能 &#x200B;](../assets/setup/adobe-tags-core-extension.png)

5. **公開フロー** に移動し、「ライブラリを追加」 **をクリックして、タグプロパティを公開し** デプロイメントライブラリを作成します。
   ![Adobe タグの公開フロー &#x200B;](../assets/setup/adobe-tags-publishing-flow.png)

6. **ライブラリを作成** ダイアログで、次の情報を指定します。
   - **名前**: ライブラリの名前
   - **環境**:「**開発**」を選択します
   - **リソースの変更**:「**変更されたすべてのリソースを追加**」を選択します

   **開発用に保存およびビルド** をクリックします。

   ![Adobe Tags Create Library](../assets/setup/adobe-tags-create-library.png)

7. ライブラリを実稼動環境に公開するには、「**承認して実稼動環境に公開**」をクリックします。 公開が完了すると、プロパティがAEMで使用できるようになります。\
   ![Adobe タグの承認と公開 &#x200B;](../assets/setup/adobe-tags-approve-publish.png)

## AEMでのタグのAdobe IMS設定の確認

AEMCS 環境がプロビジョニングされると、対応するAdobe Developer Console プロジェクトと共にタグのAdobe IMS設定が自動的に含まれます。 この設定により、AEMとタグの間で安全な API 通信が確保されます。

1. AEMで、**ツール**/**セキュリティ**/**Adobe IMS設定** に移動します。\
   ![Adobe IMS 設定](../assets/setup/aem-ims-configurations.png)

2. **Adobe Launch** 設定を見つけます。 使用可能な場合は、そのオプションを選択し、「**ヘルスチェック**」をクリックして接続を確認します。 成功の応答が表示されます。\
   ![Adobe IMS構成の正常性チェック &#x200B;](../assets/setup/aem-ims-configuration-health-check.png)

## AEMでのタグ設定の作成

AEMでタグ設定を作成し、サイトページに必要なプロパティと設定を指定します。

1. AEMで、**ツール**/**クラウドサービス**/**Adobe Launch 設定** に移動します。\
   ![Adobe Launch の設定 &#x200B;](../assets/setup/aem-launch-configurations.png)

2. サイトのルートフォルダー（WKND Site など）を選択し、「**作成**」をクリックします。\
   ![Adobe Launch 設定の作成 &#x200B;](../assets/setup/aem-create-launch-configuration.png)

3. ダイアログで、以下を入力します。
   - **タイトル**：例：「Adobe Tags」
   - **IMS 設定**：検証済みの **Adobe Launch** IMS 設定を選択します
   - **会社**：タグプロパティにリンクする会社を選択します
   - **プロパティ**：前に作成したタグプロパティを選択します

   「**次へ**」をクリックします。

   ![Adobe Launch 設定の詳細 &#x200B;](../assets/setup/aem-launch-configuration-details.png)

4. デモ目的で、**ステージング** 環境と **実稼動** 環境のデフォルト値を維持します。 「**作成**」をクリックします。\
   ![Adobe Launch 設定の詳細 &#x200B;](../assets/setup/aem-launch-configuration-create.png)

5. 新しく作成した設定を選択し、「**公開**」をクリックしてサイトページで使用できるようにします。\
   ![Adobe Launch 設定公開 &#x200B;](../assets/setup/aem-launch-configuration-publish.png)

## AEM サイトへのタグ設定の適用

Tags 設定を適用して、web SDKとパーソナライゼーションロジックをサイトページに挿入します。

1. AEMで、**Sites** に移動して、ルートサイトフォルダー（WKND Site など）を選択し、**プロパティ** をクリックします。\
   ![AEM サイトのプロパティ &#x200B;](../assets/setup/aem-site-properties.png)

2. **サイトのプロパティ** ダイアログで、「**詳細**」タブを開きます。 **設定** で、「`/conf/wknd` クラウド設定 **」に** が選択されていることを確認します。\
   ![AEM サイトの詳細プロパティ &#x200B;](../assets/setup/aem-site-advanced-properties.png)

## 統合の検証

タグの設定が正しく動作していることを確認するには、次の操作を実行します。

1. AEM パブリッシュページのビューソースを確認するか、ブラウザーのデベロッパーツールを使用して調べます。
2. [Adobe Experience Platform Debugger](https://chromewebstore.google.com/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) を使用して、Web SDKとJavaScriptのインジェクションを検証します

![Adobe Experience Platform Debugger](../assets/setup/aep-debugger.png)

## その他のリソース

- [Adobe Experience Platform Debugger の概要](https://experienceleague.adobe.com/ja/docs/experience-platform/debugger/home)
- [Tags の概要](https://experienceleague.adobe.com/ja/docs/experience-platform/tags/home)
