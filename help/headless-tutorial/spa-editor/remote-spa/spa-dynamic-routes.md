---
title: 編集可能なコンポーネントをリモートSPA Dynamic Routesに追加
description: リモートSPAの動的ルートに編集可能なコンポーネントを追加する方法を説明します。
topic: ヘッドレス、SPA、開発
feature: SPAエディター、コアコンポーネント、API、開発
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '958'
ht-degree: 1%

---


# 動的ルートと編集可能なコンポーネント

この章では、編集可能なコンポーネントをサポートする2つの動的アドベンチャー詳細ルートを有効にします。__Bali Surf Camp__&#x200B;と&#x200B;__Beervana in Portland__。

![動的ルートと編集可能なコンポーネント](./assets/spa-dynamic-routes/intro.png)

Adventure Detail SPAルートは`/adventure:path`と定義されます。ここで`path`は、詳細を表示するWKNDアドベンチャー（コンテンツフラグメント）へのパスです。

## SPA URLのAEMページへのマッピング

前の2つの章では、編集可能なコンポーネントのコンテンツをSPAホームビューからAEMの対応するリモートSPAルートページ(`/content/wknd-app/us/en/`)にマッピングしました。

SPAの動的ルートの編集可能なコンポーネントのマッピングの定義は類似していますが、ルートのインスタンスとAEMページの間で1対1のマッピングスキームを作成する必要があります。

このチュートリアルでは、パスの最後のセグメントであるWKND Adventure Content Fragmentの名前を使用し、`/content/wknd-app/us/en/adventure`の下のシンプルなパスにマッピングします。

| リモートSPAルート | AEMページパス |
|------------------------------------|--------------------------------------------|
| ／ | /content/wknd-app/us/en/home |
| /adventure:/content/dam/wknd/en/adventures/bali-surf-camp/__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /adventure:/content/dam/wknd/en/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

したがって、このマッピングに基づいて、次の場所に2つの新しいAEMページを作成する必要があります。

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## リモートSPAマッピング

リモートSPAからの要求のマッピングは、[Bootstrapの](./spa-bootstrap.md)で行われた`setupProxy`設定を使用して設定されます。

## SPA Editorマッピング

AEM SPA EditorでSPAを開いたときのSPA要求のマッピングは、[AEM](./aem-configure.md)の設定で行われたSling Mappings設定を使用して設定します。

## AEMでのコンテンツページの作成

最初に、中間の`adventure`ページセグメントを作成します。

1. AEMオーサーにログインします。
1. __サイト/WKNDアプリ/ us / en / WKNDアプリホームページ__&#x200B;に移動します。
   + このAEMページはSPAのルートとしてマッピングされるので、他のSPAルート用のAEMページ構造の構築を開始します。
1. 「__作成__」をタップし、「__ページ__」を選択します。
1. __リモートSPAページ__&#x200B;テンプレートを選択し、__次へ__&#x200B;をタップします。
1. ページプロパティの入力
   + __タイトル__:冒険
   + __名前__：`adventure`
      + この値はAEMページのURLを定義するので、SPAのルートセグメントと一致する必要があります。
1. __完了__&#x200B;をタップします。

次に、編集可能な領域を必要とするSPA URLのそれぞれに対応するAEMページを作成します。

1. サイト管理の新しい&#x200B;__アドベンチャー__&#x200B;ページに移動します。
1. 「__作成__」をタップし、「__ページ__」を選択します。
1. __リモートSPAページ__&#x200B;テンプレートを選択し、__次へ__&#x200B;をタップします。
1. ページプロパティの入力
   + __タイトル__:バリサーフキャンプ
   + __名前__：`bali-surf-camp`
      + この値はAEMページのURLを定義するので、SPAのルートの最後のセグメントと一致する必要があります
1. __完了__&#x200B;をタップします。
1. 手順3～6を繰り返して、Portland __の__ Beervanaを次のように作成します。
   + __タイトル__:ベアヴァナインポートランド
   + __名前__：`beervana-in-portland`
      + この値はAEMページのURLを定義するので、SPAのルートの最後のセグメントと一致する必要があります

これらの2つのAEMページには、一致するSPAルート用にそれぞれオーサリングされたコンテンツが含まれます。 他のSPAルートでオーサリングが必要な場合は、新しいAEM PagesをSPAのURL(Remote SPAページのルートページ(`/content/wknd-app/us/en/home`)のAEMで作成する必要があります。

## WKNDアプリの更新

[最後の章](./spa-container-component.md)で作成した`<AEMResponsiveGrid...>`コンポーネントを、`AdventureDetail` SPAコンポーネントに配置し、編集可能なコンテナを作成します。

### AEMResponsiveGrid SPAコンポーネントの配置

`<AEMResponsiveGrid...>`を`AdventureDetail`コンポーネントに配置すると、そのルートに編集可能なコンテナが作成されます。 これは、複数のルートが`AdventureDetail`コンポーネントを使用してレンダリングするため、`<AEMResponsiveGrid...>'s pagePath`属性を動的に調整する必要があるからです。 ルートのインスタンスが表示するアドベンチャーに基づいて、対応するAEMページを指すように`pagePath`を派生させる必要があります。

1. `react-app/src/components/AdventureDetail.js`を開いて編集します
1. 次の行を`AdventureDetail(..)'s` 2番目の`return(..)`ステートメントの前に追加します。このステートメントは、コンテンツフラグメントパスからアドベンチャー名を派生させます。

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = adventureData._path.split('/').pop();
   ...
   ```

1. `AEMResponsiveGrid`コンポーネントを読み込み、`<h2>Itinerary</h2>`コンポーネントの上に配置します。
1. `<AEMResponsiveGrid...>`コンポーネントに次の属性を設定します
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   これは、`AEMResponsiveGrid`コンポーネントに対し、AEMリソースからコンテンツを取得するように指示します。

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


`AdventureDetail.js`を次の行で更新します。

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = adventureData._path.split('/').pop();

    return(
        ...
        <AEMResponsiveGrid 
            pagePath={`/content/wknd-app/us/en/home/adventure/${adventureName}`}
            itemPath="root/responsivegrid"/>
            
        <h2>Itinerary</h2>
        ...
    )
}
```

`AdventureDetail.js`ファイルは次のようになります。

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## AEMでのコンテナの作成

`<AEMResponsiveGrid...>`を配置し、レンダリングされるアドベンチャーに基づいて動的に設定された`pagePath`を使用して、コンテンツをオーサリングします。

1. AEMオーサーにログインします。
1. __サイト/WKND App > us > en__&#x200B;に移動します。
1. ____ WKNDアプリのホ __ームページページの__ 編集
   + SPAの&#x200B;__Bali Surf Camp__&#x200B;ルートに移動して編集します。
1. 右上のモードセレクターから「__プレビュー__」を選択します。
1. SPAの&#x200B;__Bali Surf Camp__&#x200B;カードをタップして、ルートに移動します。
1. mode-selectorから「__編集__」を選択します。
1. __レイアウトコンテナ__&#x200B;編集可能な領域を&#x200B;__旅程__&#x200B;のすぐ上に見つけます。
1. __ページエディターのサイドバー__&#x200B;を開き、__コンポーネントビュー__&#x200B;を選択します。
1. 有効なコンポーネントの一部を&#x200B;__レイアウトコンテナ__&#x200B;にドラッグします。
   + 画像
   + テキスト
   + タイトル

   プロモーションマーケティング資料を作成します。 次のようになります。

   ![バリアドベンチャー詳細オーサリング](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. ____ AEMページエディターでの変更のプレビュー
1. [http://localhost:3000](http://localhost:3000)上でローカルで実行されているWKNDアプリを更新し、__Bali Surf Camp__&#x200B;ルートに移動して、作成した変更を確認します。

   ![リモートSPAバリ](./assets/spa-dynamic-routes/remote-spa-final.png)

マッピングされたAEM Pageを持たないアドベンチャー詳細ルートに移動する場合、そのルートインスタンスではオーサリング機能はありません。 これらのページでオーサリングを有効にするには、__Adventure__&#x200B;ページの下に、一致する名前のAEMページを作成します。

## バリデーターが

バリデーターがSPAで動的なルートにオーサリング機能が追加されました。

+ AEM React編集可能コンポーネントのResponsiveGridコンポーネントを動的ルートに追加しました。
+ SPA（Bali Surf CampとBeervana in Portland）での2つの特定のルートのオーサリングをサポートするAEMページを作成しました。
+ ダイナミックバリサーフキャンプルートのコンテンツを作成しました！

これで、AEM SPA Editorを使用して特定の編集可能領域をRemote SPAに追加する方法の最初の手順を学習しました。


>[!NOTE]
>
>気をつけろ！ このチュートリアルは、Adobeのベストプラクティスと、SPA EditorソリューションをAEM as aCloud Serviceおよび実稼動環境にデプロイする方法に関する推奨事項について説明するように拡張されます。