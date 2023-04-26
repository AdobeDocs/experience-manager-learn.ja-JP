---
title: AEMヘッドレスでのリッチテキストの使用
description: Adobe Experience Managerコンテンツフラグメントを使用した複数行のリッチテキストエディターを使用して、コンテンツを作成し参照コンテンツを埋め込む方法、およびヘッドレスアプリケーションで使用される JSON としてAEM GraphQL API がリッチテキストを配信する方法について説明します。
version: Cloud Service
doc-type: article
kt: 9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
source-git-commit: 117b67bd185ce5af9c83bd0c343010fab6cd0982
workflow-type: tm+mt
source-wordcount: '1465'
ht-degree: 0%

---

# AEMヘッドレスを含むリッチテキスト

複数行テキストフィールドは、作成者がリッチテキストコンテンツを作成できるコンテンツフラグメントのデータ型です。 画像や他のコンテンツフラグメントなど、他のコンテンツへの参照を、テキストのフロー内にインラインで動的に挿入できます。 「 1 行テキスト」フィールドは、単純なテキスト要素に使用するコンテンツフラグメントの別のデータ型です。

AEM GraphQL API は、リッチテキストをHTML、プレーンテキストまたは純粋な JSON として返す堅牢な機能を備えています。 JSON 表現は、コンテンツのレンダリング方法をクライアントアプリケーションで完全に制御できるので、強力です。

## 複数行エディター

>[!VIDEO](https://video.tv.adobe.com/v/342104?quality=12&learn=on)

コンテンツフラグメントエディターでは、複数行テキストフィールドのメニューバーを使用して、標準のリッチテキスト書式設定機能 ( **太字**, *斜体*、および下線 複数行フィールドをフルスクリーンモードで開くと、 [段落の種類、検索と置換、スペルチェックなどの追加の書式設定ツール](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html?lang=ja).

>[!NOTE]
>
> 複数行エディターのリッチテキストプラグインはカスタマイズできません。

## 複数行テキストのデータタイプ {#multi-line-data-type}

以下を使用： **複数行テキスト** リッチテキストのオーサリングを有効にするためのデータタイプをコンテンツフラグメントモデルの定義時に追加します。

![複数行のリッチテキストデータタイプ](assets/rich-text/multi-line-rich-text.png)

複数行フィールドの複数のプロパティを設定できます。

この **レンダリング形式** プロパティは次の値に設定できます。

* テキスト領域 — 1 つの複数行フィールドをレンダリングします。
* 複数のフィールド — 複数の複数行フィールドをレンダリングします


この **デフォルトのタイプ** は次のように設定できます。

* リッチテキスト
* Markdown
* プレーンテキスト

この **デフォルトのタイプ** オプションは編集エクスペリエンスに直接影響し、リッチテキストツールが存在するかどうかを指定します。

また、 [インライン参照の有効化](#insert-fragment-references) を他のコンテンツフラグメントに追加する場合は、 **フラグメント参照を許可** と、 **許可されているコンテンツフラグメントモデル**.

次を確認します。 **翻訳可能** 」ボックスに表示されます。 ローカライズできるのは、「リッチテキスト」と「プレーンテキスト」のみです。 詳しくは、 [ローカライズされたコンテンツの使用を参照](./localized-content.md).

## GraphQL API を使用したリッチテキスト応答

GraphQLクエリを作成する場合、開発者は次の中から異なる応答タイプを選択できます。 `html`, `plaintext`, `markdown`、および `json` 複数行フィールドから。

開発者は、 [JSON プレビュー](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html) コンテンツフラグメントエディターに表示され、現在のコンテンツフラグメントのすべての値 (GraphQL API を使用して返すことができる値 ) を確認できます。

## GraphQL永続クエリ

の選択 `json` 複数行フィールドの応答形式は、リッチテキストコンテンツを操作する際の最も柔軟性の高い形式です。 リッチテキストコンテンツは、クライアントプラットフォームに基づいて一意に処理できる JSON ノードタイプの配列として提供されます。

以下は、という名前の複数行フィールドの JSON 応答タイプです。 `main` 段落を含む&quot;*これは、**重要**コンテンツ。*&quot;ここで、&quot;重要&quot;は **太字**.

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
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

この `$path` 変数 `_path` フィルターにはコンテンツフラグメントへの完全パスが必要です ( 例： `/content/dam/wknd/en/magazine/sample-article`) をクリックします。

**GraphQLの応答：**

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

**GraphQLで保持されたクエリ：**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
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

**GraphQLの応答：**

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

**GraphQLで保持されたクエリ：**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
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

**GraphQLの応答：**

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

**GraphQLで保持されたクエリ：**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
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

**GraphQLの応答：**

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

複数行フィールドのリッチテキスト JSON 応答は、階層ツリーとして構造化されます。 各オブジェクトまたはノードは、リッチテキストの異なるHTMLブロックを表します。

次に、複数行テキストフィールドの JSON 応答の例を示します。 各オブジェクト（ノード）に `nodeType` これは、次のようなリッチテキストのHTMLブロックを表します。 `paragraph`, `link`、および `text`. 各ノードには、オプションで `content` 現在のノードの子を含むサブ配列。

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

複数行をレンダリングする最も簡単な方法 `json` 応答は、応答内の各オブジェクトまたはノードを処理し、現在のノードの子を処理します。 再帰関数を使用して、JSON ツリーをトラバースできます。

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

`renderNodeList` は、 `childNodes`. 次に、配列内の各ノードが関数に渡されます `renderNode`を呼び出します。 `renderNodeList` （ノードに子がある場合）

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

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js)  — 関数を公開する再利用可能なユーティリティ `mapJsonRichText`. このユーティリティは、リッチテキスト JSON 応答を React JSX としてレンダリングするコンポーネントで使用できます。
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js)  — リッチテキストを含むGraphQLリクエストを作成するサンプルコンポーネント。 このコンポーネントは、 `mapJsonRichText` リッチテキストおよび参照をレンダリングするユーティリティ。


## リッチテキストへのインライン参照の追加 {#insert-fragment-references}

「複数行」フィールドを使用すると、作成者は、リッチテキストのフロー内にAEM Assetsから画像やその他のデジタルアセットを挿入できます。

![画像を挿入](assets/rich-text/insert-image.png)

上のスクリーンショットは、複数行フィールドに、 **アセットを挿入** 」ボタンをクリックします。

他のコンテンツフラグメントへの参照は、 **コンテンツフラグメントを挿入** 」ボタンをクリックします。

![コンテンツフラグメント参照を挿入](assets/rich-text/insert-contentfragment.png)

上のスクリーンショットは、別のコンテンツフラグメント、「 Ultimate Guide to LA Skate Parks（LA スケートパークの究極のガイド）」を、複数行フィールドに挿入したものです。 フィールドに挿入できるコンテンツフラグメントのタイプは、 **許可されているコンテンツフラグメントモデル** 設定 [複数行データタイプ](#multi-line-data-type) 」と入力します。

## GraphQLでのインライン参照のクエリ

GraphQL API を使用すると、開発者は、複数行フィールドに挿入された参照に関する追加のプロパティを含むクエリを作成できます。 JSON 応答には、別の `_references` これらの追加のプロパティをリストするオブジェクト。 JSON 応答を使用すると、開発者は、固有のHTMLに対処する必要がなく、参照やリンクのレンダリング方法を完全に制御できます。

例えば、次のような場合に役立ちます。

* React Router や Next.js の使用など、シングルページアプリケーションを実装する際に他のコンテンツフラグメントへのリンクを管理するカスタムルーティングロジックを含めます
* AEM パブリッシュ環境への絶対パスを `src` の値です。
* 別のコンテンツフラグメントへの埋め込み参照を、追加のカスタムプロパティと共にレンダリングする方法を決定します。

以下を使用： `json` 戻り値の型と `_references` オブジェクトは、GraphQLクエリを構築する際に次のようになります。

**GraphQLで保持されたクエリ：**

```graphql
query ($path: String!) {
  articleByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true })
  {
    item {
      _path
      main {
        json
      }
    }
    _references {
      ...on ImageRef {
        _dynamicUrl
        __typename
      }
      ...on ArticleModel {
        _path
        author
        __typename
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
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/sport-climbing.jpg?preferwebp=true",
          "__typename": "ImageRef"
        },
        {
          "_path": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
          "author": "Stacey Roswells",
          "__typename": "ArticleModel"
        }
      ]
    }
  }
}
```

JSON 応答には、参照がリッチテキスト内の `"nodeType": "reference"`. この `_references` オブジェクトには、各参照が含まれます。

## リッチテキストでのインライン参照のレンダリング

インライン参照をレンダリングするには、 [複数行の JSON 応答のレンダリング](#render-multiline-json-richtext) を展開できます。

ここで、 `nodeMap` は、JSON ノードをレンダリングするマップです。

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if (node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if (node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

高レベルのアプローチは、 `nodeType` 次と等しい `reference` （複数行の JSON 応答）を参照してください。 次に、カスタムレンダリング関数を呼び出し、 `_references` オブジェクトがGraphQL応答で返されました。

次に、インライン参照パスを `_references` オブジェクトと別のカスタムマップ `renderReference` を呼び出すことができます。

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._dynamicUrl} alt={'in-line reference'} /> 
    },
    'ArticleModel': (node) => {
        // when __typename === ArticleModel
        return <Link to={`/article:${node._path}`}>{`${node.value}`}</Link>;
    }
    ...
}
```

この `__typename` の `_references` オブジェクトを使用して、異なる参照タイプを異なるレンダリング関数にマッピングできます。

### フルコードの例

カスタム参照レンダラーの記述の完全な例については、 [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) の一部として [WKND GraphQL React の例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## エンドツーエンドの例

>[!VIDEO](https://video.tv.adobe.com/v/342105?quality=12&learn=on)

>[!NOTE]
>
> 上記のビデオでは、 `_publishUrl` をクリックして、イメージリファレンスをレンダリングします。 代わりに、 `_dynamicUrl` 詳しくは、 [web に最適化された画像のハウツー](./images.md);


前述のビデオでは、エンドツーエンドの例を示します。

1. コンテンツフラグメントモデルの複数行テキストフィールドを更新して、フラグメント参照を許可する
2. コンテンツフラグメントエディターを使用して、複数行テキストフィールドに画像と別のフラグメントへの参照を含めます。
3. 複数行テキストの応答を JSON として含むGraphQLクエリの作成 `_references` 使用済み
4. リッチテキスト応答のインライン参照をレンダリングする React SPAを記述する。
