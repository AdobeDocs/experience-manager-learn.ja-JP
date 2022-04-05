---
title: リッチテキストの操作 | AEMヘッドレス
description: Adobe Experience Managerコンテンツフラグメントを使用した複数行のリッチテキストエディターを使用して、コンテンツの作成と参照コンテンツの埋め込みを行う方法、およびヘッドレスアプリケーションで使用する JSON としてAEM GraphQL API がリッチテキストを配信する方法を説明します。
version: Cloud Service
doc-type: article
kt: 9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
source-git-commit: 88797cf950dae46d0f856330df12c59a4efe6456
workflow-type: tm+mt
source-wordcount: '1380'
ht-degree: 0%

---


# Adobe Experience Manager Headless でのリッチテキストの使用

「複数行テキスト」フィールドは、作成者がリッチテキストコンテンツを作成できるようにするコンテンツフラグメントのデータ型です。 画像や他のコンテンツフラグメントなど、他のコンテンツへの参照を、テキストのフロー内にインラインで動的に挿入できます。 AEM GraphQL API は、リッチテキストをHTML、プレーンテキスト、純粋な JSON として返す堅牢な機能を備えています。 JSON 表現は、コンテンツのレンダリング方法をクライアントアプリケーションで完全に制御できるので、強力です。

## 複数行エディター

>[!VIDEO](https://video.tv.adobe.com/v/342104/?quality=12&learn=on)


コンテンツフラグメントエディターでは、複数行テキストフィールドのメニューバーを使用して、作成者に標準のリッチテキスト書式設定機能 ( **太字**, *斜体*、および下線 フルスクリーンモードで複数行フィールドを開くと、 [段落の種類、検索と置換、スペルチェックなどの追加の書式設定ツール](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html).

## 複数行テキストのデータタイプ {#multi-line-data-type}

以下を使用： **複数行テキスト** リッチテキストのオーサリングを有効にするためのデータタイプをコンテンツフラグメントモデルの定義時に追加します。

![複数行のリッチテキストデータタイプ](assets/rich-text/multi-line-rich-text.png)

複数行テキストデータタイプを使用する場合、 **デフォルトのタイプ** 移動先：

* リッチテキスト
* Markdown
* プレーンテキスト

この **デフォルトのタイプ** オプションは編集エクスペリエンスに直接影響し、リッチテキストツールが存在するかどうかを指定します。

また、 [インライン参照の有効化](#insert-fragment-references) を他のコンテンツフラグメントに追加する場合は、 **フラグメント参照を許可** と、 **許可されているコンテンツフラグメントモデル**.

## GraphQL API を使用したリッチテキストの応答

GraphQL クエリを作成する場合、開発者は次の中から異なる応答タイプを選択できます。 `html`, `plaintext`, `markdown`、および `json` 複数行フィールドから。

開発者は、 [JSON プレビュー](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html) コンテンツフラグメントエディターに表示され、GraphQL API を使用して返すことができる、現在のコンテンツフラグメントのすべての値が表示されます。

### JSON の例

この `json` の応答は、フロントエンド開発者がリッチテキストコンテンツを操作する際に最も柔軟に使用できます。 リッチテキストコンテンツは、クライアントプラットフォームに基づいて一意に処理できる JSON ノードタイプの配列として提供されます。

以下は、という名前の複数行フィールドの JSON 応答タイプです。 `main` 段落を含む&quot;*これは、**重要**コンテンツ。*&quot;ここで、&quot;重要&quot;は **太字**.

**GraphQL クエリ：**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        json
      }
    }
  }
}
```

**GraphQL の応答：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            }
          ]
        }
      }
    }
  }
}
```

### その他の例

次に、という複数行フィールドの応答タイプの例をいくつか示します。 `main` 段落を含む「これは **重要** 内容」 ここで、「重要」は **太字**.

+++HTML例

**GraphQL クエリ：**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        html
      }
    }
  }
}
```

**GraphQL の応答：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "html": "<p>This is a paragraph that includes <b>important</b> content.&nbsp;</p>\n"
        }
      }
    }
  }
}
```

+++

+++Markdown の例

**GraphQL クエリ：**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        markdown
      }
    }
  }
}
```

**GraphQL の応答：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "markdown": "This is a paragraph that includes **important** content. \n\n ",
        }
      }
    }
  }
}
```

+++

+++平文の例

**GraphQL クエリ：**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        plaintext
      }
    }
  }
}
```

**GraphQL の応答：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "plaintext": "This is a paragraph that includes important content. ",
        }
      }
    }
  }
}
```

この `plaintext` 「レンダリング」オプションを選択すると、すべての書式設定が削除されます。

+++


## リッチテキスト JSON 応答のレンダリング {#render-multiline-json-richtext}

複数行フィールドのリッチテキスト JSON 応答は、階層ツリーとして構造化されています。 各オブジェクトまたはノードは、リッチテキストの異なるHTMLブロックを表します。

以下は、複数行テキストフィールドの JSON 応答の例です。 各オブジェクト（ノード）に `nodeType` これは、次のようなリッチテキストのHTMLブロックを表します。 `paragraph`, `link`、および `text`. 各ノードには、オプションで `content` 現在のノードの子を含むサブ配列。

```json
"json": [// root "content" or child nodes
            {
                "nodeType": "paragraph", // node for a paragraph
                "content": [ // children of current node
                {
                    "nodeType": "text", // node for a text
                    "value": "This is the first paragraph. "
                },
                {
                    "nodeType": "link",
                    "data": {
                        "href": "http://www.adobe.com"
                    },
                    "value": "An external link"
                }
                ],
            },
            {
                "nodeType": "paragraph",
                "content": [
                {
                    "nodeType": "text",
                    "value": "This is the second paragraph."
                },
                ],
            },
]
```

マルチラインをレンダリングする最も簡単な方法 `json` 応答は、応答内の各オブジェクトまたはノードを処理し、現在のノードの子を処理します。 再帰関数を使用して、JSON ツリーをトラバースできます。

次に、再帰的なトラバーサルアプローチを示すサンプルコードを示します。 サンプルは JavaScript ベースで、React の [JSX](https://reactjs.org/docs/introducing-jsx.html)ただし、プログラミングの概念はどの言語にも適用できます。

```javascript
// renderNodeList - renders a list of nodes
function renderNodeList(childNodes) {
    
    if(!childNodes) {
        // null check
        return null;
    }

    return childNodes.map(node, index) => {
        return renderNode(node);
    }
}
```

この `renderNodeList` 関数は、再帰的なアルゴリズムへのエントリポイントです。 この `renderNodeList` 関数は、次の配列を想定します。 `childNodes`. 次に、配列内の各ノードが関数に渡されます `renderNode`.

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

この `renderNode` 関数は、 `node`. ノードには、 `renderNodeList` 関数に関する情報が含まれます。 最後に、 `nodeMap` を使用して、ノードのコンテンツを、 `nodeType`.

```javascript
// nodeMap - object literal that maps a JSX response based on a given key (nodeType)
const nodeMap = {
    'paragraph': (node, children) => <p>{children}</p>,
    'link': node => <a href={node.data.href} target={node.data.target}>{node.value}</a>,
    'text': node => node.value,
    'unordered-list': (node, children) => <ul>{children}</ul>,
    'ordered-list': (node, children) => <ol>{children}</ol>,
    'list-item': (node, children) => <li>{children}</li>,
    ...
}
```

この `nodeMap` は、マップとして使用される JavaScript オブジェクトリテラルです。 各「キー」は、異なる `nodeType`. のパラメーター `node` および `children` は、ノードをレンダリングする結果の関数に渡すことができます。 この例で使用される戻り値のタイプは JSX ですが、HTMLの内容を表す文字列リテラルを構築するように適応させることもできます。

### フルコードの例

再利用可能なリッチテキストレンダリングユーティリティは、 [WKND GraphQL React の例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app/src/utils/renderRichText.js)  — 関数を公開する再利用可能なユーティリティ `mapJsonRichText`. このユーティリティは、リッチテキスト JSON 応答を React JSX としてレンダリングするコンポーネントで使用できます。
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js)  — リッチテキストを含む GraphQL リクエストを作成するサンプルコンポーネント。 このコンポーネントは、 `mapJsonRichText` リッチテキストおよび参照をレンダリングするユーティリティ。


## リッチテキストへのインライン参照の追加 {#insert-fragment-references}

「複数行」フィールドを使用すると、作成者は、リッチテキストのフローにAEM Assetsから画像やその他のデジタルアセットを挿入できます。

![画像を挿入](assets/rich-text/insert-image.png)

上のスクリーンショットは、複数行フィールドに、 **アセットを挿入** 」ボタンをクリックします。

他のコンテンツフラグメントへの参照は、 **コンテンツフラグメントを挿入** 」ボタンをクリックします。

![コンテンツフラグメント参照を挿入](assets/rich-text/insert-contentfragment.png)

上のスクリーンショットは、別のコンテンツフラグメント、「 Ultimate Guide to LA Skate Parks（LA スケートパークの究極のガイド）」を、複数行フィールドに挿入したものです。 フィールドに挿入できるコンテンツフラグメントのタイプは、 **許可されているコンテンツフラグメントモデル** 設定 [複数行のデータタイプ](#multi-line-data-type) 」と入力します。

## GraphQL を使用したクエリインライン参照

GraphQL API を使用すると、開発者は複数行フィールドに挿入された参照に関する追加のプロパティを含むクエリを作成できます。 JSON 応答には、別の `_references` これらの追加のプロパティをリストするオブジェクト。 JSON 応答を使用すると、開発者は、固有のHTMLに対処する必要がなく、参照やリンクのレンダリング方法を完全に制御できます。

例えば、次のような場合に役立ちます。

* React Router や Next.js の使用など、シングルページアプリケーションを実装する際に他のコンテンツフラグメントへのリンクを管理するカスタムルーティングロジックを含めます
* AEM パブリッシュ環境への絶対パスを `src` の値です。
* 別のコンテンツフラグメントへの埋め込み参照を、追加のカスタムプロパティと共にレンダリングする方法を決定します。

以下を使用： `json` 戻り値の型と `_references` オブジェクトを次のように指定します。

**GraphQL クエリ：**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        json
      }
    }
    _references {
      ...on ImageRef {
        _path
        _publishUrl
        width
      }
      ...on ArticleModel {
        _path
        author
      }
      
    }
  }
}
```

上記のクエリでは、 `main` フィールドが JSON として返されます。 この `_references` オブジェクトには、型の参照を処理するフラグメントが含まれます `ImageRef` またはタイプ `ArticleModel`.

**JSON 応答：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "reference",
                  "data": {
                    "path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
                    "mimetype": "image/jpeg"
                  }
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "Reference another Content Fragment: "
                },
                {
                  "nodeType": "reference",
                  "data": {
                    "href": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
                    "type": "fragment"
                  },
                  "value": "Ultimate Guide to LA Skateparks"
                }
              ]
            }
          ]
        }
      },
      "_references": [
        {
          "_path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
          "_publishUrl": "http://localhost:4503/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
          "width": 1920
        },
        {
          "_path": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
          "author": "Stacey Roswells",
        }
      ]
    }
  }
}
```

JSON 応答には、参照がリッチテキスト内の `"nodeType": "reference"`. この `_references` オブジェクトには、各参照と、リクエストされた追加のプロパティが含まれます。 例えば、 `ImageRef` は、 `width` 」という名前に変更されます。

## リッチテキストでのインライン参照のレンダリング

インライン参照をレンダリングするには、 [複数行の JSON 応答のレンダリング](#render-multiline-json-richtext) を展開できます。

ここで、 `nodeMap` は、JSON ノードをレンダリングするマップです。

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if(node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if(node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

高レベルのアプローチは、 `nodeType` 次と等しい `reference` （複数行の JSON 応答）を参照してください。 次に、カスタムレンダリング関数を呼び出し、 `_references` オブジェクトが GraphQL 応答で返されました。

次に、インライン参照パスを `_references` オブジェクトと別のカスタムマップ `renderReference` を呼び出すことができます。

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._path} alt={'in-line reference'} /> 
    },
    'AdventureModel': (node) => {
        // when __typename === AdventureModel
        return <Link to={`/adventure:${node._path}`}>{`${node.adventureTitle}: ${node.adventurePrice}`}</Link>;
    }
    ...
}
```

この `__typename` の `_references` オブジェクトを使用して、異なる参照タイプを異なるレンダリング関数にマッピングできます。

### フルコードの例

カスタム参照レンダラーの記述の完全な例については、 [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) の一部として [WKND GraphQL React の例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## エンドツーエンドの例

>[!VIDEO](https://video.tv.adobe.com/v/342105/?quality=12&learn=on)

前述のビデオでは、エンドツーエンドの例を示します。

1. コンテンツフラグメントモデルの複数行テキストフィールドを更新して、フラグメント参照を許可する
1. コンテンツフラグメントエディターを使用して、画像と別のフラグメントへの参照を複数行テキストフィールドに含めます。
1. 複数行のテキストの応答を JSON として含む GraphQL クエリの作成 `_references` 使用済み
1. リッチテキスト応答のインライン参照をレンダリングする React SPAを記述する。
