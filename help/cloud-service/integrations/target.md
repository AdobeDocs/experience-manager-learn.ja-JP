---
title: AEMヘッドレスと Target のパーソナライゼーション
description: このチュートリアルでは、AEMコンテンツフラグメントをAdobe Targetに書き出し、AdobeWeb SDK を使用してヘッドレスエクスペリエンスをパーソナライズする方法について説明します。
version: Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
exl-id: 60a3e18a-090f-4b0e-8ba0-d4afd30577dd
source-git-commit: d81c66e041abbd56e7115f37732550cf10e59359
workflow-type: tm+mt
source-wordcount: '1671'
ht-degree: 2%

---

# コンテンツフラグメントを使用したAEMヘッドレスエクスペリエンスのパーソナライズ

このチュートリアルでは、AEMコンテンツフラグメントをAdobe Targetに書き出し、AdobeWeb SDK を使用してヘッドレスエクスペリエンスをパーソナライズする方法について説明します。 この [React WKND アプリ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html) は、WKND アドベンチャーを促進するために、コンテンツフラグメントオファーを使用してパーソナライズされた Target アクティビティをエクスペリエンスに追加する方法を調べるために使用されます。

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

このチュートリアルでは、AEMとAdobe Targetの設定に関する手順を説明します。

1. [Adobe TargetのAdobe IMS設定を作成](#adobe-ims-configuration) （AEM オーサー）
1. [Adobe TargetCloud Service](#adobe-target-cloud-service) （AEM オーサー）
1. [AEM AssetsCloud ServiceーへのAdobe Targetフォルダーの適用](#configure-asset-folders) （AEM オーサー）
1. [Adobe TargetCloud Service](#permission) Adobe Admin Console
1. [コンテンツフラグメントを書き出し](#export-content-fragments) AEM オーサーからターゲットへ
1. [コンテンツフラグメントオファーを使用したアクティビティの作成](#activity) Adobe Target
1. [Experience Platformデータストリームの作成](#datastream-id) Experience Platform
1. [パーソナライゼーションを React ベースのAEMヘッドレスアプリに統合する](#code) AdobeWeb SDK を使用する。

## Adobe IMS設定{#adobe-ims-configuration}

AEMとAdobe Target間の認証を容易にするAdobe IMS設定。

レビュー [ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html) Adobe IMS設定の作成手順を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Adobe TargetCloud Service{#adobe-target-cloud-service}

Adobe TargetCloud Serviceは、コンテンツフラグメントをAdobe Targetに書き出しやすくするために、AEMで作成されます。

レビュー [ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html?lang=ja) Adobe TargetCloud Serviceの作成手順を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## アセットフォルダーの設定{#configure-asset-folders}

コンテキスト対応Cloud Serviceで設定したAdobe Target設定は、Adobe Targetに書き出すコンテンツフラグメントを含むAEM Assetsフォルダー階層に適用する必要があります。

+++詳しい手順については、展開してください。

1. にログインします。 __AEM オーサーサービス__ DAM 管理者として
1. に移動します。 __アセット/ファイル__&#x200B;で、 `/conf` 適用先
1. アセットフォルダーを選択し、「 」を選択します。 __プロパティ__ 上部のアクションバーから
1. を選択します。 __Cloud Services__ タブ
1. クラウド設定がコンテキスト対応設定 (`/conf`) にAdobe TargetCloud Services設定が含まれます。
1. 選択 __Adobe Target__ から __Cloud Service設定__ ドロップダウン。
1. 選択 __保存して閉じる__ 右上に

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## AEM Target 統合の権限{#permission}

Adobe Target統合は、developer.adobe.com プロジェクトとして表示され、 __編集者__ Adobe Admin Consoleの製品ロールを使用して、コンテンツフラグメントをAdobe Targetに書き出すことができます。

+++詳しい手順については、展開してください。

1. Adobe Admin ConsoleでAdobe Target製品を管理できるExperience Cloudとしてユーザーにログインします。
1. を開きます。 [Adobe Admin Console](https://adminconsole.adobe.com)
1. 選択 __製品__ そして、 __Adobe Target__
1. の __製品プロファイル__ タブ、選択 __*DefaultWorkspace*__
1. を選択します。 __API 資格情報__ タブ
1. このリストで developer.adobe.com アプリを見つけ、 __製品の役割__ から __編集者__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## コンテンツフラグメントを Target に書き出す{#export-content-fragments}

の下に存在するコンテンツフラグメント [設定済みのAEM Assetsフォルダー階層](#apply-adobe-target-cloud-service-to-aem-assets-folders) は、コンテンツフラグメントオファーとしてAdobe Targetに書き出すことができます。 これらのコンテンツフラグメントオファーは、Target の JSON オファーの特殊な形式で、ヘッドレスアプリでパーソナライズされたエクスペリエンスを提供するために、Target アクティビティで使用できます。

+++詳しい手順については、展開してください。

1. にログインします。 __AEM オーサー__ DAM ユーザーとして
1. に移動します。 __アセット/ファイル__&#x200B;を開き、「Adobe Target enabled」フォルダーの下で、JSON として Target に書き出すコンテンツフラグメントを探します。
1. Adobe Targetに書き出すコンテンツフラグメントを選択します。
1. 選択 __Adobe Targetオファーに書き出し__ 上部のアクションバーから
   + このアクションは、コンテンツフラグメントの完全にハイドレートされた JSON 表現を、「コンテンツフラグメントオファー」としてAdobe Targetに書き出します。
   + 完全にハイドレートされた JSON 表現をAEMで確認できます
      + コンテンツフラグメントを選択
      + サイドパネルを展開します。
      + 選択 __プレビュー__ 左側のパネルのアイコン
      + Adobe Targetに書き出された JSON 表現は、メインビューに表示されます
1. にログインします。 [Adobe Experience Cloud](https://experience.adobe.com) Adobe Targetのエディターの役割を持つユーザー
1. 次の [Experience Cloud](https://experience.adobe.com)を選択します。 __ターゲット__ 右上の製品切り替えボタンからAdobe Targetを開きます。
1. 「デフォルトのワークスペース」が __ワークスペース切り替え__ をクリックします。
1. を選択します。 __オファー__ 上部のナビゲーションの「 」タブ
1. を選択します。 __タイプ__ ドロップダウン、選択 __コンテンツフラグメント__
1. AEMから書き出したコンテンツフラグメントがリストに表示されることを確認します。
   + オファーにカーソルを合わせ、 __表示__ ボタン
   + 以下を確認します。 __オファー情報__ また、 __AEMディープリンク__ AEM オーサーサービスでコンテンツフラグメントを直接開く

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## コンテンツフラグメントオファーを使用した Target アクティビティ{#activity}

Adobe Targetでは、コンテンツフラグメントオファー JSON をコンテンツとして使用するアクティビティを作成し、AEMで作成および管理されたコンテンツを使用してヘッドレスアプリでパーソナライズされたエクスペリエンスを実現できます。

この例では、シンプルな A/B アクティビティを使用しますが、任意の Target アクティビティを使用できます。

+++詳しい手順については、展開してください。

1. を選択します。 __アクティビティ__ 上部のナビゲーションの「 」タブ
1. 選択 __+アクティビティを作成__&#x200B;をクリックし、作成するアクティビティのタイプを選択します。
   + この例では、単純な __A/B テスト__ しかし、コンテンツフラグメントオファーは、どのアクティビティタイプでも機能します
1. 内 __アクティビティを作成__ ウィザード
   + 選択 __Web__
   + In __Experience Composer を選択__&#x200B;を選択します。 __フォーム__
   + In __ワークスペースを選択__&#x200B;を選択します。 __デフォルトのワークスペース__
   + In __プロパティを選択__、アクティビティを使用できるプロパティを選択するか、 __プロパティの制限がありません__ を使用して、すべてのプロパティで使用できるようにします。
   + 選択 __次へ__ アクティビティを作成するには
1. 「 」を選択して、アクティビティの名前を変更します。 __名前変更__ 左上
   + アクティビティにわかりやすい名前を付ける
1. 最初のエクスペリエンスで、 __場所 1__ ターゲットとするアクティビティ
   + この例では、という名前のカスタムの場所をターゲットにします。 `wknd-adventure-promo`
1. の下 __コンテンツ__ 「デフォルトコンテンツ」を選択し、「 __コンテンツフラグメントを変更__
1. このエクスペリエンス用に書き出したコンテンツフラグメントを選択し、「 」を選択します。 __完了__
1. 「コンテンツテキスト」領域で「コンテンツフラグメントオファー JSON」を確認します。これは、AEM オーサーサービスでコンテンツフラグメントのプレビューアクションから利用できる JSON と同じです。
1. 左側のレールで、エクスペリエンスを追加し、提供する別のコンテンツフラグメントオファーを選択します
1. 選択 __次へ__&#x200B;をクリックし、アクティビティの必要に応じてターゲット設定ルールを設定します。
   + この例では、A/B テストは手動で50/50分割したままにします。
1. 選択 __次へ__&#x200B;をクリックし、アクティビティの設定を完了します。
1. 選択 __保存して閉じる__ そして意味のある名前を付け
1. Adobe Targetの「アクティビティ」から、 __有効化__ 右上の非アクティブ/アクティブ化/アーカイブドロップダウンから、次の操作を実行します。

Adobe Targetアクティビティ `wknd-adventure-promo` の場所を統合し、AEMヘッドレスアプリで公開できるようになりました。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## Experience Platformデータストリーム ID{#datastream-id}

An [Adobe Experience Platform Datastream](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) AEMヘッドレスアプリが [AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++詳しい手順については、展開してください。

1. に移動します。 [Adobe Experience Cloud](https://experience.adobe.com/)
1. 開く __Experience Platform__
1. 選択 __データ収集/データストリーム__ を選択し、 __新規データストリーム__
1. 新規データストリームウィザードで、次のように入力します。
   + 名前：`AEM Target integration`
   + 説明: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + イベントスキーマ： `Leave blank`
1. 「__保存__」を選択します
1. 選択 __サービスを追加__
1. In __サービス__ 選択 __Adobe Target__
   + 有効： __はい__
   + プロパティトークン： __空白のままにする__
   + ターゲット環境 ID: __空白のままにする__
      + Target 環境は、 Adobe Target ( ) で設定できます。 __管理/ホスト__.
   + Target サードパーティ ID 名前空間： __空白のままにする__
1. 「__保存__」を選択します
1. 右側で、 __データストリーム ID__ 使用 [AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html) 設定呼び出し。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## AEMヘッドレスアプリへのパーソナライゼーションの追加{#code}

このチュートリアルでは、を介してAdobe Targetのコンテンツフラグメントオファーを使用したシンプルな React アプリのパーソナライズについて説明します。 [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=ja). この方法は、JavaScript ベースの Web エクスペリエンスをパーソナライズするために使用できます。

Android™およびiOSのモバイルエクスペリエンスは、 [Adobeのモバイル SDK](https://developer.adobe.com/client-sdks/documentation/).

### 前提条件

+ Node.js 14
+ Git
+ [WKND Shared 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) AEM as a Cloud Author および Publish Services にインストールされている

### セットアップ

1. サンプル React アプリのソースコードをからダウンロードします。 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. コードベースを次の場所で開きます。 `~/Code/aem-guides-wknd-graphql/personalization-tutorial` お気に入りの IDE 内
1. アプリの接続先となるAEMサービスのホストを更新する `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. アプリを実行し、設定済みのAEMサービスに接続されていることを確認します。 コマンドラインから、次のコマンドを実行します。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. のインストール [AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package) を NPM パッケージとして追加します。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   Web SDK をコード内で使用して、アクティビティの場所別にコンテンツフラグメントオファー JSON を取得できます。

   Web SDK を設定する場合、次の 2 つの ID が必要です。

   + `edgeConfigId` これは [データストリーム ID](#datastream-id)
   + `orgId` 次の場所にあるAEMas a Cloud Service/TargetAdobe組織 ID __Experience Cloud/プロファイル/アカウント情報/現在の組織 ID__

   Web SDK を呼び出すと、Adobe Targetアクティビティの場所 ( この例では `wknd-adventure-promo`) を `decisionScopes` 配列。

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### 実装

1. React コンポーネントの作成 `AdobeTargetActivity.js` Adobe Targetアクティビティを表示します。

   __src/components/AdobeTargetActivity.js__

   ```javascript
   import React, { useEffect } from 'react';
   import { createInstance } from '@adobe/alloy';
   
   const alloy = createInstance({ name: 'alloy' });
   
   alloy('configure', { 
     'edgeConfigId': 'e3db252d-44d0-4a0b-8901-aac22dbc88dc', // AEP Datastream ID
     'orgId':'7ABB3E6A5A7491460A495D61@AdobeOrg',
     'debugEnabled': true,
   });
   
   export default function AdobeTargetActivity({ activityLocation, OfferComponent }) { 
     const [offer, setOffer] = React.useState();
   
     useEffect(() => {
       async function sendAlloyEvent() {
         // Get the activity offer from Adobe Target
         const result = await alloy('sendEvent', {
           // decisionScopes is set to an array containing the Adobe Target activity location
           'decisionScopes': [activityLocation],
         });
   
         if (result.propositions?.length > 0) {
           // Find the first proposition for the active activity location
           var proposition = result.propositions?.filter((proposition) => { return proposition.scope === activityLocation; })[0];
   
           // Get the Content Fragment Offer JSON from the Adobe Target response
           const contentFragmentOffer = proposition?.items[0]?.data?.content || { status: 'error', message: 'Personalized content unavailable'};
   
           if (contentFragmentOffer?.data) {
             // Content Fragment Offers represent a single Content Fragment, hydrated by
             // the byPath GraphQL query, we must traverse the JSON object to retrieve the 
             // Content Fragment JSON representation
             const byPath = Object.keys(contentFragmentOffer.data)[0];
             const item = contentFragmentOffer.data[byPath]?.item;
   
             if (item) {
               // Set the offer to the React state so it can be rendered
               setOffer(item);
   
               // Record the Content Fragment Offer as displayed for Adobe Target Activity reporting
               // If this request is omitted, the Target Activity's Reports will be blank
               alloy("sendEvent", {
                   xdm: {
                       eventType: "decisioning.propositionDisplay",
                       _experience: {
                           decisioning: {
                               propositions: [proposition]
                           }
                       }
                   }
               });          
             }
           }
         }
       };
   
       sendAlloyEvent();
   
     }, [activityLocation, OfferComponent]);
   
     if (!offer) {
       // Adobe Target offer initializing; we render a blank component (which has a fixed height) to prevent a layout shift
       return (<OfferComponent></OfferComponent>);
     } else if (offer.status === 'error') {
       // If Personalized content could not be retrieved either show nothing, or optionally default content.
       console.error(offer.message);
       return (<></>);
     }
   
     console.log('Activity Location', activityLocation);
     console.log('Content Fragment Offer', offer);
   
     // Render the React component with the offer's JSON
     return (<OfferComponent content={offer} />);
   };
   ```

   AdobeTargetActivity React コンポーネントは、次のようにを使用して呼び出します。

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. React コンポーネントの作成 `AdventurePromo.js` アドベンチャー JSON Adobe Targetサービスをレンダリングするには：

   この React コンポーネントは、アドベンチャーコンテンツフラグメントを表す完全にハイドレートされた JSON を取得し、プロモーション方法で表示します。 Adobe Targetコンテンツフラグメントオファーから提供される JSON を表示する React コンポーネントは、Adobe Targetに書き出されるコンテンツフラグメントに基づいて、必要に応じて様々で複雑になります。

   __src/components/AdventurePromo.js__

   ```javascript
   import React from 'react';
   
   import './AdventurePromo.scss';
   
   /**
   * @param {*} content is the fully hydrated JSON data for a WKND Adventure Content Fragment
   * @returns the Adventure Promo component
   */
   export default function AdventurePromo({ content }) {
       if (!content) {
           // If content is still loading, then display an empty promote to prevent layout shift when Target loads the data
           return (<div className="adventure-promo"></div>)
       }
   
       const title = content.title;
       const description = content?.description?.plaintext;
       const image = content.primaryImage?._publishUrl;
   
       return (
           <div className="adventure-promo">
               <div className="adventure-promo-text-wrapper">
                   <h3 className="adventure-promo-eyebrow">Promoted adventure</h3>
                   <h2 className="adventure-promo-title">{title}</h2>
                   <p className="adventure-promo-description">{description}</p>
               </div>
               <div className="adventure-promo-image-wrapper">
                   <img className="adventure-promo-image" src={image} alt={title} />
               </div>
           </div>
       )
   }
   ```

   __src/components/AdventurePromo.scss__

   ```css
   .adventure-promo {
       display: flex;
       margin: 3rem 0;
       height: 400px;
   }
   
   .adventure-promo-text-wrapper {
       background-color: #ffea00;
       color: black;
       flex-grow: 1;
       padding: 3rem 2rem;
       width: 55%;
   }
   
   .adventure-promo-eyebrow {
       font-family: Source Sans Pro,Helvetica Neue,Helvetica,Arial,sans-serif;
       font-weight: 700;
       font-size: 1rem;
       margin: 0;
       text-transform: uppercase;
   }
   
   .adventure-promo-description {
       line-height: 1.75rem;
   }
   
   .adventure-promo-image-wrapper {
       height: 400px;
       width: 45%;
   }
   
   .adventure-promo-image {
       height: 100%;
       object-fit: cover;
       object-position: center center;
       width: 100%;
   }
   ```

   この React コンポーネントは、次のように呼び出されます。

   ```jsx
   <AdventurePromo adventure={adventureJSON}/>
   ```

1. React アプリのに AdobeTargetActivity コンポーネントを追加する `Home.js` 冒険のリストの上に

   __src/components/Home.js__

   ```javascript
   import AdventurePromo from './AdventurePromo';
   import AdobeTargetActivity from './AdobeTargetActivity';
   ... 
   export default function Home() {
       ...
       return(
           <div className="Home">
   
             <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   
             <h2>Current Adventures</h2>
             ...
       )
   }
   ```

1. React アプリが実行されていない場合は、 `npm run start`.

   2 つの異なるブラウザーで React アプリを開き、A/B テストで各ブラウザーに異なるエクスペリエンスを提供できるようにします。 両方のブラウザーに同じアドベンチャーオファーが表示される場合は、別のエクスペリエンスが表示されるまで、いずれかのブラウザーを閉じるか、再度開いてみてください。

   次の画像は、 `wknd-adventure-promo` Adobe Targetのロジックに基づくアクティビティ。

   ![エクスペリエンスオファー](./assets/target/offers-in-app.png)

## おめでとうございます。

これで、コンテンツフラグメントをAdobe Targetに書き出し、Adobe Targetアクティビティでコンテンツフラグメントオファーを使用し、AEMヘッドレスアプリでそのアクティビティを表示して、エクスペリエンスをパーソナライズするようにAEMas a Cloud Serviceを設定しました。
