---
title: AEM GraphQL API - AEMヘッドレスの高度な概念 — GraphQL
description: GraphiQL IDE を使用して GraphQL クエリを送信します。 フィルター、変数およびディレクティブを使用した詳細クエリについて説明します。 複数行テキストフィールドからの参照を含む、フラグメントおよびコンテンツ参照のクエリ。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: bd7916be-8caa-4321-add0-4c9031306d60
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '1322'
ht-degree: 0%

---

# AEM GraphQL API の参照

AEMの GraphQL API を使用すると、コンテンツフラグメントデータをダウンストリームアプリケーションに公開できます。 基本チュートリアル内 [複数手順の GraphQL チュートリアル](../multi-step/explore-graphql-api.md)GraphiQL Explorer を使用して、GraphQL クエリをテストし、調整しました。

この章では、GraphiQL エクスプローラーを使用して、で作成したコンテンツフラグメントのデータを収集するためのより高度なクエリを定義します。 [前の章](../advanced-graphql/author-content-fragments.md).

## 前提条件 {#prerequisites}

このドキュメントは、マルチパートチュートリアルの一部です。 この章を進める前に、前の章が完了していることを確認してください。

## 目的 {#objectives}

この章では、次の方法について説明します。

* クエリ変数を使用した参照でコンテンツフラグメントのリストをフィルタリング
* フラグメント参照内のコンテンツのフィルター
* 複数行テキストフィールドからのインラインコンテンツおよびフラグメント参照のクエリ
* ディレクティブを使用したクエリ
* JSON オブジェクトコンテンツタイプのクエリ

## GraphiQL エクスプローラーの使用


この [GraphiQL エクスプローラ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) ツールを使用すると、開発者は、現在のAEM環境のコンテンツに対するクエリを作成およびテストできます。 GraphiQL ツールを使用すると、次のことが可能になります。 **保持するか保存する** 実稼働設定でクライアントアプリケーションで使用されるクエリ。

次に、組み込みの GraphiQL エクスプローラーを使用してAEM GraphQL API の機能を調べます。

1. AEM Start 画面で、に移動します。 **ツール** > **一般** > **GraphQL クエリエディター**.

   ![GraphiQL IDE に移動します。](assets/explore-graphql-api/navigate-graphql-query-editor.png)

>[!IMPORTANT]
>
>AEM (6.X.X) の一部のバージョンでは、GraphiQL Explorer (GraphiQL IDE) ツールを手動でインストールする必要があります。 [ここからの指示](../multi-step/explore-graphql-api.md#install-the-graphiql-tool-optional).

1. 右上隅で、「エンドポイント」が「 **WKND 共有エンドポイント**. 変更 _エンドポイント_ ここに既存の値を表示するドロップダウン値 _永続クエリ_ をクリックします。

   ![GraphQL エンドポイントを設定](assets/explore-graphql-api/set-wknd-shared-endpoint.png)

これにより、 **WKND 共有** プロジェクト。


## クエリ変数を使用したコンテンツフラグメントのリストのフィルタリング

前の [複数手順の GraphQL チュートリアル](../multi-step/explore-graphql-api.md)を定義し、基本的な永続化クエリでコンテンツフラグメントデータを取得して使用しました。 ここでは、この知識を拡張し、永続的なクエリに変数を渡してコンテンツフラグメントデータをフィルタリングします。

クライアントアプリケーションを開発する場合は、通常、動的な引数に基づいてコンテンツフラグメントをフィルタリングする必要があります。 AEM GraphQL API を使用すると、実行時にクライアント側で文字列が構築されるのを避けるために、これらの引数を変数としてクエリに渡すことができます。 GraphQL 変数について詳しくは、 [GraphQL ドキュメント](https://graphql.org/learn/queries/#variables).

この例では、特定のスキルを持つすべての講師に問い合わせます。

1. GraphiQL IDE で、左のパネルに次のクエリを貼り付けます。

   ```graphql
   query listPersonBySkill ($skillFilter: String!){
     personList(
       _locale: "en"
       filter: {skills: {_expressions: [{value: $skillFilter}]}}
     ) {
       items {
         fullName
         contactInfo {
           phone
           email
         }
         profilePicture {
           ... on ImageRef {
             _path
           }
         }
         biography {
           plaintext
         }
         instructorExperienceLevel
         skills
       }
     }
   }
   ```

   この `listPersonBySkill` 上のクエリは 1 つの変数 (`skillFilter`) が必須 `String`. このクエリは、すべてのユーザーコンテンツフラグメントに対して検索を実行し、 `skills` フィールドと、 `skillFilter`.

   この `listPersonBySkill` 次を含む `contactInfo` プロパティ。これは、前の章で定義した連絡先情報モデルへのフラグメント参照です。 連絡先情報モデルには次が含まれます： `phone` および `email` フィールド。 クエリが正しく実行するには、クエリ内のこれらのフィールドの少なくとも 1 つが存在する必要があります。

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. 次に、 `skillFilter` スキーに堪能なインストラクターを全員揃えて。 GraphiQL IDE の「Query Variables」パネルに次の JSON 文字列を貼り付けます。

   ```json
   {
       "skillFilter": "Skiing"
   }
   ```

1. クエリを実行します。 結果は次のようになります。

   ```json
   {
     "data": {
       "personList": {
         "items": [
           {
             "fullName": "Stacey Roswells",
             "contactInfo": {
               "phone": "209-888-0011",
               "email": "sroswells@wknd.com"
             },
             "profilePicture": {
               "_path": "/content/dam/wknd-shared/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer. Born in Baltimore, Maryland, Stacey is the youngest of six children. Stacey's father was a lieutenant colonel in the US Navy and mother was a modern dance instructor. Stacey's family moved frequently with father's duty assignments and took the first pictures when father was stationed in Thailand. This is also where Stacey learned to rock climb."
             },
             "instructorExperienceLevel": "Advanced",
             "skills": [
               "Rock Climbing",
               "Skiing",
               "Backpacking"
             ]
           }
         ]
       }
     }
   }
   ```

を押します。 **再生** ボタンを使用してクエリを実行します。 前の章のコンテンツフラグメントの結果が表示されます。

![技能実績別人](assets/explore-graphql-api/person-by-skill.png)

## フラグメント参照内のコンテンツのフィルター

AEM GraphQL API を使用すると、ネストされたコンテンツフラグメントをクエリできます。 前の章では、アドベンチャーコンテンツフラグメントに 3 つの新しいフラグメント参照を追加しました。 `location`, `instructorTeam`、および `administrator`. 次に、特定の名前を持つ管理者のすべての Adventures をフィルタリングします。

>[!CAUTION]
>
>このクエリが正しく実行されるための参照として許可するモデルは 1 つだけです。

1. GraphiQL IDE で、左のパネルに次のクエリを貼り付けます。

   ```graphql
   query getAdventureAdministratorDetailsByAdministratorName ($name: String!){
     adventureList(
     _locale: "en"
       filter: {administrator: {fullName: {_expressions: [{value: $name}]}}}
     ) {
       items {
         title
         administrator {
           fullName
           contactInfo {
             phone
             email
           }
           administratorDetails {
             json
           }
         }
       }
     }
   }
   ```

1. 次に、次の JSON 文字列をクエリ変数パネルに貼り付けます。

   ```json
   {
       "name": "Jacob Wester"
   }
   ```

   この `getAdventureAdministratorDetailsByAdministratorName` クエリは、任意ののアドベンチャをすべてフィルタリングします `administrator` / `fullName` ネストされた 2 つのコンテンツフラグメントから情報を返す「Jacob Wester」アドベンチャーと講師。

1. クエリを実行します。 結果は次のようになります。

   ```json
   {
     "data": {
       "adventureList": {
         "items": [
           {
             "title": "Yosemite Backpacking",
             "administrator": {
               "fullName": "Jacob Wester",
               "contactInfo": {
                 "phone": "209-888-0000",
                 "email": "jwester@wknd.com"
               },
               "administratorDetails": {
                 "json": [
                   {
                     "nodeType": "paragraph",
                     "content": [
                       {
                         "nodeType": "text",
                         "value": "Jacob Wester has been coordinating backpacking adventures for three years."
                       }
                     ]
                   }
                 ]
               }
             }
           }
         ]
       }
     }
   }
   ```

## 複数行テキストフィールドからのインライン参照のクエリ {#query-rte-reference}

AEM GraphQL API を使用すると、複数行テキストフィールド内のコンテンツおよびフラグメント参照をクエリできます。 前の章では、両方の参照タイプを **説明** Yosemite チームコンテンツフラグメントのフィールド。 次に、これらの参照を取得します。

1. GraphiQL IDE で、左のパネルに次のクエリを貼り付けます。

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata {
             stringMetadata {
               name
               value
             }
         }
           teamFoundingDate
           description {
             plaintext
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   この `getTeamByAdventurePath` クエリは、すべての冒険をパスでフィルタリングし、 `instructorTeam` 特定のアドベンチャーのフラグメント参照。

   `_references` は、複数行テキストフィールドに挿入された参照を含む、参照を表示するために使用される、システム生成フィールドです。

   この `getTeamByAdventurePath` クエリは複数の参照を取得します。 まず、組み込みの `ImageRef` 取得するオブジェクト `_path` および `__typename` 複数行テキストフィールドへのコンテンツ参照として挿入された画像の数。 次に、 `LocationModel` を使用して、同じフィールドに挿入された場所コンテンツフラグメントのデータを取得します。

   クエリには、 `_metadata` フィールドに入力します。 これにより、チームコンテンツフラグメントの名前を取得し、後で WKND アプリに表示することができます。

1. 次に、クエリ変数パネルに次の JSON 文字列を貼り付けて、Yosemite Backpacking Adventure を取得します。

   ```json
   {
       "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. クエリを実行します。 結果は次のようになります。

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge"
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   この `_references` フィールドには、ロゴイメージと、 **説明** フィールドに入力します。


## ディレクティブを使用したクエリ

クライアントアプリケーションを開発する際に、クエリの構造を条件付きで変更する必要が生じる場合があります。 この場合、AEM GraphQL API を使用すると、GraphQL ディレクティブを使用して、提供された条件に基づいてクエリの動作を変更できます。 GraphQL ディレクティブについて詳しくは、 [GraphQL ドキュメント](https://graphql.org/learn/queries/#directives).

内 [前のセクション](#query-rte-reference)では、複数行テキストフィールド内でインライン参照をクエリする方法を学習しました。 コンテンツは `description` ～に提出される `plaintext` 形式 次に、そのクエリを展開し、ディレクティブを使用して条件付きで取得します `description` 内 `json` 形式も同様です。

1. GraphiQL IDE で、左のパネルに次のクエリを貼り付けます。

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!, $includeJson: Boolean!){
     adventureByPath(_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata{
             stringMetadata{
               name
               value
             }
           }
           teamFoundingDate
           description {
             plaintext
             json @include(if: $includeJson)
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   上記のクエリは、1 つ以上の変数 (`includeJson`) が必須 `Boolean`は、クエリのディレクティブとも呼ばれます。 ディレクティブは、 `description` フィールド `json` 渡されるブール値に基づく形式 `includeJson`.

1. 次に、次の JSON 文字列をクエリ変数パネルに貼り付けます。

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. クエリを実行します。 前の節のと同じ結果が得られます。 [複数行テキストフィールド内でインライン参照をクエリする方法](#query-rte-reference).

1. を更新します。 `includeJson` 指示に従う `true` をクリックし、クエリを再実行します。 結果は次のようになります。

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge",
               "json": [
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
                         "mimetype": "image/png"
                       }
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "text",
                       "value": "The team of professional adventurers and hiking instructors working in Yosemite National Park."
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "href": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
                         "type": "fragment"
                       },
                       "value": "Yosemite Valley Lodge"
                     }
                   ]
                 }
               ]
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

## JSON オブジェクトコンテンツタイプのクエリ

コンテンツフラグメントのオーサリングに関する前の章で、JSON オブジェクトを **シーズン別の天気** フィールドに入力します。 次に、場所コンテンツフラグメント内でそのデータを取得します。

1. GraphiQL IDE で、左のパネルに次のクエリを貼り付けます。

   ```graphql
   query getLocationDetailsByLocationPath ($fragmentPath: String!) {
     locationByPath(_path: $fragmentPath) {
       item {
         name
         description {
           json
         }
         contactInfo {
           phone
           email
         }
         locationImage {
           ... on ImageRef {
             _path
           }
         }
         weatherBySeason
         address {
           streetAddress
           city
           state
           zipCode
           country
         }
       }
     }
   }
   ```

1. 次に、次の JSON 文字列をクエリ変数パネルに貼り付けます。

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park"
   }
   ```

1. クエリを実行します。 結果は次のようになります。

   ```json
   {
     "data": {
       "locationByPath": {
         "item": {
           "name": "Yosemite National Park",
           "description": {
             "json": [
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Yosemite National Park is in California's Sierra Nevada mountains. It's famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
                   }
                 ]
               },
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Hiking and camping are the best ways to experience Yosemite. Numerous trails provide endless opportunities for adventure and exploration."
                   }
                 ]
               }
             ]
           },
           "contactInfo": {
             "phone": "209-999-0000",
             "email": "yosemite@wknd.com"
           },
           "locationImage": {
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
           },
           "weatherBySeason": {
             "summer": "81 / 89°F",
             "fall": "56 / 83°F",
             "winter": "46 / 51°F",
             "spring": "57 / 71°F"
           },
           "address": {
             "streetAddress": "9010 Curry Village Drive",
             "city": "Yosemite Valley",
             "state": "CA",
             "zipCode": "95389",
             "country": "United States"
           }
         }
       }
     }
   }
   ```

   この `weatherBySeason` フィールドには、前の章で追加した JSON オブジェクトが含まれます。

## すべてのコンテンツを一度にクエリ

これまでに、AEM GraphQL API の機能を説明するために複数のクエリが実行されてきました。

同じデータを 1 つのクエリでのみ取得でき、このクエリは後でクライアントアプリケーションで使用され、場所、チーム名、アドベンチャーのチームメンバーなどの追加情報を取得します。

```graphql
query getAdventureDetailsBySlug($slug: String!) {
  adventureList(filter: {slug: {_expressions: [{value: $slug}]}}) {
    items {
      _path
      title
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      primaryImage {
        ... on ImageRef {
          _path
          mimeType
          width
          height
        }
      }
      description {
        html
        json
      }
      itinerary {
        html
        json
      }
      location {
        _path
        name
        description {
          html
          json
        }
        contactInfo {
          phone
          email
        }
        locationImage {
          ... on ImageRef {
            _path
          }
        }
        weatherBySeason
        address {
          streetAddress
          city
          state
          zipCode
          country
        }
      }
      instructorTeam {
        _metadata {
          stringMetadata {
            name
            value
          }
        }
        teamFoundingDate
        description {
          json
        }
        teamMembers {
          fullName
          contactInfo {
            phone
            email
          }
          profilePicture {
            ... on ImageRef {
              _path
            }
          }
          instructorExperienceLevel
          skills
          biography {
            html
          }
        }
      }
      administrator {
        fullName
        contactInfo {
          phone
          email
        }
        biography {
          html
        }
      }
    }
    _references {
      ... on ImageRef {
        _path
        mimeType
      }
      ... on LocationModel {
        _path
        __typename
      }
    }
  }
}


# in Query Variables
{
  "slug": "yosemite-backpacking"
}
```

## おめでとうございます。

おめでとうございます。これで、前の章で作成したコンテンツフラグメントのデータを収集するための高度なクエリをテストしました。

## 次の手順

内 [次の章](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md)では、GraphQL クエリを永続化する方法と、永続化されたクエリをアプリケーションで使用することがベストプラクティスである理由を学びます。
