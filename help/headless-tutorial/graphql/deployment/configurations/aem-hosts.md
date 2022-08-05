---
title: AEM GraphQL 用AEMホストの管理
description: AEMヘッドレスアプリでAEMホストを設定する方法について説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10831
thumbnail: KT-10831.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '1712'
ht-degree: 0%

---


# AEMホストの管理

AEMヘッドレスアプリケーションのデプロイでは、AEM URL の構築方法に注意して、正しいAEMホスト/ドメインが使用されるようにする必要があります。 認識する必要のあるプライマリ URL/リクエストタイプは次のとおりです。

+ への HTTP リクエスト __[AEM GraphQL API](#aem-graphql-api-requests)__
+ __[画像 URL](#aem-image-urls)__ コンテンツフラグメントで参照され、AEMによって配信されるアセットを画像化する

通常、AEMヘッドレスアプリは、GraphQL API とイメージリクエストの両方で単一のAEMサービスとやり取りします。 AEMサービスは、AEMヘッドレスアプリのデプロイメントに応じて変更されます。

| AEMヘッドレスデプロイメントタイプ | AEM環境 | AEM service |
|-------------------------------|:---------------------:|:----------------:|
| 実稼動 | 実稼動 | 公開 |
| オーサリングプレビュー | 実稼動 | プレビュー |
| 開発 | 開発 | 公開 |

デプロイメントタイプの順列を処理するために、各アプリケーションのデプロイメントは、接続先のAEMサービスを指定する設定を使用して構築されます。 次に、設定済みのAEMサービスのホスト/ドメインを使用して、AEM GraphQL API の URL と画像 URL を構築します。 ビルドに依存する設定を管理するための正しい方法を判断するには、AEMヘッドレスアプリのフレームワーク (React、iOS、Android™など ) ドキュメントを参照してください。この方法はフレームワークによって異なります。

| クライアントタイプ | [シングルページアプリ (SPA)](../spa.md) | [Web コンポーネント/JS](../web-component.md) | [モバイル](../mobile.md) | [サーバー間](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| AEMホスト設定 | ✔ | ✔ | ✔ | ✔ |

以下は、 [AEM GraphQL API](#aem-graphql-api-requests) および [イメージリクエスト](#aem-image-requests)（一般的なヘッドレスフレームワークおよびプラットフォーム向け）

## AEM GraphQL API リクエスト

ヘッドレスアプリからAEM GraphQL API への HTTPGETリクエストは、正しいAEMサービスとやり取りするように設定する必要があります。詳しくは、 [上の表](#managing-aem-hosts).

を使用する場合 [AEMヘッドレス SDK](../../how-to/aem-headless-sdk.md) ( ブラウザーベースの JavaScript、サーバーベースの JavaScript および Java™で使用可能 )AEMホストは、接続するAEMサービスを使用してAEMヘッドレスクライアントオブジェクトを初期化できます。

カスタムAEMヘッドレスクライアントを開発する場合は、AEMサービスのホストが、ビルドパラメーターに基づいてパラメーター化可能であることを確認します。

### 例

次に、AEM GraphQL API リクエストでAEMホスト値を様々なヘッドレスアプリフレームワーク用に設定できるようにする方法の例を示します。

+++ React の例

この例では、大まかに [AEMヘッドレス React アプリ](../../example-apps/react-app.md)は、環境変数に基づいて、AEM GraphQL API リクエストを様々なAEM Services に接続するように設定する方法を示しています。

React アプリでは、 [JavaScript 用AEMヘッドレスクライアント](../../how-to/aem-headless-sdk.md) AEM GraphQL API とやり取りする AEM Headless Client for JavaScript が提供するAEMヘッドレスクライアントは、接続先のAEMサービスホストで初期化する必要があります。

#### React 環境ファイル

React はを使用 [カスタム環境ファイル](https://create-react-app.dev/docs/adding-custom-environment-variables/)または `.env` ビルド固有の値を定義するために、プロジェクトのルートに保存されるファイル。 例えば、 `.env.development` ファイルには開発時に使用される値が含まれますが、 `.env.production` には、実稼動ビルドに使用される値が含まれます。

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` 他の用途のファイル [指定可能](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) ポストフィックスで `.env` とセマンティック記述子 ( `.env.stage` または `.env.production`. 異なる `.env` ファイルは、 `REACT_APP_ENV` 実行する前に `npm` コマンドを使用します。

例えば、React アプリの `package.json` 次のものを含むことができます。 `scripts` config:

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### AEMヘッドレスクライアント

この [JavaScript 用AEMヘッドレスクライアント](../../how-to/aem-headless-sdk.md) には、AEM GraphQL API に対して HTTP リクエストをおこなうAEMヘッドレスクライアントが含まれています。 AEMヘッドレスクライアントは、操作するAEMホストで、アクティブなの値を使用して初期化する必要があります `.env` ファイル。

+ `src/api/headlessClient.js`

```
const { AEMHeadless } = require('@adobe/aem-headless-client-js');
...
// Get the environment variables for configuring the headless client, 
// specifically the `REACT_APP_AEM_HOST` which contains the AEM service host.
const {
    REACT_APP_AEM_HOST,         // https://publish-p123-e456.adobeaemcloud.com
    REACT_APP_GRAPHQL_ENDPOINT,
} = process.env;
...

// Initialize the AEM Headless client with the AEM Service host, which dictates the AEM service provides the GraphQL data.
export const aemHeadlessClient = new AEMHeadless({
    serviceURL: REACT_APP_AEM_HOST,        
    endpoint: REACT_APP_GRAPHQL_ENDPOINT
});
```

#### React useEffect(...) フック

カスタム React useEffect フックは、ビューをレンダリングする React コンポーネントの代わりに、AEMホストで初期化されたAEMヘッドレスクライアントを呼び出します。

+ `src/api/persistedQueries.js`

```javascript
import { aemHeadlessClient , mapErrors} from "./headlessClient";
...
// The exported useEffect hook
export const getAdventureByPath = async function(adventurePath) {
    const queryVariables = {'adventurePath': adventurePath};
    return executePersistedQuery('wknd-shared/adventures-by-path', queryVariables);
}
...
// Private function that invokes the aemHeadlessClient
const executePersistedQuery = async function(persistedQueryPath, queryVariables) {
    let data;
    let errors;

    try {
        // Run the persisted query using using the aemHeadlessClient that's initialized with the AEM host
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

#### React コンポーネント

カスタムの useEffect フック `useAdventureByPath` が読み込まれ、AEMヘッドレスクライアントを使用してデータを取得し、最終的にコンテンツをエンドユーザーにレンダリングするために使用されます。

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ iOS™の例

この例では、 [AEMヘッドレスiOS™アプリの例](../../example-apps/ios-swiftui-app.md)を参照し、に基づいて異なるAEMホストに接続するようにAEM GraphQL API リクエストを設定する方法を説明します。 [ビルド固有の設定変数](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

iOS™アプリでAEM GraphQL API とやり取りするには、カスタムAEMヘッドレスクライアントが必要です。 AEMヘッドレスクライアントは、AEMサービスホストを設定できるように記述する必要があります。

#### 構築設定

XCode 設定ファイルには、デフォルトの設定の詳細が含まれています。

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### カスタムAEMヘッドレスクライアントを初期化

この [AEMヘッドレスiOSアプリの例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) は、次の設定値で初期化されたカスタムAEMヘッドレスクライアントを使用します。 `AEM_SCHEME` および `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

カスタムAEMヘッドレスクライアント (`api/Aem.swift`) にメソッドが含まれる場合にのみ考慮されます `makeRequest(..)` は、AEM GraphQL API リクエストに設定済みのAEMを接頭辞として付加します。 `scheme` および `host`.

+ `api/Aem.swift`

```swift
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
    
    return request;
}
```

[新しいビルド設定ファイルを作成できます](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) 別のAEMサービスに接続する場合。 のビルド固有の値 `AEM_SCHEME` および `AEM_HOST` は、XCode で選択されたビルドに基づいて使用され、その結果、カスタムAEMヘッドレスクライアントが正しいAEMサービスに接続するようになります。

+++

+++ Android™の例

この例では、 [AEMヘッドレス Android™アプリの例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)は、ビルド固有の（または各種の）設定変数に基づいて、様々なAEM Services に接続するようにAEM GraphQL API リクエストを設定する方法を示します。

Android™アプリ (Java™で記述される場合 ) は、 [AEM Headless Client for Java™](https://github.com/adobe/aem-headless-client-java) AEM GraphQL API とやり取りする AEM Headless Client for Java™が提供するAEMヘッドレスクライアントは、接続先のAEMサービスホストで初期化する必要があります。

#### 設定ファイルを作成

Android™アプリでは、様々な用途のアーティファクトを構築するために使用される「productFlavors」を定義します。
この例では、2 つの Android™製品のフレーバーを定義し、異なるAEMサービスホスト (`AEM_HOST`) 開発用の値 (`dev`) および実稼動 (`prod`) はを使用します。

アプリの `build.gradle` ファイル、新規 `flavorDimension` 名前付き `env` が作成されました。

内 `env` 寸法、2 `productFlavors` が定義されている： `dev` および `prod`. 各 `productFlavor` uses `buildConfigField` 接続先のAEMサービスを定義するビルド固有の変数を設定する場合。

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Android™ loader

を初期化します。 `AEMHeadlessClient` ビルダー。AEM Headless Client for Java™によって提供され、 `AEM_HOST` 値 `buildConfigField` フィールドに入力します。

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/AdventuresLoader.java`

```java
public class AdventuresLoader extends AsyncTaskLoader<AdventureList> {
    ...

    @Override
    public AdventureList loadInBackground() {
        ...
        // Initialize the AEM Headless client using the AEM Host exposed via BuildConfig.AEM_HOST
        AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(BuildConfig.AEM_HOST);
        AEMHeadlessClient client = builder.build();
        // With the AEM headless client initialized, make GraphQL persisted query calls to AEM
        ...    
    }
    ...
}
```

様々な用途で Android™アプリを作成する場合は、 `env` フレーバーと、対応するAEMホスト値が使用されます。

+++

## AEM画像 URL

ヘッドレスアプリからAEMに対するイメージリクエストは、正しいAEMサービスとやり取りするように設定する必要があります。詳しくは、 [上の表](#managing-aem-hosts).

AEM GraphQL の `... on ImageRef` フィールドを提供 `_authorUrl` および `_publishUrl` 各AEMサービスへの絶対 URL を含む場合、多くの場合、 `_path` AEM GraphQL API のクエリに使用するAEMサービスホストのフィールドとプレフィックスを設定します。

使用 `_path` は、ヘッドレスアプリがデプロイメントのコンテキストに基づいて AEM オーサーまたは AEM パブリッシュに接続できる場合に特に便利です。

ヘッドレスアプリが AEM オーサーまたはパブリッシュと排他的にやり取りする場合、 `_authorUrl` または `_publishUrl` フィールドを使用して実装を簡略化でき、以下の例のガイダンスは無視できます。

### 例

様々なヘッドレスアプリフレームワーク用に設定されたAEMホスト値に、画像 URL でプレフィックスを付ける方法の例を以下に示します。 この例では、 `_path` フィールドに入力します。

次に例を示します。

#### GraphQL 永続クエリ

この GraphQL クエリは、画像参照の `_path`. 例えば、 [GraphQL の応答](#examples-react-graphql-response) ホストを除外する

```graphql
query ($path: String!) {
  adventureByPath(_path: $path) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _path
        }
      }
    }
  }
}
```

#### GraphQL の応答

この GraphQL 応答は、画像参照の `_path` ホストを除外する

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg",
        }
      }
    }
  }
}
```

+++ React の例

この例では、 [AEMヘッドレス React アプリの例](../../example-apps/react-app.md)では、環境変数に基づいて、適切なAEM Services に接続するように画像 URL を設定する方法を説明します。

この例では、画像参照のプレフィックスを示します `_path` フィールド（設定可能） `REACT_APP_AEM_HOST` React 環境変数。

#### React 環境ファイル

React はを使用 [カスタム環境ファイル](https://create-react-app.dev/docs/adding-custom-environment-variables/)または `.env` ビルド固有の値を定義するために、プロジェクトのルートに保存されるファイル。 例えば、 `.env.development` ファイルには開発時に使用される値が含まれますが、 `.env.production` には、実稼動ビルドに使用される値が含まれます。

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` 他の用途のファイル [指定可能](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) ポストフィックスで `.env` とセマンティック記述子 ( `.env.stage` または `.env.production`. 異なる `.env` ファイルは、 `REACT_APP_ENV` 実行する前に `npm` コマンドを使用します。

例えば、React アプリの `package.json` 次のものを含むことができます。 `scripts` config:

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### React コンポーネント

React コンポーネントは、 `REACT_APP_AEM_HOST` 環境変数に追加し、イメージのプレフィックスを付けます。 `_path` の値を指定して、完全に解決可能な画像 URL を指定します。

同じ `REACT_APP_AEM_HOST` 環境変数は、 `useAdventureByPath(..)` AEMから GraphQL データを取得するために使用するカスタム useEffect フック。 同じ変数を使用して、画像 URL として GraphQL API リクエストを作成し、React アプリが両方の使用例で同じAEMサービスとやり取りすることを確認します。

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._path }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ iOS™の例

この例では、 [AEMヘッドレスiOS™アプリの例](../../example-apps/ios-swiftui-app.md)では、に基づいてAEMの画像 URL を様々なAEMホストに接続するように設定する方法を説明します。 [ビルド固有の設定変数](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### 構築設定

XCode 設定ファイルには、デフォルトの設定の詳細が含まれています。

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### 画像 URL ジェネレーター

In `Aem.swift`、カスタムAEMヘッドレスクライアント実装、カスタム関数 `imageUrl(..)` は、 `_path` GraphLQ 応答のフィールドに入力し、AEMホストの前に追加します。 その後、この関数は、イメージがレンダリングされるたびにiOSビューで呼び出されます。

+ `WKNDAdventures/AEM/Aem.swift`

```swift
class Aem: ObservableObject {
    let scheme: String
    let host: String
    ...
    init(scheme: String, host: String) {
        self.scheme = scheme
        self.host = host
    }
    ...
    /// Prefixes AEM image paths wit the AEM scheme/host
    func imageUrl(path: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(path)")!
    }
    ...
}
```

#### iOSビュー

iOSビューでは、画像のプレフィックスが付きます。 `_path` の値を指定して、完全に解決可能な画像 URL を指定します。

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image path to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(path: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[新しいビルド設定ファイルを作成できます](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) 別のAEMサービスに接続する場合。 のビルド固有の値 `AEM_SCHEME` および `AEM_HOST` は、XCode で選択されたビルドに基づいて使用され、その結果、カスタムAEMヘッドレスクライアントが正しいAEMサービスとやり取りすることになります。

+++

+++ Android™の例

この例では、 [AEMヘッドレス Android™アプリの例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)では、ビルド固有の（または各種の）設定変数に基づいて、様々なAEM Services に接続するようにAEM画像 URL を設定する方法を示します。

#### 設定ファイルを作成

Android™アプリでは、様々な用途のアーティファクトを構築するために使用される「productFlavors」を定義します。
この例では、2 つの Android™製品のフレーバーを定義し、異なるAEMサービスホスト (`AEM_HOST`) 開発用の値 (`dev`) および実稼動 (`prod`) はを使用します。

アプリの `build.gradle` ファイル、新規 `flavorDimension` 名前付き `env` が作成されました。

内 `env` 寸法、2 `productFlavors` が定義されている： `dev` および `prod`. 各 `productFlavor` uses `buildConfigField` 接続先のAEMサービスを定義するビルド固有の変数を設定する場合。

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### AEM画像の読み込み

Android™では、 `ImageGetter` を使用して、AEMから画像データを取得し、ローカルにキャッシュします。 In `prepareDrawableFor(..)` アクティブなビルド設定で定義されたAEMサービスホストは、解決可能な URL を作成する画像パスのプレフィックスとしてAEMに使用されます。

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/RemoteImagesCache.java`

```java
...
public class RemoteImagesCache implements Html.ImageGetter {
    ...
    private final Map<String, Drawable> drawablesByPath = new HashMap<>();
    ...
    public void prepareDrawableFor(String path) {
        ...

        // Prefix the image path with the build config AEM_HOST variable
        String urlStr = BuildConfig.AEM_HOST + path;

        URL url = new URL(urlStr);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        // Get the image data from AEM 
        Drawable drawable = Drawable.createFromStream(is, new File(path).getName());
        ...
        // Save the image data into the cache using the path as the key
        drawablesByPath.put(path, drawable);
        ...    
    }

    @Override
    public Drawable getDrawable(String path) {
        // Get the image data from the cache using the path as the key
        Drawable drawable = drawablesByPath.get(path);
        return drawable;
    }
}
```

#### Android™ view

Android™ビューは、 `RemoteImagesCache` の使用 `_path` の値を返します。

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImagePath()));
        ...
    }
...
}
```

様々な用途で Android™アプリを作成する場合は、 `env` フレーバーと、対応するAEMホスト値が使用されます。

+++