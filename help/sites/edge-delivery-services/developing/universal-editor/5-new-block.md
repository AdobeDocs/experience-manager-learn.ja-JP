---
title: ブロックの作成
description: ユニバーサルエディターで編集できる Edge Delivery Services の web サイト用のブロックを作成します。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 9698c17a-0ac8-426d-bccb-729b048cabd1
source-git-commit: 775821f37df87905ea176b11ecf0ed4a42d00940
workflow-type: ht
source-wordcount: '1742'
ht-degree: 100%

---

# 新しいブロックの作成

この章では、ユニバーサルエディターを使用して、Edge Delivery Services web サイト用の新しい編集できるティーザーブロックを作成するプロセスについて説明します。

![新しいティーザーブロック](./assets//5-new-block/teaser-block.png)

`teaser` という名前のブロックでは、次の要素を示します。

- **画像**：視覚的に魅力的な画像。
- **テキストコンテンツ**：
   - **タイトル**：注目を集める説得力のある見出し。
   - **本文**：オプションの利用条件を含む、コンテキストや詳細を提供する説明的なコンテンツ。
   - **コールトゥアクション（CTA）ボタン**：ユーザーのインタラクションを促し、さらなるエンゲージメントを促すように設計されたリンク。

`teaser` ブロックのコンテンツはユニバーサルエディターで編集できるので、web サイト全体での使いやすさと再利用性が確保されます。

`teaser` ブロックはボイラープレートの `hero` ブロックに似ています。したがって、`teaser` ブロックは開発の概念を説明する簡単な例としてのみ使用されることを目的としています。

## 新しい Git 分岐の作成

クリーンで組織化されたワークフローを維持するには、特定の開発タスクごとに新しい分岐を作成します。これにより、不完全なコードやテストされていないコードを実稼動環境にデプロイする際に発生する問題を回避できます。

1. **メイン分岐から開始**：最新の実稼動コードから作業することで、強固な基盤が確保されます。
2. **リモート変更を取得**：GitHub から最新の更新を取得すると、開発を開始する前に最新のコードが使用できます。
   - 例：`wknd-styles` 分岐からの変更を `main` に結合した後、最新の更新を取得します。
3. **新しい分岐を作成**：

```bash
# ~/Code/aem-wknd-eds-ue

$ git fetch origin  
$ git checkout -b teaser origin/main  
```

`teaser` 分岐を作成すると、ティーザーブロックの開発を開始する準備が整います。

## ブロックのフォルダー

プロジェクトの `blocks` ディレクトリに `teaser` という名前の新しいフォルダーを作成します。このフォルダーには、ブロックの JSON、CSS、JavaScript ファイルが含まれており、ブロックのファイルが 1 つの場所に整理されます。

```
# ~/Code/aem-wknd-eds-ue

/blocks/teaser
```

ブロックフォルダー名は、ブロックの ID として機能し、開発全体を通じてブロックを参照するのに使用されます。

## ブロックの JSON

ブロックの JSON は、ブロックの次の 3 つの主要な側面を定義します。

- **定義**：ブロックをユニバーサルエディターで編集できるコンポーネントとして登録し、ブロックモデルおよびオプションでフィルターにリンクします。
- **モデル**：ブロックのオーサリングフィールドと、これらのフィールドがセマンティック Edge Delivery Services HTML としてレンダリングされる方法を指定します。
- **フィルター**：ユニバーサルエディター経由でブロックを追加できるコンテナを制限するフィルタリングルールを設定します。ほとんどのブロックはコンテナではありませんが、その ID は他のコンテナブロックのフィルターに追加されます。

次の初期構造を正確な順序で使用して、`/blocks/teaser/_teaser.json` に新しいファイルを作成します。キーの順序が正しくない場合、正しく作成されない場合があります。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```json
{
    "definitions": [],
    "models": [],
    "filters": []
}
```

### ブロックモデル

ブロックモデルは、以下を定義するので、ブロックの設定の重要な部分です。

1. 編集できるフィールドを定義することによるオーサリングエクスペリエンス。

   ![ユニバーサルエディターのフィールド](./assets/5-new-block/fields-in-universal-editor.png)

2. フィールドの値を Edge Delivery Services HTML にレンダリングする方法。

モデルには、[ブロックの定義](#block-definition)に対応する `id` が割り当てられ、編集できるフィールドを指定する `fields` 配列が含まれます。

`fields` 配列の各フィールドには、次の必須プロパティを含む JSON オブジェクトがあります。

| JSON プロパティ | 説明 |
|---------------|-----------------------------------------------------------------------------------------------------------------------|
| `component` | `text`、`reference`、`aem-content` などの[フィールドタイプ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/field-types#component-types)。 |
| `name` | AEM で値が保存される JCR プロパティにマッピングされるフィールドの名前。 |
| `label` | ユニバーサルエディターで作成者に表示されるラベル。 |

オプションを含むプロパティの包括的なリストについて詳しくは、[ユニバーサルエディターのフィールド](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/field-types#fields)のドキュメントを参照してください。

#### ブロックデザイン

![ティーザーブロック](./assets/5-new-block/block-design.png)

ティーザーブロックには、次の編集できる要素が含まれます。

1. **画像**：ティーザーの視覚的なコンテンツを表します。
2. **テキストコンテンツ**：タイトル、本文、コールトゥアクションボタンが含まれ、白色の長方形内に配置されます。
   - **タイトル**&#x200B;と&#x200B;**本文**&#x200B;は、同じリッチテキストエディターを使用して作成できます。
   - **CTA** は、**ラベル**&#x200B;の「`text`」フィールドと&#x200B;**リンク**&#x200B;の「`aem-content`」フィールドを使用して作成できます。

ティーザーブロックのデザインは、これら 2 つの論理コンポーネント（画像とテキストコンテンツ）に分割され、構造化された直感的なオーサリングエクスペリエンスをユーザーに提供します。

### ブロックのフィールド

ブロックに必要なフィールド（画像、画像の代替テキスト、テキスト、CTA ラベル、CTA リンク）を定義します。

>[!BEGINTABS]

>[!TAB 正しい方法]

**このタブでは、ティーザーブロックをモデル化する正しい方法について説明します。**

ティーザーは、画像とテキストという 2 つの論理領域で構成されます。Edge Delivery Services HTML を目的の web エクスペリエンスとして表示するのに必要なコードを簡素化するには、ブロックモデルでこの構造を反映する必要があります。

- [フィールドの折りたたみ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)を使用して、**画像**&#x200B;と&#x200B;**画像の代替テキスト**&#x200B;をグループ化します。
- [要素のグループ化](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping)と [CTA のフィールドの折りたたみ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)を使用して、テキストコンテンツフィールドをグループ化します。

[フィールドの折りたたみ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)、[要素のグループ化](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping)、または[型推論](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference)に慣れていない場合は、適切に構造化されたブロックモデルを作成するのに不可欠であるので、続行する前にリンクされているドキュメントを確認します。

以下に例を示します。

- [型推論](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference)は、「`image`」フィールドから `<img>` HTML 要素を自動的に作成するのに使用されます。フィールドの折りたたみは、「`image`」フィールドと「`imageAlt`」フィールドで使用され、`<img>` HTML 要素を作成します。`src` 属性は「`image`」フィールドの値に設定され、`alt` 属性は「`imageAlt`」フィールドの値に設定されます。
- `textContent` は、フィールドを分類するのに使用されるグループ名です。セマンティックにする必要がありますが、このブロックに固有のものであれば何でも構いません。これにより、ユニバーサルエディターに、このプレフィックスを持つすべてのフィールドを最終的な HTML 出力の同じ `<div>` 要素内にレンダリングするように通知されます。
- フィールドの折りたたみは、コールトゥアクション（CTA）の `textContent` グループ内でも適用されます。CTA は、[型推論](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference)を通じて `<a>` として作成されます。「`cta`」フィールドは `<a>` 要素の `href` 属性を設定するのに使用され、「`ctaText`」フィールドは `<a ...>` タグ内のリンクのテキストコンテンツを提供します。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```json
{
    "definitions": [],
    "models": [
        {
            "id": "teaser", 
            "fields": [
                {
                    "component": "reference",
                    "valueType": "string",
                    "name": "image",
                    "label": "Image",
                    "multi": false
                },
                {
                    "component": "text",
                    "valueType": "string",
                    "name": "imageAlt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "textContent_text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "textContent_cta",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "textContent_ctaText",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

このモデルは、ブロックのユニバーサルエディターでのオーサリング入力を定義します。

このブロックの結果として得られる Edge Delivery Services HTML では、最初の div に画像が配置され、2 番目の div に要素グループの「`textContent`」フィールドが配置されます。

```html
<div>
    <div>
        <!-- This div contains the field-collapsed image fields  -->
        <picture>
            ...
            <source .../>            
            <img src="..." alt="The authored alt text"/>
        </picture>
    </div>
    <div>
        <!-- This div, via element grouping contains the textContent fields -->
        <h2>The authored title</h2>
        <p>The authored body text</p>
        <a href="/authored/cta/link">The authored CTA label</a>
    </div>
</div>        
```

[次の章](./7a-block-css.md)で説明するように、この HTML 構造により、ブロックをまとまりのある単位としてスタイル設定することが簡素化されます。

フィールドの折りたたみと要素のグループ化を使用しない場合の結果を理解するには、上記の「**間違った方法**」タブを参照してください。

>[!TAB 間違った方法]

**このタブでは、ティーザーブロックをモデル化する次善策としての方法について説明しますが、正しい方法との比較にすぎません。**

[フィールドの折りたたみ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)や[要素のグループ化](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping)を使用せずに、各フィールドをブロックモデル内のスタンドアロンフィールドとして定義することは魅力的に思える場合があります。ただし、この見落としにより、ブロックをまとまりのある単位としてスタイル設定することが複雑になります。

例えば、ティーザーモデルは、フィールドの折りたたみや要素のグループ化を&#x200B;**行わず**&#x200B;に次のように定義できます。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```json
{
    "definitions": [],
    "models": [
        {
            "id": "teaser", 
            "fields": [
                {
                    "component": "reference",
                    "valueType": "string",
                    "name": "image",
                    "label": "Image",
                    "multi": false
                },
                {
                    "component": "text",
                    "valueType": "string",
                    "name": "alt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "link",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "label",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

ブロックの Edge Delivery Services HTML では、各フィールドの値を個別の `div` にレンダリングするので、目的のデザインを実現するコンテンツの理解、スタイルの適用、HTML 構造の調整が複雑になります。

```html
<div>
    <div>
        <!-- This div contains the field-collapsed image  -->
        <picture>
            ...
            <source .../>            
            <img src="/authored/image/reference"/>
        </picture>
    </div>
    <div>
        <p>The authored alt text</p>
    </div>
    <div>
        <h2>The authored title</h2>
        <p>The authored body text</p>
    </div>
    <div>
        <a href="/authored/cta/link">/authored/cta/link</a>
    </div>
    <div>
        The authored CTA label
    </div>
</div>        
```

各フィールドは独自の `div` に分離されているので、画像とテキストコンテンツをまとまりのある単位としてスタイル設定することが困難です。労力と創造性で目的のデザインを実現することは可能ですが、[要素のグループ化](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping)を使用してテキストコンテンツフィールドをグループ化し、[フィールドの折りたたみ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)を使用して、作成された値を要素属性として追加する方が、よりシンプルで簡単であり、セマンティクス的にも適切です。

ティーザーブロックをより適切にモデル化する方法について詳しくは、上記の「**正しい方法**」タブを参照してください。

>[!ENDTABS]


### ブロックの定義

ブロックの定義では、ユニバーサルエディターにブロックを登録します。ブロックの定義で使用される JSON プロパティの分類を以下に示します。

| JSON プロパティ | 説明 |
|---------------|-------------|
| `definition.title` | ユニバーサルエディターの&#x200B;**追加**&#x200B;ブロックに表示されるブロックのタイトル。 |
| `definition.id` | `filters` での使用を制御するのに使用される、ブロックの一意の ID。 |
| `definition.plugins.xwalk.page.resourceType` | ユニバーサルエディターでコンポーネントをレンダリングする Sling リソースタイプを定義します。常に `core/franklin/components/block/v#/block` リソースタイプを使用します。 |
| `definition.plugins.xwalk.page.template.name` | ブロックの名前。ブロックのフォルダー名と一致するように、小文字にしてハイフンで区切る必要があります。この値は、ユニバーサルエディターでブロックのインスタンスにラベルを付ける場合にも使用されます。 |
| `definition.plugins.xwalk.page.template.model` | ユニバーサルエディターでブロックに対して表示されるオーサリングフィールドを制御する `model` 定義にこの定義をリンクします。ここの値は、`model.id` 値と一致する必要があります。 |
| `definition.plugins.xwalk.page.template.classes` | 値がブロック HTML 要素の `class` 属性に追加される、オプションのプロパティ。これにより、同じブロックのバリアントが可能になります。ブロックの[モデル](#block-model)に[クラスフィールドを追加](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/create-block#block-options)して、`classes` 値を編集可能にすることができます。 |


ブロック定義の JSON の例を次に示します。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```json
{
    "definitions": [{
      "title": "Teaser",
      "id": "teaser",
      "plugins": {
        "xwalk": {
          "page": {
            "resourceType": "core/franklin/components/block/v1/block",
            "template": {
              "name": "Teaser",
              "model": "teaser",
              "textContent_text": "<h2>Enter a title</h2><p>...and body text here!</p>",
              "textContent_cta": "/",
              "textContent_ctaText": "Click me!"
            }
          }
        }
      }
    }],
    "models": [... from previous section ...],
    "filters": []
}
```

この例では、次のようになります。

- このブロックの名前は「ティーザー」で、ユニバーサルエディターで編集できるフィールドを決定する `teaser` モデルを使用します。
- ブロックには、タイトルと本文のリッチテキスト領域である「`textContent_text`」フィールドのデフォルトコンテンツと、CTA（コールトゥアクション）リンクとラベルの `textContent_cta` と `textContent_ctaText` が含まれます。初期コンテンツを含むテンプレートのフィールド名は、[コンテンツモデルのフィールド配列](#block-model)で定義されたフィールド名と一致します。

この構造により、レンダリング用の適切なフィールド、コンテンツモデル、リソースタイプを使用して、ブロックがユニバーサルエディターで設定されます。

### ブロックのフィルター

ブロックの `filters` 配列は、[コンテナブロック](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#container)の場合、コンテナに追加できる他のブロックを定義します。フィルターは、コンテナに追加できるブロック ID（`model.id`）のリストを定義します。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```json
{
  "definitions": [... populated from previous section ...],
  "models": [... populated from previous section ...],
  "filters": []
}
```

ティーザーコンポーネントは[コンテナブロック](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#container)ではないので、他のブロックを追加できません。その結果、`filters` 配列は空のままになります。代わりに、ティーザーの ID をセクションブロックのフィルターリストに追加して、ティーザーをセクションに追加できるようにします。

![ブロックのフィルター](./assets/5-new-block/filters.png)

アドビが提供するブロック（セクションブロックなど）は、プロジェクトの `models` フォルダーにフィルターを保存します。調整するには、アドビが提供するブロックの JSON ファイル（`/models/_section.json` など）を見つけて、ティーザーの ID（`teaser`）をフィルターリストに追加します。この設定により、ティーザーコンポーネントをセクションコンテナブロックに追加できることがユニバーサルエディターに通知されます。

[!BADGE /models/_section.json]{type=Neutral tooltip="以下のコードサンプルのファイル名。"}

```json
{
  "definitions": [],
  "models": [],
  "filters": [
    {
      "id": "section",
      "components": [
        "text",
        "image",
        "button",
        "title",
        "hero",
        "cards",
        "columns",
        "fragment",
        "teaser"
      ]
    }
  ]
}
```

`teaser` のティーザーブロック定義 ID が `components` 配列に追加されます。

## JSON ファイルのリント

変更がクリーンで一貫性のあることを確認するには、[頻繁にリント](./3-local-development-environment.md#linting)します。頻繁にリンティングを行うと、問題を早期に発見し、全体的な開発時間を短縮できます。`npm run lint:js` コマンドも JSON ファイルをリントし、構文エラーを検出します。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:js
```

## プロジェクトの JSON のビルド

ブロックの JSON ファイル（`blocks/teaser/_teaser.json`、`models/_section.json`）を設定したら、プロジェクトの `component-models.json`、`component-definitions.json`、`component-filters.json` ファイルにコンパイルする必要があります。コンパイルは、プロジェクトの[ビルド JSON](./3-local-development-environment.md#build-json-fragments) npm スクリプトを実行することによって行われます。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run build:json
```

## ブロック定義のデプロイ

ユニバーサルエディターでブロックを使用できるようにするには、プロジェクトをコミットして GitHub リポジトリの分岐（この場合は、`teaser` 分岐）にプッシュする必要があります。

ユニバーサルエディターが使用する正確な分岐名は、ユニバーサルエディターの URL を通じてユーザー別に調整できます。

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "Add teaser block JSON files so it is available in Universal Editor"
$ git push origin teaser
```

クエリパラメーター `?ref=teaser` を使用してユニバーサルエディターを開くと、新しい `teaser` ブロックがブロックパレットに表示されます。ブロックにはスタイル設定がないので、ブロックのフィールドはセマンティック HTML としてレンダリングされ、[グローバル CSS](./4-website-branding.md#global-css) を通じてのみスタイル設定されます。
