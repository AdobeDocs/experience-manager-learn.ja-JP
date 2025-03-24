---
title: iOS App - AEM ヘッドレスの例
description: サンプルアプリケーションは、Adobe Experience Manager（AEM）のヘッドレス機能を調べるうえで役に立ちます。 この iOS アプリケーションでは、永続クエリを使用して AEM の GraphQL API でコンテンツに対してクエリを実行する方法を示します。
version: Experience Manager as a Cloud Service
mini-toc-levels: 2
jira: KT-10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
duration: 278
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 100%

---

# iOS アプリ

サンプルアプリケーションは、Adobe Experience Manager（AEM）のヘッドレス機能を調べるうえで役に立ちます。 この iOS アプリケーションでは、永続クエリを使用して AEM の GraphQL API でコンテンツに対してクエリを実行する方法を示します。

![AEM ヘッドレスを備えた iOS SwiftUI アプリ](./assets/ios-swiftui-app/ios-app.png)

[GitHub のソースコード](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)を表示

## 前提条件 {#prerequisites}

次のツールをローカルにインストールする必要があります。

+ [Xcode](https://developer.apple.com/xcode/)（macOS が必要）
+ [Git](https://git-scm.com/)

## AEM の要件

iOS アプリケーションは、次の AEM デプロイメントオプションと連携します。すべてのデプロイメントに [WKND Site v3.0.0 以降](https://github.com/adobe/aem-guides-wknd/releases/latest)をインストールする必要があります。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=ja)
+ [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja) を使用したローカル設定

iOS アプリケーションは __AEM パブリッシュ__&#x200B;環境に接続するように設計されていますが、iOS アプリケーションの設定で認証が提供されている場合、AEM オーサーからコンテンツを取得できます。

## 使用方法

1. `adobe/aem-guides-wknd-graphql` リポジトリのクローンを作成します。

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. [Xcode](https://developer.apple.com/xcode/)、フォルダー `ios-app` の順に開く
1. `Config.xcconfig` ファイルを変更し、ターゲットの AEM パブリッシュサービスに一致するように `AEM_SCHEME` と `AEM_HOST` を更新します。

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   AEM オーサーに接続する場合は、`AEM_AUTH_TYPE` およびサポートする認証プロパティを `Config.xcconfig` に追加します。

   __基本認証__

   `AEM_USERNAME` と `AEM_PASSWORD` は、WKND GraphQL コンテンツにアクセスできるローカル AEM ユーザーを認証します。

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __トークン認証__

   `AEM_TOKEN` は[アクセストークン](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=ja)であり、WKND GraphQL コンテンツへのアクセス権を持つ AEM ユーザーを認証します。

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Xcode を使用してアプリケーションを作成し、アプリケーションを iOS シミュレーターにデプロイします。
1. WKND サイトからのアドベンチャーのリストがアプリケーションに表示されます。アドベンチャーを選択すると、アドベンチャーの詳細が開きます。アドベンチャーリストビューで、AEM からのデータを取り込んで更新します。

## コード

以下は、iOS アプリケーションの構築方法、AEM ヘッドレスに接続して GraphQL 永続クエリを使用してコンテンツを取得する方法、およびそのデータを表示する方法の概要です。完全なコードは [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) にあります。

### 永続クエリ

AEM ヘッドレスのベストプラクティスに従って、iOS アプリケーションは AEM GraphQL 永続クエリを使用してアドベンチャーデータをクエリします。アプリケーションでは、次の 2 つの永続クエリを使用します。

+ `wknd/adventures-all` 永続クエリ。AEM のすべてのアドベンチャーを要約されたプロパティセットとともに返します。この永続クエリは、初期ビューのアドベンチャーリストを制御します。

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }

query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ `wknd/adventure-by-slug` 永続クエリ。`slug`（アドベンチャーを一意に識別するカスタムプロパティ）によって、完全なプロパティセットを含む単一のアドベンチャーを返します。この永続クエリで、アドベンチャーの詳細ビューが強化されます。

```
query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
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
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
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

### GraphQL 永続クエリの実行

AEM の永続クエリは HTTP GET で実行されるので、Apollo などの HTTP POST を使用する一般的な GraphQL ライブラリは使用できません。代わりに、AEM に対する永続クエリ HTTP GET リクエストを実行するカスタムクラスを作成します。

`AEM/Aem.swift` は AEMヘッドレスとのすべてのやり取りに使用される `Aem` クラスをインスタンス化します。パターンは次のとおりです。

1. 各永続クエリには、対応するパブリック関数（例：`getAdventures(..)` または `getAdventureBySlug(..)`）があり、iOS アプリケーションのビューが呼び出されて、アドベンチャーデータを取得します。
1. パブリック関数は、AEM ヘッドレスへの非同期 HTTP GET リクエストを呼び出すプライベート関数 `makeRequest(..)` を呼び出し、JSON データを返します。
1. 次に、各パブリック関数は JSON データをデコードし、必要なチェックや変換を行ってから、アドベンチャーデータをビューに返します。

   + AEM の GraphQL JSON データは、`AEM/Models.swift` で定義された構造体／クラスを使用してデコードされます。これは、AEM ヘッドレスから返された JSON オブジェクトにマップされます。

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(params: [String:String], completion: @escaping ([Adventure]) ->  ()) {
               
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all", params: params)
        
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            } else if (!data!.isEmpty) {
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                DispatchQueue.main.async {
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

iOS では、JSON オブジェクトを型指定されたデータモデルにマッピングすることが好まれています。

`src/AEM/Models.swift` は、AEM の JSON 応答によって返される AEM JSON 応答にマップされる [decodable](https://developer.apple.com/documentation/swift/decodable) Swift 構造体とクラスを定義します。

### ビュー

SwiftUI は、アプリケーションの様々な表示で使用されます。 Apple は、[SwiftUI を使用してリストとナビゲーションを構築する](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)ための入門チュートリアルを提供しています。

+ `WKNDAdventuresApp.swift`

  アプリケーションのエントリには `AdventureListView` が含まれ、その `.onAppear` イベントハンドラーは `aem.getAdventures()` を介してすべてのアドベンチャーデータを取得するために使用されます。共有 `aem` オブジェクトはここで初期化され、他のビューには [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject) として公開されます。

+ `Views/AdventureListView.swift`

  （`aem.getAdventures()` からのデータに基づく）アドベンチャーのリストを表示し、`AdventureListItemView` を使用している各アドベンチャーのリスト項目を表示します。

+ `Views/AdventureListItemView.swift`

  アドベンチャーリストの各項目を表示します（`Views/AdventureListView.swift`）。

+ `Views/AdventureDetailView.swift`

  タイトル、説明、価格、アクティビティタイプ、プライマリ画像など、アドベンチャーの詳細を表示します。 このビューは、`aem.getAdventureBySlug(slug: slug)` を使用して完全なアドベンチャーの詳細のクエリを AEM に対して実行します。ここで、`slug` パラメーターは選択リストの行に基づいて渡されます。

### リモート画像

アドベンチャーコンテンツフラグメントで参照される画像は、AEM が提供します。 この iOS アプリは、GraphQL 応答のパス `_dynamicUrl` フィールドを使用し、プレフィックス `AEM_SCHEME` と `AEM_HOST` を付けて完全修飾 URL を作成します。AEM SDK に対応した開発の場合、`_dynamicUrl` は null を返すので、画像の `_path` フィールドにフォールバックします。

認証が必要な AEM 上の保護されたリソースに接続する場合は、資格情報も画像リクエストに追加する必要があります。

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) および [SDWebImage](https://github.com/SDWebImage/SDWebImage) は、`AdventureListItemView` 表示と `AdventureDetailView` 表示に アドベンチャー画像を取り込む AEM からリモート画像を読み込むために使用されます。

`aem`クラス（`AEM/Aem.swift`）は、次の 2 つの方法で AEM 画像の使用を容易にします。

1. `aem.imageUrl(path: String)` はビューで使用され、AEM のスキームとホストを画像のパスに追加して、完全修飾 URL を作成します。

   ```swift
   // adventure.image() => /adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   ```

2. `Aem` の `convenience init(..)` は、iOS アプリケーションの設定に基づいて、画像 HTTP リクエストに HTTP 認証ヘッダーを設定します。

   + __基本認証__&#x200B;が設定されている場合、すべての画像リクエストで基本認証が行われます。

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

   + __トークン認証__&#x200B;が設定されている場合、すべての画像リクエストでトークン認証が行われます。

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

   + __認証なし__&#x200B;が設定されている場合、画像リクエストで認証は行われません。

SwiftUI ネイティブの [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage) でも同様のアプローチを使用できます。`AsyncImage` は iOS 15.0 以降でサポートされています。

## その他のリソース

+ [AEM ヘッドレス入門 - GraphQL チュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=ja)
+ [SwiftUI リストとナビゲーションチュートリアル](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
