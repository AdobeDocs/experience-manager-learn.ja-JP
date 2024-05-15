---
title: 編集可能なコンポーネントのリモート SPA 動的ルートへの追加
description: リモート SPA の動的ルートに編集可能なコンポーネントを追加する方法を説明します。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7636
thumbnail: kt-7636.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
duration: 202
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 100%

---

# 動的ルートと編集可能なコンポーネント

この章では、2 つの動的な「アドベンチャーの詳細」ルートを有効にして、編集可能なコンポーネント（__Bali Surf Camp__ と __Beervana in Portland__）をサポートします。

![動的ルートと編集可能なコンポーネント](./assets/spa-dynamic-routes/intro.png)

「アドベンチャーの詳細」SPA ルートは、`/adventure/:slug` として定義されます。`slug` は、アドベンチャーコンテンツフラグメント上の一意の識別子プロパティです。

## SPA URL の AEM ページへのマッピング

前の 2 つの章では、編集可能なコンポーネントのコンテンツを SPA のホームビューから、`/content/wknd-app/us/en/` の AEM の対応するリモート SPA ルートページにマッピングしました。

SPA の動的ルートの編集可能なコンポーネントのマッピングの定義は似ていますが、ルートのインスタンスと AEM ページの間で 1 対 1 のマッピングスキームを作成する必要があります。

このチュートリアルでは、パスの最後のセグメントである WKND アドベンチャーコンテンツフラグメントの名前の名前を取得し、`/content/wknd-app/us/en/adventure` の下のシンプルなパスにマッピングします。

| リモート SPA ルート | AEM ページパス |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure/__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /adventure/__beervana-portland__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

したがって、このマッピングに基づいて、次の場所に 2 つの新しい AEM ページを作成する必要があります。

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## リモート SPA のマッピング

リモート SPA から移動する要求のマッピングは、[SPA のブートストラップ](./spa-bootstrap.md)で行われる `setupProxy` 設定を介して設定されます。

## SPA エディターのマッピング

SPA が AEM SPA エディターで開かれたときの SPA 要求のマッピングは、[AEM の設定](./aem-configure.md)で行われる Sling マッピング設定を使用して設定されます。

## AEM でのコンテンツページの作成

まず、中間の `adventure` ページセグメントを作成します。

1. AEM オーサーにログインします。
1. __Sites／WKND アプリ／米国／英語／WKND アプリのホームページ__&#x200B;に移動します
   + この AEM ページは SPA のルートとしてマッピングされるので、ここから他の SPA ルートの AEM ページ構造の構築を開始します。
1. 「__作成__」をタップし、「__ページ__」を選択します
1. __リモート SPA ページ__&#x200B;テンプレートを選択し、「__次へ__」をタップします
1. ページプロパティの入力
   + __タイトル__：Adventure
   + __名前__：`adventure`
      + この値は AEM ページの URL を定義するので、SPA のルートセグメントと一致する必要があります。
1. 「__完了__」をタップします

次に、編集可能な領域が必要な各 SPA URL に対応する AEM ページを作成します。

1. サイト管理者の新しい __Adventure__ ページに移動します
1. 「__作成__」をタップし、「__ページ__」を選択します
1. __リモート SPA ページ__&#x200B;テンプレートを選択し、「__次へ__」をタップします
1. ページプロパティの入力
   + __タイトル__：Bali Surf Camp
   + __名前__：`bali-surf-camp`
      + この値は AEM ページの URL を定義するので、SPA のルートの最後のセグメントと一致する必要があります
1. 「__完了__」をタップします
1. 手順 3～6 を繰り返して、__Beervana in Portland__ ページ（以下を含む）を作成します。
   + __タイトル__：Beervana in Portland
   + __名前__：`beervana-in-portland`
      + この値は AEM ページの URL を定義するので、SPA のルートの最後のセグメントと一致する必要があります

これら 2 つの AEM ページには、一致する SPA ルート用にそれぞれオーサリングされたコンテンツが格納されます。 他の SPA ルートでオーサリングが必要な場合は、AEM のリモート SPA ページのルートページ（`/content/wknd-app/us/en/home`）の下にある SPA の URL で、新しい AEM ページを作成する必要があります。

## WKND アプリの更新

[最後の章](./spa-container-component.md)で作成した `<ResponsiveGrid...>` コンポーネントを `AdventureDetail` SPA コンポーネントに配置して、編集可能なコンテナを作成します。

### ResponsiveGrid SPA コンポーネントの配置

`<ResponsiveGrid...>` を `AdventureDetail` コンポーネントに配置すると、そのルートに編集可能なコンテナが作成されます。これは、複数のルートが `AdventureDetail` コンポーネントを使用してレンダリングするので、`<ResponsiveGrid...>'s pagePath` 属性を動的に調整する必要があるためです。 `pagePath` は、ルートのインスタンスに表示されるアドベンチャーに基づいて、対応する AEM ページを指すように派生する必要があります。

1. `react-app-/src/components/AdventureDetail.js` を開いて編集する
1. `ResponsiveGrid` コンポーネントを読み込んで、`<h2>Itinerary</h2>` コンポーネントの上に配置します。
1. `<ResponsiveGrid...>` コンポーネントに次の属性を設定します。`pagePath` 属性は、上で定義されたマッピングに従ってアドベンチャーページにマップされる現在の `slug` を追加することに注意します。
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   これは、`ResponsiveGrid` コンポーネントが AEM リソースからコンテンツを取得するように指示するものです。

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

`AdventureDetail.js` を次の行で更新します。

```javascript
...
import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
...

function AdventureDetailRender(props) {
    ...
    // Get the slug from the React route parameter, this will be used to specify the AEM Page to store/read editable content from
    const { slug } = useParams();

    return(
        ...
        // Pass the slug in
        function AdventureDetailRender({ title, primaryImage, activity, adventureType, tripLength, 
                groupSize, difficulty, price, description, itinerary, references, slug }) {
            ...
            return (
                ...
                <ResponsiveGrid 
                    pagePath={`/content/wknd-app/us/en/home/adventure/${slug}`}
                    itemPath="root/responsivegrid"/>
                    
                <h2>Itinerary</h2>
                ...
            )
        }
    )
}
```

この `AdventureDetail.js` ファイルは次のようになります。

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## AEM でのコンテナの作成

`<ResponsiveGrid...>` が配置され、その `pagePath` がレンダリングされるアドベンチャーに基づいて動的に設定されている状態で、その中にコンテンツをオーサリングします。

1. AEM オーサーにログインします。
1. __Sites／WKND アプリ／米国／英語__&#x200B;に移動
1. __WKND アプリのホームページ__ のページを&#x200B;__編集__
   + __Bali Surf Camp__ SPAで編集するルートに移動
1. 右上のモードセレクターから&#x200B;__プレビュー__&#x200B;を選択
1. SPA 内の __Bali Surf Camp__ のカードをタップして、そのルートに移動します。
1. モードセレクターから「__編集__」を選択します。
1. 「__Itinerary__」のすぐ上にある&#x200B;__レイアウトコンテナ__&#x200B;の編集可能な領域を見つける
1. __ページエディターのサイドバー__&#x200B;をクリックし、「__コンポーネント表示__」を選択します。
1. 有効なコンポーネントのいくつかを&#x200B;__レイアウトコンテナ__&#x200B;にドラッグします。
   + 画像
   + テキスト
   + タイトル

   プロモーションマーケティング資料を作成します。 次のようになります。

   ![Bali Adventure Detail のオーサリング](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __AEM ページエディターで変更内容をプレビュー__
1. [http://localhost:3000](http://localhost:3000) でローカルに動作している WKND アプリを更新し、__Bali Surf Camp__ ルートに移動して、オーサリングした変更を確認します。

   ![リモートSPA Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

AEM ページがマッピングされていないアドベンチャーの詳細ルートに移動する場合、そのルートインスタンスにはオーサリング機能はありません。これらのページでオーサリングを可能にするには、__アドベンチャー__&#x200B;ページの下に、同じ名前の AEM ページを作成します。

## これで完了です。

これで完了です。オーサリング機能が SPA で動的ルートに追加されました。

+ AEM React 編集可能コンポーネントの ResponsiveGrid コンポーネントを動的ルートに追加しました。
+ SPA（Bali Surf Camp と Beervana in Portland）での 2 つの特定のルートのオーサリングをサポートする AEM ページを作成しました。
+ 動的な Bali Surf Camp ルートのコンテンツを作成しました。

AEM SPA エディターを使用して、特定の編集可能な領域を Remote SPA に追加する方法の最初の手順を完了しました。
