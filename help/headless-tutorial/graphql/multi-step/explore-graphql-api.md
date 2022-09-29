---
title: GraphQL API の参照 — AEMヘッドレスの概要 — GraphQL
description: Adobe Experience Manager(AEM) と GraphQL の基本を学びます。 組み込みの GraphQL IDE を使用してAEM GraphQL API を調べます。 コンテンツフラグメントモデルに基づいて、AEMが GraphQL スキーマを自動的に生成する方法を説明します。 GraphQL 構文を使用して、基本的なクエリを作成してみてください。
version: Cloud Service
mini-toc-levels: 1
kt: 6714
thumbnail: KT-6714.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 508b0211-fa21-4a73-b8b4-c6c34e3ba696
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1529'
ht-degree: 7%

---

# GraphQL API の参照 {#explore-graphql-apis}

AEMの GraphQL API は、コンテンツフラグメントのデータをダウンストリームアプリケーションに公開する強力なクエリ言語です。 コンテンツフラグメントモデルは、コンテンツフラグメントで使用されるデータスキーマを定義します。 コンテンツフラグメントモデルが作成または更新されるたびに、スキーマが翻訳され、GraphQL API を構成する「グラフ」に追加されます。

この章では、IDE を使用してコンテンツを収集する、一般的な GraphQL クエリをいくつか調べます。 [GraphiQL](https://github.com/graphql/graphiql). GraphiQL IDE を使用すると、返されるクエリとデータをすばやくテストし、調整できます。 また、GraphiQL はドキュメントへのアクセスも容易にし、どのようなメソッドがあるのか、簡単に学習して理解できます。

## 前提条件 {#prerequisites}

これは複数のパートから成るチュートリアルで、 [コンテンツフラグメントのオーサリング](./author-content-fragments.md) が完了しました。

## 目的 {#objectives}

* GraphiQL ツールを使用して、GraphQL 構文を使用してクエリを作成する方法を説明します。
* コンテンツフラグメントのリストと単一のコンテンツフラグメントに対してクエリを実行する方法を説明します。
* 特定のデータ属性をフィルタリングしてリクエストする方法を説明します。
* 複数のコンテンツフラグメントモデルのクエリを結合する方法を説明します
* GraphQL クエリを保持する方法を説明します。

## GraphQL エンドポイントの有効化 {#enable-graphql-endpoint}

GraphQL エンドポイントは、コンテンツフラグメントに対する GraphQL API クエリを有効にするように設定する必要があります。

1. AEM Start 画面からに移動します。 **ツール** > **一般** > **GraphQL**.

   ![GraphQL エンドポイントに移動します。](assets/explore-graphql-api/navigate-to-graphql-endpoint.png)

1. タップ **作成** をクリックします。 ダイアログで、次の値を入力します。

   * 名前*: **マイプロジェクトエンドポイント**.
   * 次によって提供される GraphQL スキーマを使用… *: **マイプロジェクト**

   ![GraphQL エンドポイントを作成](assets/explore-graphql-api/create-graphql-endpoint.png)

   タップ **作成** をクリックしてエンドポイントを保存します。

   プロジェクト設定に基づいて作成された GraphQL エンドポイントでは、そのプロジェクトに属するモデルに対するクエリのみが有効になります。 この場合、 **人物** および **チーム** モデルを使用できます。

   >[!NOTE]
   >
   > また、グローバルエンドポイントを作成して、複数の設定をまたいでモデルに対するクエリを有効にすることもできます。 これは、環境がセキュリティの脆弱性をさらに増す可能性があり、AEMの管理が全体的に複雑になるので、慎重に使用する必要があります。

1. これで、お使いの環境で 1 つの GraphQL エンドポイントが有効になっているはずです。

   ![Graphql エンドポイントが有効](assets/explore-graphql-api/enabled-graphql-endpoints.png)

## GraphiQL IDE の使用

この [GraphiQL ツール](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) を使用すると、開発者は、現在のAEM環境上のコンテンツに対するクエリを作成およびテストできます。 GraphiQL ツールを使用すると、次のことが可能になります。 **保持** 実稼働設定でクライアントアプリケーションが使用するクエリを保存します。

次に、組み込みの GraphiQL IDE を使用してAEM GraphQL API の機能を調べます。

1. AEM Start 画面からに移動します。 **ツール** > **一般** > **GraphQL クエリエディター**.

   ![GraphiQL IDE に移動します。](assets/explore-graphql-api/navigate-graphql-query-editor.png)

   >[!NOTE]
   >
   > 古いバージョンのAEMの場合、GraphiQL IDE は組み込まれない可能性があります。 次の手順に従って手動でインストールできます [説明](#install-graphiql).

1. 右上隅に、 **エンドポイント** が **マイプロジェクトエンドポイント**.

   ![GraphQL エンドポイントを設定](assets/explore-graphql-api/set-my-project-endpoint.png)

これにより、 **マイプロジェクト** プロジェクト。

### コンテンツフラグメントのリストのクエリ {#query-list-cf}

共通の要件は、複数のコンテンツフラグメントに対するクエリです。

1. 次のクエリをメインパネルに貼り付けます（コメントのリストの置き換え）。

   ```graphql
   query allTeams {
     teamList {
       items {
         _path
         title
       }
     }
   } 
   ```

1. を押します。 **再生** ボタンを使用してクエリを実行します。 前の章のコンテンツフラグメントの結果が表示されます。

   ![担当者リストの結果](assets/explore-graphql-api/all-teams-list.png)

1. カーソルを `title` テキストと入力 **Ctrl +スペース** トリガーコードのヒント 追加 `shortname` および `description` をクエリに追加します。

   ![コード編集でクエリを更新](assets/explore-graphql-api/update-query-codehinting.png)

1. クエリを再度実行するには、 **再生** ボタンをクリックすると、 `shortname` および `description`.

   ![短い名前と説明の結果](assets/explore-graphql-api/updated-query-shortname-description.png)

   この `shortname` は単純なプロパティで、 `description` は複数行テキストフィールドで、GraphQL API を使用すると、次のような結果に対して様々な形式を選択できます。 `html`, `markdown`, `json` または `plaintext`.

### ネストされたフラグメントのクエリ

次に、クエリを試してみます。ネストされたフラグメントを取得する際には、 **チーム** モデルが参照する **人物** モデル。

1. クエリを更新して `teamMembers` プロパティ。 これは **フラグメント参照** フィールドから担当者モデルへ。 人物モデルのプロパティを返すことができます。

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   JSON 応答：

   ```json
   {
       "data": {
           "teamList": {
           "items": [
               {
               "_path": "/content/dam/my-project/en/team-alpha",
               "title": "Team Alpha",
               "shortName": "team-alpha",
               "description": {
                   "plaintext": "This is a description of Team Alpha!"
               },
               "teamMembers": [
                   {
                   "fullName": "John Doe",
                   "occupation": [
                       "Artist",
                       "Influencer"
                   ]
                   },
                   {
                   "fullName": "Alison Smith",
                   "occupation": [
                       "Photographer"
                   ]
                   }
                 ]
           }
           ]
           }
       }
   }
   ```

   ネストされたフラグメントに対してクエリを実行する機能は、AEM GraphQL API の強力な機能です。 この簡単な例では、ネストの深さは 2 レベルです。 ただし、フラグメントをさらにネストすることは可能です。 例えば、 **住所** ～に関連するモデル **人物** 3 つのモデルすべてから 1 つのクエリでデータを返すことができます。

### コンテンツフラグメントのリストのフィルタリング {#filter-list-cf}

次に、プロパティ値に基づいて結果をコンテンツフラグメントのサブセットにフィルタリングする方法を見てみましょう。

1. GraphiQL UI で次のクエリを入力します。

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{
         _path
         fullName
         occupation
       }
     }
   }  
   ```

   上記のクエリは、システム内のすべてのユーザーフラグメントに対して検索を実行します。 クエリの先頭に追加されたフィルターにより、 `name` フィールドと変数文字列 `$name`.

1. 内 **クエリ変数** パネルに次を入力します。

   ```json
   {"name": "John Doe"}
   ```

1. クエリを実行する場合、必ず **人物** の値は「John Doe」で返されます。

   ![クエリ変数を使用したフィルタリング](assets/explore-graphql-api/using-query-variables-filter.png)

   複雑なクエリをフィルタリングして作成する方法は他にも多数あります。詳しくは、 [AEMでの GraphQL の使用方法の学習 — サンプルコンテンツとクエリ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html?lang=ja).

1. 上のクエリを強化してプロファイル画像を取得

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{  
         _path
         fullName
         occupation
         profilePicture{
           ... on ImageRef{
             _path
             _authorUrl
             _publishUrl
             height
             width
   
           }
         }
       }
     }
   } 
   ```

   この `profilePicture` はコンテンツ参照であり、画像であると想定されるので、組み込みの `ImageRef` オブジェクトが使用されます。 これにより、参照されている画像に関する追加データ ( `width` および `height`.

### 単一のコンテンツフラグメントのクエリ {#query-single-cf}

単一のコンテンツフラグメントに対して直接クエリを実行することもできます。 AEMのコンテンツは階層的に保存され、フラグメントの一意の識別子はフラグメントのパスに基づきます。

1. GraphiQL エディターに次のクエリを入力します。

   ```graphql
   query personByPath($path: String!) {
       personByPath(_path: $path) {
           item {
           fullName
           occupation
           }
       }
   }
   ```

1. に次を入力します。 **クエリ変数**:

   ```json
   {"path": "/content/dam/my-project/en/alison-smith"}
   ```

1. クエリを実行し、単一の結果が返されることを確認します。

## クエリを保持 {#persist-queries}

開発者がクエリとデータが返されたら、次の手順はクエリをAEMに保存または保持することです。 [永続クエリ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html) は、GraphQL API をクライアントアプリケーションに公開するための推奨されるメカニズムです。 クエリが保持されると、GETリクエストを使用してリクエストでき、Dispatcher および CDN レイヤーでキャッシュできます。 永続化されたクエリのパフォーマンスがはるかに向上しました。 パフォーマンスのメリットに加えて、永続化されたクエリにより、余分なデータが誤ってクライアントアプリケーションに公開されるのを防ぐことができます。 詳細： [永続化されたクエリは、ここで確認できます。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html).

次に、2 つのシンプルなクエリを保持します。これらは次の章で使用します。

1. GraphiQL IDE で、次のクエリを入力します。

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   クエリが機能することを確認します。

1. 次のタップ **名前を付けて保存** と入力します。 `all-teams` を **クエリ名**.

   これで、クエリがの下に表示されます。 **永続クエリ** をクリックします。

   ![すべてのチームが持続するクエリ](assets/explore-graphql-api/all-teams-persisted-query.png)
1. 次に、省略記号をタップします。 **...** 永続クエリの横にあるをタップし、 **URL をコピー** をクリックして、パスをクリップボードにコピーします。

   ![永続的なクエリ URL をコピー](assets/explore-graphql-api/copy-persistent-query-url.png)

1. 新しいタブを開き、コピーしたパスをブラウザーに貼り付けます。

   ```plain
   https://$YOUR-AEMasCS-INSTANCEID$.adobeaemcloud.com/graphql/execute.json/my-project/all-teams
   ```

   上記のパスのようになります。 返されたクエリの JSON 結果が表示されます。

   URL の分類：

   | 名前 | 説明 |
   | ---------|---------- |
   | `/graphql/execute.json` | 永続的なクエリエンドポイント |
   | `/my-project` | のプロジェクト設定 `/conf/my-project` |
   | `/all-teams` | 永続的なクエリの名前 |

1. GraphiQL IDE に戻り、プラスボタンを使用します。 **+** をクリックして、NEW クエリを終了します。

   ```graphql
   query personByName($name: String!) {
     personList(
       filter: {
         fullName:{
           _expressions: [{
             value: $name
             _operator:EQUALS
           }]
         }
       }){
       items {
         _path
         fullName
         occupation
         biographyText {
           json
         }
         profilePicture {
           ... on ImageRef {
             _path
             _authorUrl
             _publishUrl
             width
             height
           }
         }
       }
     }
   }
   ```

1. クエリに次の名前を付けて保存します。 **人名別**.
1. 次の 2 つの永続クエリを保存する必要があります。

   ![最終的に保持されたクエリ](assets/explore-graphql-api/final-persisted-queries.png)


## GraphQL エンドポイントと永続クエリの公開

レビューと検証の際に、 `GraphQL Endpoint` &amp; `Persisted Queries`

1. AEM Start 画面からに移動します。 **ツール** > **一般** > **GraphQL**.

1. の横にあるチェックボックスをタップします。 **マイプロジェクトエンドポイント** とタップします。 **公開**

   ![GraphQL エンドポイントを公開](assets/explore-graphql-api/publish-graphql-endpoint.png)

1. AEM Start 画面からに移動します。 **ツール** > **一般** > **GraphQL クエリエディター**

1. 次をタップします。 *all-teams* 永続クエリパネルからをタップし、 **公開**

   ![永続クエリの公開](assets/explore-graphql-api/publish-persisted-query.png)

1. 上の手順を繰り返します： `person-by-name` クエリ

## ソリューションファイル {#solution-files}

最後の 3 つの章で作成したコンテンツ、モデル、永続クエリをダウンロードします。 [tutorial-solution-content.zip](assets/explore-graphql-api/tutorial-solution-content.zip)

## その他のリソース

GraphQL クエリのその他の例の多くは、次を参照してください。 [AEMでの GraphQL の使用方法の学習 — サンプルコンテンツとクエリ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html).

## おめでとうございます。 {#congratulations}

おめでとうございます。複数の GraphQL クエリを作成して実行しました。

## 次の手順 {#next-steps}

次の章では、 [React アプリの作成](./graphql-and-react-app.md)を使用して、外部アプリケーションがAEM GraphQL エンドポイントに対してクエリを実行し、これら 2 つの永続的なクエリを活用する方法を調べます。 また、基本的なエラー処理についても説明します。

## GraphiQL ツールのインストール（オプション） {#install-graphiql}

AEMの一部のバージョンでは、GraphiQL IDE ツールを手動でインストールする必要があります。 手動でインストールするには、次の手順に従います。

1. **[ソフトウェア配布ポータル](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)**／**AEM as a Cloud Service** に移動します。
1. 「GraphiQL」を検索します（**GraphiQL** の **i** は必ず入れてください）。
1. 最新の **GraphiQL コンテンツパッケージ v.x.x.x** をダウンロードします。

   ![GraphiQL パッケージをダウンロード](assets/explore-graphql-api/software-distribution.png)

   zip ファイルは、直接インストールできるAEMパッケージです。

1. **AEM 開始**&#x200B;メニューで&#x200B;**ツール**／**デプロイメント**／**パッケージ**&#x200B;に移動します。
1. 「**パッケージをアップロード**」をクリックし、前の手順でダウンロードしたパッケージを選択します。「**インストール**」をクリックして、パッケージをインストールします。

   ![GraphiQL パッケージのインストール](assets/explore-graphql-api/install-graphiql-package.png)
