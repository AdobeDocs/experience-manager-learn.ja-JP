---
title: AEM ヘッドレスと Target の統合
description: AEM ヘッドレスと Adobe Target を統合し、Experience Platform Web SDK を使用してヘッドレスエクスペリエンスをパーソナライズする方法を説明します。
version: Experience Manager as a Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: be886c64-9b8e-498d-983c-75f32c34be4b
duration: 1549
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1618'
ht-degree: 100%

---

# AEM ヘッドレスと Target の統合

AEM コンテンツフラグメントを Adobe Target に書き出すことで AEM ヘッドレスを Adobe Target と統合し、それらのコンテンツフラグメントと Adobe Experience Platform Web SDK の alloy.js を使用してヘッドレスエクスペリエンスをパーソナライズする方法を説明します。[React WKND アプリ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html?lang=ja)を使用して、WKND アドベンチャーを促進するために、コンテンツフラグメントオファーを使用してパーソナライズされた Target アクティビティをエクスペリエンスに追加する方法を探索します。

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

このチュートリアルでは、AEM と Adobe Target の設定に関する手順を説明します。

1. AEM オーサーでの [Adobe Target の Adobe IMS 設定の作成](#adobe-ims-configuration)
2. AEM オーサーでの [Adobe Target Cloud Service の作成](#adobe-target-cloud-service)
3. AEM オーサーでの [AEM Assets フォルダーへの Adobe Target Cloud Service の適用](#configure-asset-folders)
4. Adobe Admin Console での [Adobe Target Cloud Service の権限](#permission)
5. AEM オーサーから Target への[コンテンツフラグメントの書き出し](#export-content-fragments)
6. Adobe Target での[コンテンツフラグメントオファーを使用したアクティビティの作成](#activity)
7. Experience Platform での [Experience Platform データストリームの作成](#datastream-id)
8. Adobe Web SDK を使用して、[パーソナライゼーションを React ベースの AEM ヘッドレスアプリに統合](#code)します。

## Adobe IMS 設定{#adobe-ims-configuration}

AEM と Adobe Target 間の認証を容易にする Adobe IMS 設定。

Adobe IMS 設定の作成方法に関する詳しい手順については、[ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html?lang=ja)を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Adobe Target Cloud Service{#adobe-target-cloud-service}

Adobe Target Cloud Service は、コンテンツフラグメントを Adobe Target に書き出しやすくするために、AEM で作成されます。

Adobe Target Cloud Service を作成する手順については、[ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html?lang=ja)を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## アセットフォルダーの設定{#configure-asset-folders}

コンテキスト対応設定で設定された Adobe Target Cloud Service は、Adobe Target に書き出すコンテンツフラグメントを含む AEM Assets フォルダー階層に適用する必要があります。

+++詳しい手順については、展開してください

1. __AEM オーサーサービス__&#x200B;に DAM 管理者としてログインします
1. __アセット／ファイル__&#x200B;に移動し、`/conf` が適用されているアセットフォルダーを見つけます
1. アセットフォルダーを選択し、アクションバーの「__プロパティ__」を選択します
1. 「__Cloud Services__」タブを選択します
1. クラウド設定が、Adobe Target Cloud Services 設定を含むコンテキスト対応設定（`/conf`）に設定されていることを確認します。
1. __Cloud Service 設定__&#x200B;ドロップダウンリストから「__Adobe Target__」を選択します。
1. 右上の「__保存して閉じる__」を選択します。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## AEM Target 統合の権限{#permission}

コンテンツフラグメントを Adobe Target に書き出すには、developer.adobe.com プロジェクトとして表示される Adobe Target 統合に、製品&#x200B;__編集者__&#x200B;の役割を Adobe Admin Console で付与する必要があります。

+++詳しい手順については、展開してください

1. Adobe Admin Console で、Adobe Target 製品を管理できるユーザーとして Experience Cloud にログインします。
1. [Adobe Admin Console](https://adminconsole.adobe.com) を開きます。
1. 「__製品__」を選択してから、__Adobe Target__ を開きます。
1. 「__製品プロファイル__」タブで、「__*DefaultWorkspace*__」を選択します。
1. 「__API 資格情報__」タブを選択します。
1. このリストで developer.adobe.com アプリを見つけ、そのアプリの「__製品の役割__」を&#x200B;__編集者__&#x200B;に設定します。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## Target へのコンテンツフラグメントの書き出し{#export-content-fragments}

[設定済みの AEM Assetsフォルダー階層](#apply-adobe-target-cloud-service-to-aem-assets-folders)の下に存在するコンテンツフラグメントは、コンテンツフラグメントオファーとして Adobe Target に書き出すことができます。これらのコンテンツフラグメントオファーは、Target における特殊な形式の JSON オファーであり、Target アクティビティで使用して、パーソナライズされたエクスペリエンスをヘッドレスアプリで提供できます。

+++詳しい手順については、展開してください

1. __AEM オーサー__&#x200B;に DAM ユーザーとしてログインします。
1. __アセット／ファイル__&#x200B;に移動し、「Adobe Target 有効」フォルダーの下で、JSON として Target に書き出すコンテンツフラグメントを見つけます。
1. Adobe Target に書き出すコンテンツフラグメントを選択します。
1. 上部のアクションバーの「__Adobe Target オファーに書き出し__」を選択します。
   + このアクションは、コンテンツフラグメントの完全にハイドレートされた JSON 表現を、「コンテンツフラグメントオファー」として Adobe Target に書き出します。
   + 完全にハイドレートされた JSON 表現を AEM で確認できます。
      + コンテンツフラグメントを選択します。
      + サイドパネルを展開します。
      + 左側のパネルの「__プレビュー__」アイコンを選択します。
      + Adobe Target に書き出される JSON 表現がメインビューに表示されます。
1. Adobe Target の編集者の役割を持つユーザーで [Adobe Experience Cloud](https://experience.adobe.com) にログインします。
1. [Experience Cloud](https://experience.adobe.com) で、右上の製品スイッチャーから「__Target__」を選択して、Adobe Target を開きます。
1. 右上の&#x200B;__ワークスペーススイッチャー__&#x200B;でデフォルトのワークスペースが選択されていることを確認します。
1. 上部のナビゲーションで「__オファー__」タブを選択します。
1. 「__タイプ__」ドロップダウンを選択し、「__コンテンツフラグメント__」を選択します。
1. AEM から書き出したコンテンツフラグメントがリストに表示されることを確認します。
   + オファーにポインタを合わせて、「__表示__」ボタンを選択します。
   + __オファー情報__&#x200B;を確認し、AEM オーサーサービスでコンテンツフラグメントを直接開く __AEM ディープリンク__&#x200B;を参照します。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## コンテンツフラグメントオファーを使用した Target アクティビティ{#activity}

Adobe Target では、コンテンツフラグメントオファー JSON をコンテンツとして使用するアクティビティを作成できるので、AEM で作成および管理されたコンテンツを使用するヘッドレスアプリで、パーソナライズされたエクスペリエンスを提供できます。

この例では、単純な A/B アクティビティを使用しますが、任意の Target アクティビティを使用できます。

+++詳しい手順については、展開してください

1. 上部のナビゲーションで「__アクティビティ__」タブを選択します。
1. 「__+ アクティビティを作成__」を選択したあと、作成するアクティビティのタイプを選択します。
   + この例では、単純な __A/B テスト__&#x200B;を作成しますが、コンテンツフラグメントオファーはあらゆるタイプのアクティビティを強化できます。
1. __アクティビティを作成__&#x200B;ウィザードで以下を行います。
   + 「__Web__」を選択します。
   + __Experience Composer を選択__&#x200B;で「__フォーム__」を選択します。
   + __ワークスペースを選択__&#x200B;で「__デフォルトのワークスペース__」を選択します。
   + __プロパティを選択__&#x200B;で、アクティビティを使用できるプロパティを選択するか、「__プロパティの制限なし__」を選択してすべてのプロパティで使用できるようにします。
   + 「__次へ__」を選択して、アクティビティを作成します。
1. 左上の「__名前変更__」を選択して、アクティビティの名前を変更します。
   + アクティビティに意味のある名前を付けます。
1. 初期のエクスペリエンスで、ターゲットとするアクティビティの「__場所 1__」を設定します。
   + この例では、`wknd-adventure-promo` という名前のカスタムの場所をターゲットにします。
1. __コンテンツ__&#x200B;の下の「デフォルトコンテンツ」を選択し、「__コンテンツフラグメントを変更__」を選択します。
1. このエクスペリエンス用に書き出したコンテンツフラグメントを選択し、「__完了__」を選択します。
1. コンテンツテキスト領域でコンテンツフラグメントオファー JSON を確認します。これは、コンテンツフラグメントのプレビューアクションを使用して AEM オーサーサービスで入手できる JSON と同じです。
1. 左側のパネルで、エクスペリエンスを追加し、提供する別のコンテンツフラグメントオファーを選択します。
1. 「__次へ__」を選択し、アクティビティの必要に応じてターゲティングルールを設定します。
   + この例では、A/B テストを手動で 50/50 に分割したままにします。
1. 「__次へ__」を選択し、アクティビティの設定を完了します。
1. 「__保存して閉じる__」を選択し、意味のある名前を付けます。
1. Adobe Target のアクティビティで、右上の非アクティブ／アクティベート／アーカイブドロップダウンから「__アクティベート__」を選択します。

`wknd-adventure-promo` の場所をターゲットにする Adobe Target アクティビティを AEM ヘッドレスアプリに統合して公開できるようになりました。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## Experience Platform データストリーム ID{#datastream-id}

AEM ヘッドレスアプリで [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html?lang=ja) を使用して Adobe Target とやり取りするには、[Adobe Experience Platform データストリーム](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html?lang=ja) ID が必要です。

+++詳しい手順については、展開してください

1. [Adobe Experience Cloud](https://experience.adobe.com/) に移動します。
1. __Experience Platform__ を開きます。
1. __データ収集／データストリーム__&#x200B;を選択し、「__新しいデータストリーム__」を選択します。
1. 新しいデータストリームウィザードで、次のように入力します。
   + 名前：`AEM Target integration`
   + 説明：`Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + イベントスキーマ：`Leave blank`
1. 「__保存__」を選択します
1. 「__サービスを追加__」を選択します。
1. __サービス__&#x200B;で「__Adobe Target__」を選択します
   + 有効：__はい__
   + プロパティトークン：__空白のままにします__
   + ターゲット環境 ID：__空白のままにします__
      + ターゲット環境は、Adobe Target の&#x200B;__管理／ホスト__&#x200B;で設定できます。
   + ターゲットサードパーティ ID 名前空間：__空白のままにします__
1. 「__保存__」を選択します
1. 右側で、[Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html?lang=ja) 設定呼び出しで使用する&#x200B;__データストリーム ID__ をコピーします。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## AEM ヘッドレスアプリへのパーソナライゼーションの追加{#code}

このチュートリアルでは、[Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=ja) 経由で Adobe Target のコンテンツフラグメントオファーを使用して、シンプルな React アプリをパーソナライズする方法を説明します。このアプローチを使用すると、JavaScript ベースの web エクスペリエンスをパーソナライズできます。

Android™ および iOS のモバイルエクスペリエンスは、[アドビの Mobile SDK](https://developer.adobe.com/client-sdks/documentation/) を使用して類似したパターンに従ってパーソナライズできます。

### 前提条件

+ Node.js 14
+ Git
+ [WKND Shared 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) が、クラウドオーサーおよびパブリッシュサービスとして AEM にインストールされている

### セットアップ

1. [Github.com](https://github.com/adobe/aem-guides-wknd-graphql) からサンプル React アプリのソースコードをダウンロードします。

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. お気に入りの IDE で `~/Code/aem-guides-wknd-graphql/personalization-tutorial` にあるコードベースを開きます。
1. アプリを `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development` に接続する AEM サービスのホストを更新します。

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. アプリを実行し、設定済みの AEM サービスに接続していることを確認します。コマンドラインから、次を実行します。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html?lang=ja#option-3%3A-using-the-npm-package) を NPM パッケージとしてインストールします。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   Web SDK をコード内で使用すると、アクティビティの場所別にコンテンツフラグメントオファー JSON を取得できます。

   Web SDK を設定する場合、次の 2 つの ID が必要です。

   + `edgeConfigId`：[データストリーム ID](#datastream-id)
   + `orgId`：__Experience Cloud／プロファイル／アカウント情報／現在の組織 ID__ で確認できる AEM as a Cloud Service／Target アドビ組織 ID

   Web SDK を呼び出す際は、Adobe Target アクティビティの場所（この例では、`wknd-adventure-promo`）を、`decisionScopes` 配列の値として設定する必要があります。

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### 実装

1. React コンポーネント `AdobeTargetActivity.js` を作成して、Adobe Target アクティビティを表示します。

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

   AdobeTargetActivity React コンポーネントは、次のように呼び出します。

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. React コンポーネント `AdventurePromo.js` を作成して、Adobe Target が提供するアドベンチャー JSON をレンダリングします。

   この React コンポーネントは、アドベンチャーコンテンツのフラグメントを表す完全にハイドレートされた JSON を取得し、プロモーション方法で表示します。Adobe Target コンテンツフラグメントオファーから提供される JSON を表示する React コンポーネントは、Adobe Target に書き出されるコンテンツフラグメントに基づいて、必要に応じて多様かつ複雑にすることができます。

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

1. AdobeTargetActivity コンポーネントを、アドベンチャーのリストの上にある React アプリの `Home.js` に追加します。

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

1. React アプリが実行されていない場合は、`npm run start` を使用して再起動します。

   2 つの異なるブラウザーで React アプリを開き、A/B テストが各ブラウザーに異なるエクスペリエンスを提供できるようにします。両方のブラウザーに同じアドベンチャーオファーが表示される場合は、もう一方のエクスペリエンスが表示されるまで、ブラウザーの 1 つを閉じて再度開きます。

   次の画像は、Adobe Target のロジックに基づいて、`wknd-adventure-promo` アクティビティに対して表示される 2 つの異なるコンテンツフラグメントオファーを示しています。

   ![エクスペリエンスオファー](./assets/target/offers-in-app.png)

## おめでとうございます。

コンテンツフラグメントを Adobe Target に書き出すように AEM as a Cloud Service を設定し、Adobe Target アクティビティでコンテンツフラグメントオファーを使用し、このアクティビティを AEM ヘッドレスアプリに表示して、エクスペリエンスをパーソナライズしました。
