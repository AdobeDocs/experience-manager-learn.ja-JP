---
title: 編集可能なコンポーネントをリモートSPA Dynamic Routes に追加
description: リモートSPAの動的ルートに編集可能なコンポーネントを追加する方法を説明します。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '916'
ht-degree: 1%

---

# 動的ルートと編集可能コンポーネント

この章では、2 つの動的なアドベンチャー詳細ルートを有効にして、編集可能なコンポーネントをサポートします。 __バリサーフキャンプ__ および __Beervana in Portland__.

![動的ルートと編集可能コンポーネント](./assets/spa-dynamic-routes/intro.png)

アドベンチャー詳細SPAルートは、次のように定義されます。 `/adventure:path` 場所 `path` は、詳細を表示する WKND アドベンチャー（コンテンツフラグメント）へのパスです。

## SPA URL のAEM Pages へのマッピング

前の 2 つの章では、編集可能なコンポーネントのコンテンツをSPAのホームビューからAEMの対応するリモートSPAのルートページ ( ) にマッピングしました。 `/content/wknd-app/us/en/`.

SPA動的ルートの編集可能なコンポーネントのマッピングの定義は似ていますが、ルートのインスタンスとAEMページの間で 1 対 1 のマッピングスキームを作成する必要があります。

このチュートリアルでは、パスの最後のセグメントである WKND Adventure コンテンツフラグメントの名前を使用し、次の単純なパスにマッピングします。 `/content/wknd-app/us/en/adventure`.

| リモートSPAルート | AEMページパス |
|------------------------------------|--------------------------------------------|
| ／ | /content/wknd-app/us/en/home |
| /adventure:/content/dam/wknd/en/adventures/bali-surf-camp/__バリサーフキャンプ__ | /content/wknd-app/us/en/home/adventure/__バリサーフキャンプ__ |
| /adventure:/content/dam/wknd/en/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

したがって、このマッピングに基づいて、次の場所に 2 つの新しいAEMページを作成する必要があります。

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## リモートSPAマッピング

リモートSPAからの要求のマッピングは、 `setupProxy` 設定が完了しました： [BootstrapSPA](./spa-bootstrap.md).

## SPA Editor マッピング

SPAがAEM SPA Editor で開かれたときのSPA要求のマッピングは、でおこなわれた Sling マッピング設定を使用して設定されます。 [AEMを設定](./aem-configure.md).

## AEMでのコンテンツページの作成

まず、仲介者を作成します。 `adventure` ページセグメント：

1. AEM オーサーにログインします。
1. に移動します。 __サイト/ WKND アプリ/米国/英語/ WKND アプリのホームページ__
   + このAEMページはSPAのルートとしてマッピングされるので、他のSPAルート用のAEMページ構造の構築を開始する場所になります。
1. タップ __作成__ を選択し、 __ページ__
1. を選択します。 __リモートSPAページ__ テンプレートをタップし、 __次へ__
1. ページプロパティの入力
   + __タイトル__:冒険
   + __名前__：`adventure`
      + この値はAEMページの URL を定義するので、SPAのルートセグメントと一致する必要があります。
1. タップ __完了__

次に、編集可能な領域が必要な各SPA URL に対応するAEMページを作成します。

1. 新しい __冒険__ ページ（サイト管理）
1. タップ __作成__ を選択し、 __ページ__
1. を選択します。 __リモートSPAページ__ テンプレートをタップし、 __次へ__
1. ページプロパティの入力
   + __タイトル__:バリサーフキャンプ
   + __名前__：`bali-surf-camp`
      + この値はAEMページの URL を定義するので、SPAのルートの最後のセグメントと一致する必要があります
1. タップ __完了__
1. 手順 3 ～ 6 を繰り返して、 __Beervana in Portland__ ページ、次を含む：
   + __タイトル__:Beervana in Portland
   + __名前__：`beervana-in-portland`
      + この値はAEMページの URL を定義するので、SPAのルートの最後のセグメントと一致する必要があります

これら 2 つのAEMページには、一致するSPAルート用にそれぞれオーサリングされたコンテンツが格納されます。 他のSPAルートでオーサリングが必要な場合は、新しいAEMページを、Remote SPAページのルートページ (`/content/wknd-app/us/en/home`) をAEMでクリックします。

## WKND アプリの更新

次に `<AEMResponsiveGrid...>` で作成されたコンポーネント [最後の章](./spa-container-component.md)を `AdventureDetail` SPAコンポーネント、編集可能なコンテナの作成

### AEMResponsiveGrid SPAコンポーネントの配置

を `<AEMResponsiveGrid...>` 内 `AdventureDetail` コンポーネントは、そのルートに編集可能なコンテナを作成します。 これは、複数のルートが `AdventureDetail` レンダリングするコンポーネントは、動的に調整する必要があります  `<AEMResponsiveGrid...>'s pagePath` 属性。 この `pagePath` は、ルートのインスタンスに表示されるアドベンチャーに基づいて、対応するAEMページを指すように派生する必要があります。

1. 開いて編集 `react-app/src/components/AdventureDetail.js`
1. の前に次の行を追加します。 `AdventureDetail(..)'s` 秒 `return(..)` コンテンツフラグメントのパスからアドベンチャー名を派生するステートメント。

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = _path.split('/').pop();
   ...
   ```

1. 次をインポート： `AEMResponsiveGrid` コンポーネントを選択し、その上に配置します。 `<h2>Itinerary</h2>` コンポーネント。
1. 次の属性を `<AEMResponsiveGrid...>` コンポーネント
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   これは、 `AEMResponsiveGrid` AEMリソースからコンテンツを取得するコンポーネント：

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


更新 `AdventureDetail.js` を次の行に置き換えます。

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = _path.split('/').pop();

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

この `AdventureDetail.js` ファイルは次のようになります。

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## AEMでのコンテナの作成

を使用 `<AEMResponsiveGrid...>` その場で `pagePath` レンダリングされるアドベンチャーに基づいて動的に設定されるコンテンツのオーサリングを試みます。

1. AEM オーサーにログインします。
1. に移動します。 __サイト/ WKND アプリ/米国/en__
1. __編集__ の __WKND アプリのホームページ__ ページ
   + 次に移動： __バリサーフキャンプ__ SPAで編集するルート
1. 選択 __プレビュー__ 右上のモードセレクターから
1. をタップします。 __バリサーフキャンプ__ ルートに移動するSPAのカード
1. 選択 __編集__ mode-selector から
1. を __レイアウトコンテナ__ 右上の編集可能な領域 __旅程__
1. を開きます。 __ページエディターのサイドバー__&#x200B;をクリックし、 __コンポーネント表示__
1. 有効なコンポーネントの一部を __レイアウトコンテナ__
   + 画像
   + テキスト
   + タイトル

   プロモーションマーケティング資料を作成します。 次のようになります。

   ![Bali Adventure Detail Authoring](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __プレビュー__ AEM Page Editor での変更
1. ローカルで実行している WKND アプリを更新 [http://localhost:3000](http://localhost:3000)をクリックし、 __バリサーフキャンプ__ 作成した変更を確認するルートです。

   ![リモートSPA Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

マッピングされたAEMページを持たないアドベンチャー詳細ルートに移動する場合、そのルートインスタンスではオーサリング機能はありません。 これらのページでのオーサリングを有効にするには、対応する名前を持つAEMページを __冒険__ ページ！

## おめでとうございます。

おめでとうございます。オーサリング機能がSPAで動的ルートに追加されました。

+ AEM React 編集可能コンポーネントの ResponsiveGrid コンポーネントを動的ルートに追加しました。
+ SPA（Bali Surf Camp と Beervana in Portland）での 2 つの特定のルートのオーサリングをサポートするAEMページを作成しました。
+ ダイナミックバリサーフキャンプルートのコンテンツを作成しました！

AEM SPA Editor を使用して、特定の編集可能な領域を Remote SPAに追加する方法の最初の手順を完了しました。
