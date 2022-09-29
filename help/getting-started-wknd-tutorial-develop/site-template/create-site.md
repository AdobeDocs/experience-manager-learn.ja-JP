---
title: サイトの作成 | AEMクイックサイト作成
description: サイト作成ウィザードを使用して新しい Web サイトを生成する方法を説明します。 Adobeが提供する標準サイトテンプレートは、新しいサイトの出発点となります。
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 5%

---

# サイトの作成 {#create-site}

クイックサイト作成の一環として、AEMのAdobe Experience Managerのサイト作成ウィザードを使用して、新しい Web サイトを生成します。 Adobeが提供する標準サイトテンプレートは、新しいサイトの出発点として使用されます。

## 前提条件 {#prerequisites}

この章の手順は、Adobe Experience Manager as a Cloud Service環境で実行します。 AEM環境への管理者アクセス権があることを確認します。 次を使用することをお勧めします。 [サンドボックスプログラム](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) および [開発環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html?lang=ja) このチュートリアルを完了する際

以下を確認します。 [オンボーディングドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=ja) を参照してください。

## 目的 {#objective}

1. サイト作成ウィザードを使用して新しいサイトを生成する方法を説明します。
1. サイトテンプレートの役割を理解します。
1. 生成されたAEMサイトを参照します。

## Adobe Experience Manager Author にログインします。 {#author}

最初の手順として、AEMas a Cloud Service環境にログインします。 AEM環境は **オーサーサービス** および **パブリッシュサービス**.

* **オーサーサービス** ：サイトコンテンツが作成、管理、更新される場所。 通常、内部ユーザーのみが **オーサーサービス** とは、ログイン画面の後ろに表示されています。
* **パブリッシュサービス** ：ライブ Web サイトをホストします。 これは、エンドユーザーに表示されるサービスで、通常は一般に利用可能です。

チュートリアルの大部分は、 **オーサーサービス**.

1. Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). 個人用アカウント、または会社/学校のアカウントを使用してログインします。
1. メニューで正しい組織が選択されていることを確認し、 **Experience Manager**.

   ![Experience Cloudホーム](assets/create-site/experience-cloud-home-screen.png)

1. の下 **Cloud Manager** クリック **起動**.
1. 使用するプログラムの上にマウスポインターを置き、 **Cloud Manager プログラム** アイコン

   ![Cloud Manager プログラムアイコン](assets/create-site/cloud-manager-program-icon.png)

1. 上部のメニューで、 **環境** プロビジョニングされた環境を表示するには：

1. 使用する環境を見つけ、 **作成者 URL**.

   ![開発者にアクセス](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >次を使用することをお勧めします。 **開発** 環境を参照してください。

1. AEMに対して新しいタブが起動されました **オーサーサービス**. クリック **Adobe** 同じログイン資格情報で自動的にログインする必要がありますExperience Cloud。

1. リダイレクトされて認証された後に、AEMの開始画面が表示されます。

   ![AEM Start 画面](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Experience Managerにアクセスできない場合 以下を確認します。 [オンボーディングドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## 基本サイトテンプレートのダウンロード

サイトテンプレートは、新しいサイトの出発点となります。 サイトテンプレートには、いくつかの基本的なテーマ、ページテンプレート、設定、サンプルコンテンツが含まれています。 サイトテンプレートに含まれる内容は、開発者が自由に使用できます。 Adobeが **基本サイトテンプレート** を使用して、新しい実装を迅速におこなうことができます。

1. 新しいブラウザータブを開き、GitHub の基本サイトテンプレートプロジェクトに移動します。 [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). このプロジェクトはオープンソースで、誰でも使用できるライセンスを持っています。
1. クリック **リリース** をクリックし、 [最新リリース](https://github.com/adobe/aem-site-template-standard/releases/latest).
1. を展開します。 **Assets** ドロップダウンし、テンプレート zip ファイルをダウンロードします。

   ![基本サイトテンプレート Zip](assets/create-site/template-basic-zip-file.png)

   この zip ファイルは、次の演習で使用します。

   >[!NOTE]
   >
   > このチュートリアルは、バージョンを使用して記述します **1.1.0** 」をクリックします。 実稼動用に新しいプロジェクトを開始する場合は、常に最新バージョンを使用することをお勧めします。

## 新しいサイトを作成

次に、前の演習の [ サイトテンプレート ] を使用して新しいサイトを生成します。

1. AEM環境に戻ります。 AEM Start 画面からに移動します。 **サイト**.
1. 右上隅で、 **作成** > **サイト（テンプレート）**. これで、 **サイト作成ウィザード**.
1. の下 **サイトテンプレートを選択** クリック **インポート** 」ボタンをクリックします。

   をアップロードします。 **.zip** 前の演習でダウンロードしたテンプレートファイル。

1. を選択します。 **基本AEMサイトテンプレート** をクリックし、 **次へ**.

   ![サイトテンプレートを選択](assets/create-site/select-site-template.png)

1. の下 **サイトの詳細** > **サイトのタイトル** 入力 `WKND Site`.

   実際の実装では、「WKND サイト」は会社または組織のブランド名に置き換えられます。 このチュートリアルでは、気まぐれなライフスタイルブランド「WKND」のサイト作成のシミュレーションを行います。

1. の下 **サイト名** 入力 `wknd`.

   ![サイトテンプレートの詳細](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 共有AEM環境を使用する場合、に一意の ID を追加します。 **サイト名**. 例： `wknd-site-johndoe`. これにより、衝突を起こさずに複数のユーザーが同じチュートリアルを完了することができます。

1. クリック **作成** をクリックしてサイトを生成します。 クリック **完了** 内 **成功** ダイアログが表示されます。

## 新しいサイトを参照

1. AEM Sitesコンソールに移動します（まだ移動していない場合）。
1. 新しい **WKND サイト** が生成されました。 多言語階層のサイト構造が含まれます。
1. を開きます。 **英語** > **ホーム** ページを表示するには、ページを選択して **編集** ボタンをクリックします。

   ![WKND サイト階層](assets/create-site/wknd-site-starter-hierarchy.png)

1. スターターコンテンツは既に作成されており、ページに追加できる複数のコンポーネントが用意されています。 これらのコンポーネントを使用してみることで、機能について大まかに把握できます。次の章では、コンポーネントの基本について学びます。

   ![ホームページを開始](assets/create-site/start-home-page.png)

   *サイトテンプレートから提供されるサンプルコンテンツ*

## おめでとうございます。 {#congratulations}

これで、最初のAEM Site が作成されました。

### 次の手順 {#next-steps}

AEMのAdobe Experience Managerのページエディターを使用して、 [コンテンツのオーサリングと公開](author-content-publish.md) チャプター。 コンテンツを更新するためにアトミックコンポーネントを設定する方法を説明します。 AEM オーサー環境とパブリッシュ環境の違いを理解し、ライブサイトに更新を公開する方法を学びます。
