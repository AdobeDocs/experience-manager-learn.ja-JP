---
title: サイトの作成
seo-title: AEM Sites使用の手引き — サイトの作成
description: 新しいWebサイトを作成するには、AEMのAdobe Experience Managerにある[サイトの作成]ウィザードを使用します。 Adobeが提供する標準サイトテンプレートは、新しいサイトの起点として使用されます。
sub-product: サイト
version: Cloud Service
type: Tutorial
topic: コンテンツ管理
feature: コアコンポーネント，ページエディター
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 2%

---


# サイトの作成{#create-site}

>[!CAUTION]
>
> ここで紹介するクイックサイト作成機能は、2021年下半期にリリースされます。 関連ドキュメントは、プレビュー目的で利用できます。

本章では、Adobe Experience Managerの新しい敷地の創設について説明します。 Adobeが提供する標準サイトテンプレートは、開始点として使用されます。

## 前提条件 {#prerequisites}

本章の各段階は、Cloud Service環境としてAdobe Experience Managerで行われる。 AEM環境への管理者アクセス権を持っていることを確認します。 このチュートリアルを完了する際は、[Sandboxプログラム](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html)と[開発環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html?lang=ja)を使用することをお勧めします。

詳しくは、[オンボーディングドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)を参照してください。

## 目的 {#objective}

1. サイト作成ウィザードを使用して新しいサイトを生成する方法を説明します。
1. サイトテンプレートの役割を理解します。
1. 生成されたAEMサイトを表示します。

## Adobe Experience Manager作成者にログイン{#author}

最初の手順として、AEMにCloud Service環境としてログインします。 AEM環境は、**Author Service**&#x200B;と&#x200B;**Publish Service**&#x200B;に分割されます。

* **Authorサービス**  — サイトコンテンツの作成、管理、更新を行います。通常、**Author Service**&#x200B;にアクセスできるのは内部ユーザーのみで、ログイン画面の背後にあります。
* **発行サービス**  — ライブWebサイトをホストします。これは、エンドユーザーに表示されるサービスで、通常は一般に利用できます。

チュートリアルの大部分は、**Author Service**&#x200B;を使用して行われます。

1. Adobe Experience Cloud[https://experience.adobe.com/](https://experience.adobe.com/)に移動します。 個人アカウントまたは会社/学校のアカウントを使用してログインします。
1. メニューで正しい組織が選択されていることを確認し、**Experience Manager**&#x200B;をクリックします。

   ![Experience Cloudホーム](assets/create-site/experience-cloud-home-screen.png)

1. **Cloud Manager**&#x200B;の下にある&#x200B;**「**&#x200B;を起動」をクリックします。
1. 使用するプログラムの上にマウスポインターを置き、**Cloud Managerプログラム**&#x200B;アイコンをクリックします。

   ![Cloud Managerプログラムアイコン](assets/create-site/cloud-manager-program-icon.png)

1. トップメニューで「**環境**」をクリックして、プロビジョニングされた環境を表示します。

1. 使用する環境を探し、**作成者URL**&#x200B;をクリックします。

   ![開発者へのアクセス](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >このチュートリアルには、**開発版**&#x200B;環境を使用することをお勧めします。

1. AEM **作成者サービス**&#x200B;に対して新しいタブが起動します。 「**Adobeでサインイン**」をクリックすると、同じExperience Cloud資格情報で自動的にログインする必要があります。

1. リダイレクトされて認証された後、AEM開始画面が表示されます。

   ![AEM開始画面](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Experience Managerへのアクセスで問題が発生した場合 [オンボーディングドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)を確認

## 基本サイトテンプレートのダウンロード

サイトテンプレートは、新しいサイトを作成するための開始点です。 サイトテンプレートには、いくつかの基本的なテーマ、ページテンプレート、設定、サンプルコンテンツが含まれます。 サイトテンプレートに含まれる内容は、開発者次第です。 Adobeは、新しい導入を迅速化するための&#x200B;**基本的なサイトテンプレート**&#x200B;を提供します。

1. 新しいブラウザータブを開き、GitHub上のBasic Site Templateプロジェクトに移動します。[https://github.com/adobe/aem-site-template-basic](https://github.com/adobe/aem-site-template-basic). プロジェクトはオープンソースで、誰でも使用できるライセンスを持っています。
1. 「**リリース**」をクリックし、[最新リリース](https://github.com/adobe/aem-site-template-basic/releases/latest)に移動します。
1. 「**アセット**」ドロップダウンを展開し、テンプレートのzipファイルをダウンロードします。

   ![基本サイトテンプレートZip](assets/create-site/template-basic-zip-file.png)

   このzipファイルは次の演習で使用します。

   >[!NOTE]
   >
   > このチュートリアルは、基本サイトテンプレートのバージョン&#x200B;**5.0.0**&#x200B;を使用して書かれています。 新しいプロジェクトを開始する場合は、常に最新バージョンを使用することをお勧めします。

## 新しいサイトの作成

次に、前の練習の[サイトテンプレート]を使用して新しいサイトを生成します。

1. AEM環境に戻ります。 AEM開始画面で、**サイト**&#x200B;に移動します。
1. 右上隅の&#x200B;**作成**/**サイト（テンプレート）**&#x200B;をクリックします。 これにより、**サイトの作成ウィザード**&#x200B;が表示されます。
1. 「**サイトテンプレート**&#x200B;を選択」で、「**読み込み**」ボタンをクリックします。

   前の演習からダウンロードした&#x200B;**.zip**&#x200B;テンプレートファイルをアップロードします。

1. **基本AEMサイトテンプレート**&#x200B;を選択し、**次へ**&#x200B;をクリックします。

   ![サイトテンプレートの選択](assets/create-site/select-site-template.png)

1. **サイトの詳細** > **サイトのタイトル**&#x200B;の下に`WKND Site`と入力します。
1. 「**サイト名**」に`wknd`と入力します。

   ![サイトテンプレートの詳細](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 共有AEM環境を使用する場合は、**サイト名**&#x200B;に一意の識別子を追加します。 （例：`wknd-johndoe`）。 これにより、衝突を起こさずに複数のユーザが同じチュートリアルを完了できるようになります。

1. 「**作成**」をクリックしてサイトを生成します。 AEMがWebサイトの作成を終了したら、**成功**&#x200B;ダイアログで「**完了**」をクリックします。

## 新しいサイトの参照

1. AEM Sitesコンソールに移動します（まだない場合）。
1. 新しい&#x200B;**WKNDサイト**&#x200B;が生成されました。 多言語階層のサイト構造が含まれます。
1. **英語**/**ホーム**&#x200B;ページを開きます。ページを選択し、メニューバーの&#x200B;**編集**&#x200B;ボタンをクリックします。

   ![WKNDサイト階層](assets/create-site/wknd-site-starter-hierarchy.png)

1. スターターコンテンツは既に作成済みで、ページに追加できるコンポーネントがいくつかあります。 これらのコンポーネントを使用してみることで、機能について大まかに把握できます。コンポーネントの基本については、次の章で学習します。

   ![開始ホームページ](assets/create-site/start-home-page.png)

   *サイトテンプレートから提供されるサンプルコンテンツ*

## これで完了です! {#congratulations}

初めてのAEMサイトを作成しました。

### 次の手順 {#next-steps}

AEMのAdobe Experience Managerのページエディターを使用して、[作成者コンテンツを更新し、](author-content-publish.md)を公開します。 アトミックコンポーネントを設定してコンテンツを更新する方法を学びます。 AEMの作成者と発行の環境の違いを理解し、更新を本番用サイトに発行する方法を学びます。
