---
title: iOS App - AEMヘッドレスの例
description: アプリケーション例は、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 このiOSアプリケーションでは、永続的なクエリを使用してAEM GraphQL API を使用してコンテンツに対してクエリを実行する方法を示します。
version: Cloud Service
mini-toc-levels: 2
kt: 10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: cd7cb89f407f5e0c465544593563534472daf928
workflow-type: tm+mt
source-wordcount: '981'
ht-degree: 5%

---

# iOSアプリ

アプリケーション例は、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 このiOSアプリケーションでは、永続的なクエリを使用してAEM GraphQL API を使用してコンテンツに対してクエリを実行する方法を示します。

![AEMヘッドレスを備えたiOS SwiftUI アプリ](./assets/ios-swiftui-app/ios-app.png)

次を表示： [GitHub のソースコード](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## 前提条件 {#prerequisites}

以下のツールをローカルにインストールする必要があります。

+ [Xcode 9.3 以降](https://developer.apple.com/xcode/) (macOSが必要 )
+ [Git](https://git-scm.com/)

## AEM要件

iOSアプリケーションは、次のAEMデプロイメントオプションと連携します。 すべてのデプロイメントには、 [WKND サイト v2.0.0 以降](https://github.com/adobe/aem-guides-wknd/releases/latest) をインストールします。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=ja)
+ を使用したローカル設定 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja?lang=en#install-local-aem-instances)

iOSアプリケーションは、 __AEM パブリッシュ__ 環境ですが、iOSアプリケーションの設定で認証が指定されている場合は、AEM オーサーからコンテンツをソース化できます。

## 使用方法

1. のクローン `adobe/aem-guides-wknd-graphql` リポジトリ：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 起動 [Xcode](https://developer.apple.com/xcode/) フォルダーを開きます。 `ios-app`
1. ファイルを変更します `Config.xcconfig` ファイルと更新 `AEM_SCHEME` および `AEM_HOST` を追加して、ターゲットの AEM パブリッシュサービスに合わせます。

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = http
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   ```

   AEM オーサーに接続する場合は、 `AEM_AUTH_TYPE` 認証プロパティを `Config.xcconfig`.

   __基本認証__

   この `AEM_USERNAME` および `AEM_PASSWORD` は、WKND GraphQL コンテンツへのアクセス権を持つローカルAEMユーザーを認証します。

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __トークン認証__

   この `AEM_TOKEN` は [アクセストークン](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) WKND GraphQL コンテンツへのアクセス権を持つAEMユーザーに対して認証をおこなう

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Xcode を使用してアプリケーションをビルドし、アプリケーションをiOSシミュレーターにデプロイします。
1. WKND サイトからの冒険のリストがアプリケーションに表示されるはずです。 アドベンチャーを選択すると、アドベンチャーの詳細が開きます。 Adventures リストビューで、プルしてAEMからデータを更新します。

## コード

iOSアプリケーションの構築方法、GraphQL での永続クエリを使用してコンテンツを取得するためAEMヘッドレスに接続する方法、およびそのデータの表示方法の概要を次に示します。 完全なコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### 永続クエリ

AEMヘッドレスのベストプラクティスに従い、iOSアプリケーションはAEM GraphQL に永続化されたクエリを使用して、アドベンチャーデータをクエリします。 アプリケーションでは、次の 2 つの永続クエリを使用します。

+ `wknd/adventures-all` 持続的なクエリで、AEM内のすべてのアドベンチャを簡潔なプロパティセットで返します。 この永続的なクエリは、初期ビューのアドベンチャーリストを駆動します。

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` 次の条件を満たす 1 つのアドベンチャーを返す永続クエリ `slug` 一連のプロパティを持つ（アドベンチャーを一意に識別するカスタムプロパティ）。 この永続的なクエリは、アドベンチャーの詳細表示を強化します。

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
      }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### GraphQL 永続クエリを実行

AEMの永続化クエリは HTTPGET上で実行されるので、Apollo などの HTTPPOSTを使用する一般的な GraphQL ライブラリは使用できません。 代わりに、AEMに対する永続化されたクエリ HTTPGETリクエストを実行するカスタムクラスを作成します。

`AEM/Aem.swift` をインスタンス化します。 `Aem` AEMヘッドレスとのすべてのやり取りに使用されるクラス。 パターンは次のようになります。

1. 永続化された各クエリには、対応するパブリック関数 ( 例： `getAdventures(..)` または `getAdventureBySlug(..)`) iOSアプリケーションのビューが呼び出されて、アドベンチャーデータを取得します。
1. public func は private func を呼び出します。 `makeRequest(..)` を呼び出すと、AEMヘッドレスに対する非同期 HTTPGETリクエストが呼び出され、JSON データが返されます。
1. 次に、各 public func は JSON データをデコードし、必要なチェックや変換を行ってから、アドベンチャーデータをビューに返します。

+ AEM GraphQL JSON データは、 `AEM/Models.swift`:JSON オブジェクトにマッピングされ、AEMヘッドレスが返されました。

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(completion: @escaping ([Adventure]) ->  ()) {
               
        // Create the HTTP request object representing the persisted query to get all adventures
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all")
        
        // Wait fo the HTTP request to return
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            // Error check as needed
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            }
                                    
            if (!data!.isEmpty) {
                // Decode the JSON data into Swift objects
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                
                DispatchQueue.main.async {
                    // Return the array of Adventure objects
                    completion(adventures.data.adventureList.items)
                }
            }
        }.resume();
    }

    ...

    /// #makeRequest(..)
    /// Generic method for constructing and executing AEM GraphQL persisted queries
    private func makeRequest(persistedQueryName: String, params: [String: String] = [:]) -> URLRequest {
        // Encode optional parameters as required by AEM
        let persistedQueryParams = params.map { (param) -> String in
            encode(string: ";\(param.key)=\(param.value)")
        }.joined(separator: "")
        
        // Construct the AEM GraphQL persisted query URL, including optional query params
        let url: String = "\(self.scheme)://\(self.host)/graphql/execute.json/" + persistedQueryName + persistedQueryParams;

        var request = URLRequest(url: URL(string: url)!);

        // Add authentication to the AEM GraphQL persisted query requests as defined by the iOS application's configuration
        request = addAuthHeaders(request: request)
        
        return request
    }
    
    ...
```

### GraphQL 応答データモデル

iOSでは、JSON オブジェクトを型指定されたデータモデルにマッピングすることをお勧めします。

この `src/AEM/Models.swift` は、 [脱落可能な](https://developer.apple.com/documentation/swift/decodable) AEM JSON 応答で返されるAEM JSON 応答にマッピングする Swift の構造およびクラス。

### 表示

SwiftUI は、アプリケーションの様々なビューに使用されます。 Appleは、 [SwiftUI を使用したリストの作成とナビゲーション](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

   アプリケーションのエントリで、を含む `AdventureListView` その `.onAppear` イベントハンドラーは、を介してすべての冒険データを取得するために使用されます。 `aem.getAdventures()`. 共有 `aem` オブジェクトはここで初期化され、他のビューには [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

   （からのデータに基づいて）冒険のリストを表示します。 `aem.getAdventures()`) をクリックし、 `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

   冒険リストの各項目を表示します (`Views/AdventureListView.swift`) をクリックします。

+ `Views/AdventureDetailView.swift`

   タイトル、説明、価格、アクティビティタイプ、プライマリ画像など、アドベンチャーの詳細を表示します。 このビューは、AEMに対して、 `aem.getAdventureBySlug(slug: slug)`( `slug` パラメーターは、選択リスト行に基づいて渡されます。

### リモート画像

アドベンチャーコンテンツフラグメントで参照される画像は、AEMが提供します。 このiOSアプリはパスを使用します `_path` GraphQL 応答のフィールドに入力し、 `AEM_SCHEME` および `AEM_HOST` をクリックして完全修飾 URL を作成します。

認証が必要なAEM上の保護されたリソースに接続する場合は、イメージリクエストに資格情報も追加する必要があります。

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) および [SDWebImage](https://github.com/SDWebImage/SDWebImage) を使用して、アドベンチャー画像をAEMから読み込み、 `AdventureListItemView` および `AdventureDetailView` ビュー。

この `aem` クラス ( `AEM/Aem.swift`) は、次の 2 つの方法でAEM画像の使用を容易にします。

1. `aem.imageUrl(path: String)` を使用すると、AEMスキームを画像のパスの前に追加し、画像のパスをホストして、完全修飾 URL を作成できます。

   ```swift
   // adventure.image() => /content/dam/path/to/an/image.png
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => http://localhost:4503/content/dam/path/to/an/image.png
   ```

2. この `convenience init(..)` in `Aem` iOSアプリケーションの設定に基づいて、イメージ HTTP リクエストに HTTP Authorization ヘッダーを設定します。

   + If __基本認証__ が設定されている場合、すべてのイメージリクエストに基本認証が添付されます。

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Basic authentication init
   /// Used when authenticating to AEM using local accounts (basic auth)
   convenience init(scheme: String, host: String, username: String, password: String) {
       ...
   
       // Add basic auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Basic \(encodeBasicAuth(username: username, password: password))", forHTTPHeaderField: "Authorization")
   }
   ```

   + If __トークン認証__ が設定されている場合、トークン認証がすべてのイメージリクエストに添付されます。

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Token authentication init
   ///  Used when authenticating to AEM using token authentication (Dev Token or access token generated from Service Credentials)
   convenience init(scheme: String, host: String, token: String) {
       ...
   
       // Add token auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Bearer \(token)", forHTTPHeaderField: "Authorization")
   }
   ```

   + If __認証なし__ が設定されている場合、イメージリクエストに認証は添付されません。



SwiftUI ネイティブの [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` はiOS 15.0 以降でサポートされています。

## その他のリソース

+ [AEMヘッドレスの概要 — GraphQL チュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [SwiftUI リストとナビゲーションチュートリアル](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
