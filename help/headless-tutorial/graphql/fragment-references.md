---
title: フラグメント参照を使用した高度なデータモデリング — AEMヘッドレス — GraphQLの概要
description: Adobe Experience Manager(AEM)とGraphQLの使い始めに 高度なデータモデリングのためのフラグメント参照機能の使用方法と、2つの異なるコンテンツフラグメント間の関係の作成方法について説明します。 GraphQLクエリを変更して、参照モデルのフィールドを含める方法を学びます。
sub-product: アセット
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6718
thumbnail: KT-6718.jpg
translation-type: tm+mt
source-git-commit: eb2b556c5947b15a31a74a86dadd525fb06bcf14
workflow-type: tm+mt
source-wordcount: '858'
ht-degree: 2%

---


# フラグメント参照を使用した高度なデータモデリング

>[!CAUTION]
>
> AEM GraphQL API for Content Fragments配信は、リクエストに応じて使用できます。
> お使いのAEM用のAPIをCloud Serviceプログラムとして有効にするには、Adobeサポートにお問い合わせください。

コンテンツフラグメントは、別のコンテンツフラグメント内から参照できます。 これにより、ユーザーはFragments間の関係を持つ複雑なデータモデルを作成できます。

この章では、**フラグメント参照**&#x200B;フィールドを使用して、アドベンチャーモデルを更新し、寄稿者モデルへの参照を含めます。 また、GraphQLクエリを修正して、参照モデルのフィールドを含める方法も学習します。

## 前提条件

これはマルチパートのチュートリアルで、前の部分で説明した手順が完了していることを前提としています。

## 目的

この章では、次の方法について学びます。

* コンテンツフラグメントモデルを更新して、フラグメント参照フィールドを使用する
* 参照モデルからフィールドを返すGraphQLクエリの作成

## 追加フラグメント参照{#add-fragment-reference}

アドベンチャーコンテンツフラグメントモデルを更新して、寄稿者モデルへの参照を追加します。

1. 新しいブラウザーを開き、AEMに移動します。
1. **AEM開始**&#x200B;メニューから&#x200B;**ツール****アセット****コンテンツフラグメントモデル****WKNDサイト**&#x200B;に移動します。
1. **アドベンチャー**&#x200B;コンテンツフラグメントモデルを開く

   ![アドベンチャーコンテンツフラグメントモデルを開く](assets/fragment-references/adventure-content-fragment-edit.png)

1. 「**データタイプ**」で、**フラグメント参照**&#x200B;フィールドをメインパネルにドラッグ&amp;ドロップします。

   ![追加フラグメント参照フィールド](assets/fragment-references/add-fragment-reference-field.png)

1. このフィールドの&#x200B;**プロパティ**&#x200B;を次の値で更新します。

   * レンダリング時の名前 - `fragmentreference`
   * フィールドラベル — **アドベンチャー寄稿者**
   * プロパティ名 - `adventureContributor`
   * モデルタイプ — **寄稿者**&#x200B;モデルを選択します。
   * ルートパス - `/content/dam/wknd`

   ![フラグメント参照プロパティ](assets/fragment-references/fragment-reference-properties.png)

   これで、プロパティ名`adventureContributor`を使用して、寄稿者コンテンツフラグメントを参照できるようになりました。

1. 変更内容をモデルに保存します。

## 寄稿者のアドベンチャーへの割り当て

アドベンチャーコンテンツフラグメントモデルが更新されたので、既存のフラグメントを編集し、寄稿者を参照できます。 コンテンツフラグメントモデル&#x200B;*の編集は、そのモデルから作成された既存のコンテンツフラグメント*&#x200B;に影響することに注意してください。

1. **アセット**/**ファイル**/**WKNDサイト**/**英語**/**冒険**/**[バリ>サーフキャンプ](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)&lt;a1に移動します。2/>.**

   ![バリサーフキャンプフォルダ](assets/setup/bali-surf-camp-folder.png)

1. **Bali Surf Camp**&#x200B;コンテンツフラグメント内をクリックして、コンテンツフラグメントエディターを開きます。
1. 「**アドベンチャー寄稿者**」フィールドを更新し、フォルダアイコンをクリックして寄稿者を選択します。

   ![寄稿者としてStacey Roswellsを選択](assets/fragment-references/stacey-roswell-contributor.png)

   *寄稿者フラグメントへのパスを選択*

   ![寄稿者への入力パス](assets/fragment-references/populated-path.png)

   **寄稿者**&#x200B;モデルを使用して作成されたフラグメントのみを選択できます。

1. フラグメントに変更を保存します。

1. 上記の手順を繰り返して、寄稿者を[Yosemite Backpacking](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking)や[Colorado Rock Climing](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/colorado-rock-climbing/colorado-rock-climbing)のような冒険に割り当てます。

## GraphiQLを使用したクエリネストコンテンツフラグメント

次に、アドベンチャーのクエリを実行し、参照先の寄稿者モデルのネストされたプロパティを追加します。 クエリの構文をすばやく確認するには、GraphicQLツールを使用します。

1. AEMのGraphicQLツールに移動します。[http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)

1. 次のクエリを入力します。

   ```graphql
   {
     adventureByPath(_path:"/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp") {
        item {
          _path
          adventureTitle
          adventureContributor {
            fullName
            occupation
            pictureReference {
           ...on ImageRef {
             _path
           }
         }
       }
     }
    }
   }
   ```

   上記のクエリは1つのアドベンチャーのためのものです。 `adventureContributor`プロパティは寄稿者モデルを参照し、ネストされたコンテンツフラグメントからプロパティを要求できます。

1. クエリを実行すると、次のような結果が得られます。

   ```json
   {
     "data": {
       "adventureByPath": {
           "item": {
               "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
               "adventureTitle": "Bali Surf Camp",
               "adventureContributor": {
                   "fullName": "Stacey Roswells",
                   "occupation": "Photographer",
                   "pictureReference": {
                       "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
                   }
               }
           }
        }
     }
   }
   ```

1. `adventureList`などの他のクエリを試し、`adventureContributor`の下に、参照されるコンテンツフラグメントのプロパティを追加します。

## 寄稿者コンテンツを表示するためのReactアプリの更新

次に、リアクトアプリケーションで使用するクエリを更新して、新しい寄稿者を含め、その寄稿者に関する情報をアドベンチャーの詳細表示の一部として表示します。

1. IDEでWKND GraphQL Reactアプリを開きます。

1. `src/components/AdventureDetail.js` ファイルを開きます。

   ![アドベンチャー詳細コンポーネントIDE](assets/fragment-references/adventure-detail-ide.png)

1. 関数`adventureDetailQuery(_path)`を探します。 `adventureDetailQuery(..)`関数は、フィルタリングGraphQLクエリを単純にラップします。この構文はAEM `<modelName>ByPath`構文を使用し、JCRパスで識別される単一のコンテンツフラグメントをクエリします。

1. 参照先の寄稿者に関する情報が含まれるようにクエリを更新します。

   ```javascript
   function adventureDetailQuery(_path) {
       return `{
           adventureByPath (_path: "${_path}") {
           item {
               _path
               adventureTitle
               adventureActivity
               adventureType
               adventurePrice
               adventureTripLength
               adventureGroupSize
               adventureDifficulty
               adventurePrice
               adventurePrimaryImage {
                   ... on ImageRef {
                   _path
                   mimeType
                   width
                   height
                   }
               }
               adventureDescription {
                   html
               }
               adventureItinerary {
                   html
               }
               adventureContributor {
                   fullName
                   occupation
                   pictureReference {
                       ...on ImageRef {
                           _path
                       }
                   }
               }
             }
          }
        }
       `;
   }
   ```

   この更新により、`adventureContributor`、`fullName`、`occupation`、`pictureReference`に関する追加のプロパティがクエリに含まれます。

1. `function Contributor(...)`の`AdventureDetail.js`ファイルに埋め込まれた`Contributor`コンポーネントをInspect。 プロパティが存在する場合、このコンポーネントは、寄稿者の名前、職業、および画像をレンダリングします。

   `Contributor`コンポーネントは、`AdventureDetail(...)` `return`メソッドで参照されます。

   ```javascript
   function AdventureDetail(props) {
       ...
       return (
           ...
            <h2>Itinerary</h2>
           <hr />
           <div className="adventure-detail-itinerary"
                dangerouslySetInnerHTML={{__html: adventureData.adventureItinerary.html}}></div>
           {/* Contributor component is instaniated and 
               is passed the adventureContributor object from the GraphQL Query results */}
           <Contributer {...adventureData.adventureContributor} />
           ...
       )
   }
   ```

1. ファイルに変更を保存します。
1. React Appの開始（まだ実行していない場合）:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. [http://localhost:3000](http://localhost:3000/)に移動し、参照先のコントリビューターを持つアドベンチャーをクリックします。 これで、**旅程**&#x200B;の下に寄稿者情報が表示されます。

   ![寄稿者がアプリに追加された](assets/fragment-references/contributor-added-detail.png)

## その他のリソース

コンテンツフラグメントとGraphQLの詳細については、次のリソースを参照してください。

* [GraphQLでのコンテンツフラグメントを使用したヘッドレスコンテンツ配信](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html)
* [AEM GraphQL API（コンテンツフラグメントで使用）](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html)

## おめでとう！{#congratulations}

バリデーターが既存のコンテンツフラグメントモデルを更新し、**フラグメント参照**&#x200B;フィールドを使用して、ネストされたコンテンツフラグメントを参照するようにしました。 また、GraphQLクエリを修正して、参照モデルからフィールドを含める方法も学びました。
