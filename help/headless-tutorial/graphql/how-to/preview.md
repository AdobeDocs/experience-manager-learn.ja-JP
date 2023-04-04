---
title: コンテンツフラグメントのプレビュー
description: すべての作成者に対してコンテンツフラグメントプレビューを使用して、コンテンツの変更がAEMヘッドレスエクスペリエンスに与える影響をすばやく確認する方法を説明します。
version: Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---


# コンテンツフラグメントのプレビュー

AEMヘッドレスアプリケーションは、統合オーサリングプレビューをサポートします。 プレビューエクスペリエンスは、AEM オーサーのコンテンツフラグメントエディターとカスタムアプリ（HTTP 経由でアドレス可能）をリンクさせ、プレビュー中のコンテンツフラグメントをレンダリングするアプリへのディープリンクを可能にします。

>[!VIDEO](https://video.tv.adobe.com/v/3416906?quality=12&learn=on)

コンテンツフラグメントのプレビューを使用するには、次の条件を満たす必要があります。

1. 作成者がアクセス可能な URL にアプリをデプロイする必要があります
1. アプリが（AEM パブリッシュサービスではなく）AEM オーサーサービスに接続するように設定されている必要があります
1. アプリは、使用可能な URL またはルートを使用して設計される必要があります [コンテンツフラグメントのパスまたは ID](#url-expressions) をクリックして、アプリエクスペリエンスでプレビュー用に表示するコンテンツフラグメントを選択します。

## プレビュー URL

プレビュー URL、使用 [URL 式](#url-expressions)は、コンテンツフラグメントモデルのプロパティに設定されます。

![コンテンツフラグメントモデルプレビュー URL](./assets/preview/cf-model-preview-url.png)

1. 管理者として AEM オーサーサービスにログインします。
1. に移動します。 __ツール/一般/コンテンツフラグメントモデル__
1. を選択します。 __コンテンツフラグメントモデル__ を選択し、 __プロパティ__ 上部のアクションバーを作成します。
1. 次を使用してコンテンツフラグメントモデルのプレビュー URL を入力 [URL 式](#url-expressions)
   + プレビュー URL は、AEM オーサーサービスに接続するアプリのデプロイメントを指している必要があります。

### URL 式

各コンテンツフラグメントモデルには、プレビュー URL を設定できます。 プレビュー URL は、コンテンツフラグメントごとに、次の表に示す URL 式を使用してパラメーター化できます。 1 つのプレビュー URL で複数の URL 式を使用できます。

|  | URL 式 | 値 |
| --------------------------------------- | ----------------------------------- | ----------- |
| コンテンツフラグメントのパス | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| コンテンツフラグメント ID | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| コンテンツフラグメントのバリエーション | `${contentFragment.variation}` | `main` |
| コンテンツフラグメントモデルのパス | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| コンテンツフラグメントモデル名 | `${contentFragment.model.name}` | `adventure` |

プレビュー URL の例：

+ プレビュー URL __冒険__ モデルは次のようになります `https://preview.app.wknd.site/adventure${contentFragment.path}` が `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali`
+ プレビュー URL __記事__ モデルは次のようになります `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` 解決された `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`

## アプリ内プレビュー

設定済みのコンテンツフラグメントモデルを使用するコンテンツフラグメントには、「プレビュー」ボタンが付きます。 「プレビュー」ボタンをクリックすると、コンテンツフラグメントモデルのプレビュー URL が開き、開いているコンテンツフラグメントの値が [URL 式](#url-expressions).

![「プレビュー」ボタン](./assets/preview/preview-button.png)

アプリでコンテンツフラグメントの変更をプレビューする際に、ハード更新（ブラウザーのローカルキャッシュをクリア）を実行します。

## React の例

AEMヘッドレスGraphQL API を使用してAEMのアドベンチャーを表示するシンプルな React アプリ WKND アプリを見てみましょう。

サンプルコードは、 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-tutorial).

## URL とルート

コンテンツフラグメントのプレビューに使用する URL またはルートは、 [URL 式](#url-expressions). このプレビュー対応バージョンの WKND アプリでは、アドベンチャーコンテンツフラグメントが `AdventureDetail` ルートにバインドされたコンポーネント `/adventure<CONTENT FRAGMENT PATH>`. したがって、WKND アドベンチャーモデルのプレビュー URL は、 `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` このルートを解決するには。

コンテンツフラグメントのプレビューは、 [URL 式](#url-expressions) プレビュー可能な方法でアプリ内でそのコンテンツフラグメントをレンダリングする

+ `src/App.js`

```javascript
...
function App() {
  return (
    <Router>
      <div className="App">
        <header>
            <Link to={"/"}>
                <img src={logo} className="logo" alt="WKND Logo"/>
            </Link>        
            <hr />
        </header>
        <Switch>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*'>
            <AdventureDetail />
          </Route>
          <Route path="/">
            <Home />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}

export default App;
```

### 作成したコンテンツを表示

この `AdventureDetail` コンポーネントは、コンテンツフラグメントパスを解析し、 `${contentFragment.path}` [URL 式](#url-expressions)をルート URL から取得し、それを使用して WKND アドベンチャーを収集してレンダリングします。

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the content fragment path value which is the parameter used to query for the adventure's details
    
    // Add the leading '/' back on since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const path = '/' + useParams()[0];

    // Query AEM for the Adventures's details, using the Content Fragment's `path`
    const { adventure, references, error } = useAdventureByPath(path);

    // Handle error and loading conditions
    if (error) {
        return <Error errorMessage={error} />;
    } else if (!adventure) {
        return <Loading />;
    }

    return (<div className="adventure-detail">
        ...
        <AdventureDetailRender {...adventure} references={references} />
    </div>);
}
...
```
