---
title: フラグメントリファレンスを使用した高度なデータモデリング — AEMヘッドレスの概要 — GraphQL
description: Adobe Experience Manager(AEM) と GraphQL の基本を学びます。 高度なデータモデリングでフラグメントリファレンス機能を使用し、2 つの異なるコンテンツフラグメント間の関係を作成する方法を説明します。 GraphQL クエリを変更して、参照モデルのフィールドを含める方法を説明します。
version: Cloud Service
mini-toc-levels: 1
kt: 6718
thumbnail: KT-6718.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d85b7ac3-42c1-4655-9394-29a797c0e1d7
source-git-commit: 0dae6243f2a30147bed7079ad06144ad35b781d8
workflow-type: tm+mt
source-wordcount: '847'
ht-degree: 2%

---

# フラグメントリファレンスを使用した高度なデータモデリング

別のコンテンツフラグメント内からコンテンツフラグメントを参照できます。 これにより、ユーザーはフラグメント間の関係を持つ複雑なデータモデルを構築できます。

この章では、アドベンチャーモデルを更新し、 **フラグメント参照** フィールドに入力します。 また、GraphQL クエリを変更して、参照モデルのフィールドを含める方法についても学習します。

## 前提条件

これは複数のパートから成るチュートリアルで、前のパートで説明した手順が完了していることを前提としています。

## 目的

この章では、次の方法について説明します。

* 「フラグメント参照」フィールドを使用するようにコンテンツフラグメントモデルを更新する
* 参照モデルからフィールドを返す GraphQL クエリを作成する

## フラグメント参照の追加 {#add-fragment-reference}

コントリビューターモデルへの参照を追加するには、アドベンチャーコンテンツフラグメントモデルを更新します。

1. 新しいブラウザーを開き、AEMに移動します。
1. 次の **AEM Start** メニュー移動先 **ツール** > **Assets** > **コンテンツフラグメントモデル** > **WKND サイト**.
1. を開きます。 **冒険** コンテンツフラグメントモデル

   ![アドベンチャーコンテンツフラグメントモデルを開く](assets/fragment-references/adventure-content-fragment-edit.png)

1. の下 **データタイプ**、ドラッグ&amp;ドロップ **フラグメント参照** フィールドをメインパネルに挿入します。

   ![フラグメント参照フィールドを追加](assets/fragment-references/add-fragment-reference-field.png)

1. を更新します。 **プロパティ** を次のように設定します。

   * レンダリング時の名前 - `fragmentreference`
   * フィールドラベル — **アドベンチャー寄稿者**
   * プロパティ名 - `adventureContributor`
   * モデルタイプ — **寄稿者** モデル
   * ルートパス - `/content/dam/wknd`

   ![フラグメント参照プロパティ](assets/fragment-references/fragment-reference-properties.png)

   プロパティ名 `adventureContributor` これで、コントリビューターコンテンツフラグメントを参照するために使用できるようになりました。

1. 変更内容をモデルに保存します。

## コントリビューターをアドベンチャーに割り当てる

アドベンチャーコンテンツフラグメントモデルが更新されたので、既存のフラグメントを編集し、コントリビューターを参照できます。 コンテンツフラグメントモデルの編集に注意してください *影響* そこから作成された既存のコンテンツフラグメント。

1. に移動します。 **Assets** > **ファイル** > **WKND サイト** > **英語** > **冒険** > **[バリサーフキャンプ](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**.

   ![バリサーフキャンプフォルダ](../quick-setup/assets/setup/bali-surf-camp-folder.png)

1. をクリックして、 **バリサーフキャンプ** コンテンツフラグメントを使用して、コンテンツフラグメントエディターを開きます。
1. を更新します。 **アドベンチャー寄稿者** 「寄稿者」フィールドに入力し、フォルダアイコンをクリックして投稿者を選択します。

   ![寄稿者として [Stacey Roswels] を選択します。](assets/fragment-references/stacey-roswell-contributor.png)

   *寄稿者フラグメントへのパスを選択*

   ![投稿者への入力済みパス](assets/fragment-references/populated-path.png)

   フラグメントは、 **寄稿者** モデルを選択できます。

1. フラグメントへの変更を保存します。

1. 上記の手順を繰り返して、コントリビューターを次のような冒険に割り当てます。 [ヨセミテバックパッキン](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking) および [コロラドロッククライミング](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/colorado-rock-climbing/colorado-rock-climbing)

## GraphiQL を使用した、入れ子になったコンテンツフラグメントのクエリ

次に、アドベンチャーのクエリを実行し、参照されるコントリビューターモデルのネストされたプロパティを追加します。 GraphiQL ツールを使用して、クエリの構文をすばやく確認します。

1. AEMの GraphiQL ツールに移動します。 [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)

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

   上記のクエリは、そのパスで 1 つのアドベンチャー用です。 この `adventureContributor` プロパティは寄稿者モデルを参照し、ネストされたコンテンツフラグメントからプロパティを要求できます。

1. クエリを実行すると、次のような結果が返されます。

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

1. などの他のクエリを使用して実験する `adventureList` 参照されるコンテンツフラグメントのプロパティをの下に追加します。 `adventureContributor`.

## 寄稿者コンテンツを表示するように React アプリを更新する

次に、React アプリケーションで使用されるクエリを更新して、新しい寄稿者を含め、その寄稿者に関する情報をアドベンチャーの詳細ビューの一部として表示します。

1. IDE で WKND GraphQL React アプリを開きます。

1. `src/components/AdventureDetail.js` ファイルを開きます。

   ![アドベンチャー詳細コンポーネント IDE](assets/fragment-references/adventure-detail-ide.png)

1. 関数を検索する `adventureDetailQuery(_path)`. この `adventureDetailQuery(..)` 関数は、AEMを使用するフィルタリング GraphQL クエリをラップするだけです `<modelName>ByPath` 構文を使用して、JCR パスで識別される単一のコンテンツフラグメントをクエリします。

1. クエリを更新して、参照先の寄稿者に関する情報を含めます。

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

   この更新により、 `adventureContributor`, `fullName`, `occupation`、および `pictureReference` がクエリに含まれます。

1. Inspect `Contributor` 埋め込まれたコンポーネント `AdventureDetail.js` ～にファイルを送る `function Contributor(...)`. このコンポーネントは、プロパティが存在する場合に、寄稿者の名前、職業、画像をレンダリングします。

   この `Contributor` コンポーネントが `AdventureDetail(...)` `return` メソッド：

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

1. 変更をファイルに保存します。
1. React アプリを起動します（まだ実行していない場合）。

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. に移動します。 [http://localhost:3000](http://localhost:3000/) そして、参照先の寄稿者を持つアドベンチャーをクリックします。 これで、以下にコントリビューター情報が表示されます。 **旅程**:

   ![アプリに追加されたコントリビューター](assets/fragment-references/contributor-added-detail.png)

## おめでとうございます。{#congratulations}

おめでとうございます。既存のコンテンツフラグメントモデルを更新し、 **フラグメント参照** フィールドに入力します。 また、GraphQL クエリを変更して、参照モデルのフィールドを含める方法も学習しました。

## 次の手順 {#next-steps}

次の章では、 [AEM パブリッシュ環境を使用した実稼動のデプロイメント](./production-deployment.md)AEM オーサーサービスとパブリッシュサービス、およびヘッドレスアプリケーションで推奨されるデプロイメントパターンについて説明します。 環境変数を使用するように既存のアプリケーションを更新し、ターゲット環境に基づいて GraphQL エンドポイントを動的に変更します。 また、クロスオリジンリソース共有 (CORS) 用にAEMを適切に設定する方法についても説明します。
