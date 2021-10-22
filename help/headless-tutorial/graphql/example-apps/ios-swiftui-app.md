---
title: iOS SwiftUI アプリ — AEMヘッドレスの例
description: サンプルアプリケーションは、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 AEMの GraphQL API を使用してコンテンツをクエリする方法を示すiOSアプリケーションが提供されます。 Apollo Client iOSは、GraphQL クエリを生成し、データを Swift オブジェクトにマッピングしてアプリを強化するために使用されます。 SwiftUI は、コンテンツの単純なリスト表示と詳細表示をレンダリングするために使用されます。
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9b1e38c8d4a0301c124c6f1607a9e4362b0e9cd1
workflow-type: tm+mt
source-wordcount: '960'
ht-degree: 4%

---


# iOS SwiftUI アプリ

サンプルアプリケーションは、Adobe Experience Manager(AEM) のヘッドレス機能を調べる優れた方法です。 このiOSアプリケーションは、AEMの GraphQL API を使用してコンテンツをクエリする方法を示します。 Apollo Client iOSは、GraphQL クエリを生成し、データを Swift オブジェクトにマッピングしてアプリを強化するために使用されます。 SwiftUI は、コンテンツの単純なリスト表示と詳細表示をレンダリングするために使用されます。

表示 [GitHub のソースコード](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-swiftui-app)

>[!VIDEO](https://video.tv.adobe.com/v/338042/?quality=12&learn=on)

## 前提条件 {#prerequisites}

次のツールは、ローカルにインストールする必要があります。

* [Xcode 9.3 以降](https://developer.apple.com/xcode/)
* [Git](https://git-scm.com/)

## AEM要件

アプリケーションは、AEMに接続するように設計されています **公開** 最新リリースの [WKND リファレンスサイト](https://github.com/adobe/aem-guides-wknd/releases/latest) インストール済み

* [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=ja)

推奨 [WKND リファレンスサイトのCloud Service環境へのデプロイ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). を使用したローカル設定 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja) または [AEM 6.5 QuickStart jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja?lang=en#install-local-aem-instances) また、を使用することもできます。

## 使用方法

1. を `aem-guides-wknd-graphql` リポジトリ：

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) フォルダを開きます。 `ios-swiftui-app`
1. ファイルの変更 `Config.xcconfig` ファイルと更新 `AEM_HOST` ターゲットの AEM パブリッシュ環境に合わせる

   ```plain
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   // GraphQL Endpoint
   AEM_GRAPHQL_ENDPOINT = /content/cq:graphql/wknd/endpoint.json
   ```

1. Xcode を使用してアプリケーションを構築し、iOSシミュレーターにアプリをデプロイします。
1. WKND リファレンスサイトからのアドベンチャーのリストがアプリケーションに表示されます。

## コード

以下に、アプリケーションの強化に使用する重要なファイルとコードの概要を示します。 完全なコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-swiftui-app).

### アポロiOS

10. [アポロiOS](https://www.apollographql.com/docs/ios/) クライアントは、アプリでAEMに対して GraphQL クエリを実行するために使用されます。 役人は [Apollo Tutorial](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/) のインストールおよび使用方法の詳細がはるかに多くあります。

`schema.json` は、WKND リファレンスサイトがインストールされているAEM環境の GraphQL スキーマを表すファイルです。 `schema.json` がAEMからダウンロードされ、プロジェクトに追加された場合。 Apollo クライアントは、拡張子がのファイルを調べます `.graphql` をカスタムビルドフェーズの一部として追加します。 Apollo クライアントは、次に `schema.json` ファイルおよび `.graphql` ファイルを自動的に生成するクエリ `API.swift`.

これにより、クエリを実行するための厳密に型指定されたモデルと、結果を表すモデルがアプリケーションに提供されます。

![Xcode カスタムビルドフェーズ](assets/ios-swiftui-app/xcode-build-phase-apollo.png)

`AdventureList.graphql` は、アドベンチャーのクエリに使用するクエリを含みます。

```
query AdventureList
{
  adventureList {
    items {
      _path
      adventureTitle
      adventurePrice
      adventureActivity
      adventureDescription {
        plaintext
        markdown
      }
      adventureDifficulty
      adventureTripLength
      adventurePrimaryImage {
        ...on ImageRef {
          _authorUrl
          _publishUrl
        }
      }
    }
  }
}
```

`Network.swift` を構成します。 `ApolloClient`. 10. `endpointURL` 使用は、 `Config.xcconfig` ファイル。 AEM **作成者** インスタンスを作成し、認証用のヘッダーを追加する必要がある場合は、 `ApolloClient` はい。

```swift
// Network.swift
private(set) lazy var apollo: ApolloClient = {
        // The cache is necessary to set up the store, which we're going to hand to the provider
        let cache = InMemoryNormalizedCache()
        let store = ApolloStore(cache: cache)
  
        let client = URLSessionClient()
        let provider = DefaultInterceptorProvider(client: client, shouldInvalidateClientOnDeinit: true, store: store)
        let url = Connection.baseURL // from Configx.xcconfig 

        // no additional headers, public instances by default require no additional authentication
        let requestChainTransport = RequestChainNetworkTransport(interceptorProvider: provider, endpointURL: url)

        return ApolloClient(networkTransport: requestChainTransport,store: store)
    }()
}
```

### アドベンチャーデータ

このアプリケーションは、Adventures のリストと各アドベンチャーの詳細ビューを表示するように設計されています。

`AdventuresDataModel.swift` は、関数を含むクラスです `fetchAdventures()`. この関数は、 `ApolloClient` をクリックしてクエリを実行します。 クエリが成功した場合、結果の配列の型は `AdventureListQuery.Data.AdventureList.Item`、 `API.swift` ファイル。

```swift
func fetchAdventures() {
        Network.shared.apollo
            //AdventureListQuery() generated based on AdventureList.graphql file
           .fetch(query: AdventureListQuery()) { [weak self] result in
           
             guard let self = self else {
               return
             }
                   
             switch result {
             case .success(let graphQLResult):
                print("Success AdventureListQuery() from: \(graphQLResult.source)")

                if let adventureDataItems =  graphQLResult.data?.adventureList.items {
                    // map graphQL items to an array of Adventure objects
                    self.adventures = adventureDataItems.compactMap { Adventure(adventureData: $0!) }
                }
                ...
             }
           }
}
```

以下を使用できます。 `AdventureListQuery.Data.AdventureList.Item` を直接呼び出して、アプリケーションの電源を入れます。 ただし、一部のデータが不完全なため、一部のプロパティが null になる可能性は非常に高くなります。

`Adventure.swift` は、Apollo で生成されたモデルのラッパーとして導入されたカスタムモデルです。 `Adventure` は `AdventureListQuery.Data.AdventureList.Item`. A `typealias` は、を短縮してコードを読みやすくするために使用します。

```
// use typealias
typealias AdventureData = AdventureListQuery.Data.AdventureList.Item
```

10. `Adventure` 構造体は `AdventureData` オブジェクト：

```swift
struct Adventure: Identifiable {
    let id: String
    let adventureTitle: String
    let adventurePrice: String
    let adventureDescription: String
    let adventureActivity: String
    let adventurePrimaryImageUrl: String
    
    // initialize with AdventureData object aka AdventureListQuery.Data.AdventureList.Item
    init(adventureData: AdventureData) {
        // use path as unique idenitifer, otherwise
        self.id = adventureData._path ?? UUID().uuidString
        self.adventureTitle = adventureData.adventureTitle ?? "Untitled"
        self.adventurePrice = adventureData.adventurePrice ?? "Free"
        self.adventureActivity = adventureData.adventureActivity ?? ""
        ...
```

これにより、デフォルト値を指定し、不完全なデータに対して追加のチェックを実行できます。 その後、 `Adventure` 様々な UI 要素を安全に使用できるようにモデルを作成し、null 値を常に確認する必要はありません。

AEMでは、コンテンツの一部は `_path`. In `Adventure.swift` 我々は `id` プロパティの値が `_path`. これにより、 `Adventure` 実行する `Identifiable` インターフェイスを使用して、配列やリストを繰り返し処理しやすくします。

### 表示

SwiftUI は、アプリケーションの様々なビューに使用されます。 向けの優れたチュートリアル [構築リストとナビゲーション](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation) は、Appleの開発者サイトで確認できます。 このアプリケーションのコードは、大まかにそこから導き出されています。

`WKNDAdventuresApp.swift` は、アプリケーションのエントリです。 これには以下が含まれます。 `AdventureListView` そして `.onAppear` イベントは、アドベンチャーデータを取得するために使用されます。

`AdventureListView.swift`  — を作成します。 `NavigationView` そして人が生み出した冒険のリスト `AdventureRowView`. ナビゲーション `AdventureDetailView` はここで設定します。

`AdventureRowView`  — アドベンチャーのプライマリ画像とアドベンチャータイトルを一列に表示します。

`AdventureDetailView`  — タイトル、説明、価格、アクティビティタイプ、プライマリ画像など、個々のアドベンチャーの完全な詳細を表示します。

Apollo CLI が実行され、再生されると `API.swift` プレビューを停止します。 自動プレビュー機能を使用するには、 **アポロ CLI** ビルドフェーズを実行し、スクリプトを実行するかどうかを確認します。 **ビルドのインストールのみ**.

![インストールビルドのみを確認](assets/ios-swiftui-app/update-build-phases.png)

### リモートイメージ

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) および [SDWEbImage](https://github.com/SDWebImage/SDWebImage) は、行ビューと詳細ビューのアドベンチャープライマリイメージを生成するAEMからリモートイメージを読み込むために使用されます。

10. [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage) は、SwiftUI のネイティブなビューであり、このビューも使用できます。 `AsyncImage` は、iOS 15.0 以降でのみサポートされます。

## その他のリソース

* [AEMヘッドレスの概要 — GraphQL チュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=ja)
* [SwiftUI のリストとナビゲーションのチュートリアル](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
* [Apollo iOS Client チュートリアル](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/)

