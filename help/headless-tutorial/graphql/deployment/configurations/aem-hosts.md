---
title: AEM GraphQL 用 AEM ホストの管理
description: AEM ヘッドレスアプリで AEM ホストを設定する方法について説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
duration: 496
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '1614'
ht-degree: 100%

---

# AEM ホストの管理

AEM ヘッドレスアプリケーションのデプロイでは、AEM の URL の作成方法に注意して、AEM の正しいホストやドメインを使用する必要があります。認識する必要のあるプライマリ URL やリクエストタイプは次のとおりです。

+ __[AEM GraphQL API](#aem-graphql-api-requests)__ への HTTP リクエスト
+ コンテンツフラグメントで参照され、AEM によって配信されるアセットを画像化する&#x200B;__[画像 URL](#aem-image-urls)__

通常、AEM ヘッドレスアプリは、GraphQL API と画像リクエストの両方で単一の AEM サービスとやり取りします。AEM サービスは、AEM ヘッドレスアプリのデプロイメントに応じて変更されます。

| AEM ヘッドレスデプロイメントのタイプ | AEM 環境 | AEM サービス |
|-------------------------------|:---------------------:|:----------------:|
| 実稼動 | 実稼動 | パブリッシュ |
| オーサリングプレビュー | 実稼動 | プレビュー |
| 開発 | 開発 | パブリッシュ |

デプロイメントタイプの順列を処理するために、各アプリのデプロイメントは、接続先の AEM サービスを指定する設定を使用して作成されます。次に、設定済みの AEM サービスのホストやドメインを使用して、AEM GraphQL API の URL と画像 URL を作成します。ビルドに依存する設定を管理するための正しい方法を判断するには、AEM ヘッドレスアプリのフレームワーク（React、iOS、Android™ など）のドキュメントを参照してください。その方法はフレームワークによって異なります。

| クライアントタイプ | [シングルページアプリ（SPA）](../spa.md) | [Web コンポーネント／JS](../web-component.md) | [モバイル](../mobile.md) | [サーバー間](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| AEM ホスト設定 | ✔ | ✔ | ✔ | ✔ |

以下は、一般的なヘッドレスフレームワークとプラットフォーム用に [AEM GraphQL API](#aem-graphql-api-requests) と[画像リクエスト](#aem-image-requests)の URL を作成するアプローチの例です。

## AEM GraphQL API リクエスト

ヘッドレスアプリから AEM GraphQL API への HTTP GET リクエストは、正しい AEM サービスとやり取りするように設定する必要があります。詳しくは、[上の表](#managing-aem-hosts)を参照してください。

[AEM ヘッドレス SDK](../../how-to/aem-headless-sdk.md)（ブラウザーベースの JavaScript、サーバーベースの JavaScript、および Java™ で利用可能）を使用する場合、AEM ホストは接続する AEM サービスで AEM ヘッドレスクライアントオブジェクトを初期化できます。

カスタム AEM ヘッドレスクライアントを開発する場合は、AEM サービスのホストが、ビルドパラメーターに基づいてパラメーター化可能であることを確認します。

### 例

次に、AEM GraphQL API リクエストで、様々なヘッドレスアプリフレームワーク用に AEM ホスト値を設定可能にする方法の例を示します。

+++ React の例

この例は、[AEM ヘッドレス React アプリ](../../example-apps/react-app.md)に大まかに基づいており、環境変数に基づいて様々な AEM サービスに接続するように AEM GraphQL API リクエストを作成する方法を示しています。

React アプリは [JavaScript 用 AEM ヘッドレスクライアント](../../how-to/aem-headless-sdk.md)を使用して、AEM GraphQL API とやり取りする必要があります。JavaScript 用 AEM ヘッドレスクライアントが提供する AEM ヘッドレスクライアントは、接続先の AEM サービスホストで初期化する必要があります。

#### React 環境ファイル

React では、プロジェクトのルートに保存されている[カスタム環境ファイル](https://create-react-app.dev/docs/adding-custom-environment-variables/)または `.env` ファイルを使用して、ビルド固有の値を定義します。例えば、`.env.development` ファイルには、開発時に使用される値が含まれているのに対して、`.env.production` には、実稼動ビルドに使用される値が含まれています。

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

他の用途の `.env` ファイルは、`.env.stage` や `.env.production` のように、`.env` とセマンティック記述子を末尾に付加して[指定できます](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used)。`npm` コマンドを実行する前に `REACT_APP_ENV` を設定することで、React アプリの実行またはビルド時に異なる `.env` ファイルを使用できます。

例えば、React アプリの `package.json` には、次の `scripts` 設定が含まれている場合があります。

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

#### AEM ヘッドレスクライアント

[JavaScript 用 AEM ヘッドレスクライアント](../../how-to/aem-headless-sdk.md)には、AEM GraphQL API に対して HTTP リクエストを行う AEM ヘッドレスクライアントが含まれています。AEM ヘッドレスクライアントは、アクティブな `.env` ファイルの値を使用して、クライアントがやり取りする AEM ホストで初期化する必要があります。

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

カスタム React useEffect フックは、ビューをレンダリングする React コンポーネントの代わりに、AEM ホストで初期化された AEM ヘッドレスクライアントを呼び出します。

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

カスタム useEffect フック `useAdventureByPath` が読み込まれ、AEM ッドレスクライアントを使用してデータを取得し、最終的にコンテンツをエンドユーザーにレンダリングするために使用されます。

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ iOS™ の例

この例は、[AEM ヘッドレス iOS™ アプリの例](../../example-apps/ios-swiftui-app.md)に基づいており、[ビルド固有の設定変数](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3)に基づいて様々な AEM ホストに接続するように AEM GraphQL API リクエストを設定する方法を示しています。 

iOS™ アプリでは、カスタム AEM ヘッドレスクライアントが AEM の GraphQL API とやり取りする必要があります。 AEM ヘッドレスクライアントは、AEM サービスホストが設定可能なように記述する必要があります。

#### ビルド設定

XCode 設定ファイルには、デフォルトの設定詳細が含まれています。

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### カスタム AEM ヘッドレスクライアントの初期化

[AEM ヘッドレス iOS アプリの例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)では、`AEM_SCHEME` および `AEM_HOST` の設定値で初期化されたカスタム AEM ヘッドレスクライアントを使用しています。 

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

カスタム AEM ヘッドレスクライアント（`api/Aem.swift`）には、AEM GraphQL API リクエストの前に設定済みの AEM `scheme` および `host` を付けるメソッド `makeRequest(..)` が含まれています。

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

別の AEM サービスに接続する[新しいビルド設定ファイルを作成できます](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3)。`AEM_SCHEME` および `AEM_HOST` のビルド固有の値は、選択したビルドに基づいて XCode で使用され、その結果、カスタム AEM ヘッドレスクライアントが正しい AEM サービスに接続するようになります。

+++

+++ Android™ の例

この例は、[AEM ヘッドレス Android™ アプリの例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)に基づいており、ビルド固有の（または各種の）設定変数に基づく様々な AEM サービスに接続するように AEM GraphQL API リクエストを設定する方法を示しています。

Android™ アプリ（Java™ で記述する場合）は、[Java™ 用 AEM ヘッドレスクライアント](https://github.com/adobe/aem-headless-client-java)を使用して、AEM の GraphQL API とやり取りする必要があります。Java™ 用 AEM ヘッドレスクライアントから提供される AEM ヘッドレスクライアントは、接続先の AEM サービスホストで初期化する必要があります。

#### ビルド設定ファイル

Android™ アプリでは、様々な用途のアーティファクトを作成するために使用される「productFlavors」を定義します。
この例では、2 つの Android™ 製品フレーバーを定義して、開発用（`dev`）と実稼動用（`prod`）とで異なる AEM サービスホスト（`AEM_HOST`）の値を指定する方法を示しています。

アプリの `build.gradle` ファイルに、`env` という名前の新しい `flavorDimension` が作成されます。

`env` ディメンションでは、`dev` と `prod` という 2 つの `productFlavors` が定義されています。それぞれの `productFlavor` は、`buildConfigField` を使用して、接続先の AEM サービスを定義するビルド固有の変数を設定します。

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

#### Android™ ローダー

Java™ 用 AEM ヘッドレスクライアントから提供される `AEMHeadlessClient` ビルダーを、`buildConfigField` フィールドの `AEM_HOST` 値で初期化します。

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

様々な用途で Android™ アプリを作成する場合は、`env` フレーバーを指定すると、対応する AEM ホスト値が使用されます。

+++

## AEM 画像 URL

ヘッドレスアプリから AEM に対する画像リクエストは、[前述の表](#managing-aem-hosts)で説明したように、正しい AEM サービスとやり取りするように設定する必要があります。

アドビでは、AEM の GraphQL API の `_dynamicUrl` フィールドを通じて入手できる[最適化された画像](../../how-to/images.md)を使用することをお勧めします。`_dynamicUrl` フィールドは、AEM GraphQL API に対するクエリに使用される AEM サービスホストをプレフィックスにできるホストレス URL を返します。GraphQL 応答の `_dynamicUrl` フィールドは次のようになります。

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### 例

様々なヘッドレスアプリフレームワークに設定可能になっている AEM ホスト値に、画像 URL でプレフィックスを付ける方法の例を以下に示します。この例では、`_dynamicUrl` フィールドを使用して画像参照を返す GraphQL クエリを使用することを想定しています。

次に例を示します。

#### GraphQL 永続クエリ

この GraphQL クエリは、画像参照の `_dynamicUrl` を返します。[GraphQL 応答](#examples-react-graphql-response)の節で示したとおり、ホストが除かれています。

```graphql
query ($path: String!) {
  adventureByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true }) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

#### GraphQL 応答

この GraphQL 応答は画像参照の `_dynamicUrl` を返しますが、そこではホストが除かれています 。

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg?preferwebp=true",
        }
      }
    }
  }
}
```

+++ React の例

この例では、[AEM ヘッドレス React アプリの例](../../example-apps/react-app.md)に基づいて、環境変数に基づく適切な AEM サービスに接続するように画像 URL を設定する方法を説明します。

この例では、画像参照の `_dynamicUrl` フィールドのプレフィックスを設定可能な `REACT_APP_AEM_HOST` React 環境変数に設定する方法を示しています。

#### React 環境ファイル

React では、プロジェクトのルートに保存されている[カスタム環境ファイル](https://create-react-app.dev/docs/adding-custom-environment-variables/)または `.env` ファイルを使用して、ビルド固有の値を定義します。例えば、`.env.development` ファイルには、開発時に使用される値が含まれているのに対して、`.env.production` には、実稼動ビルドに使用される値が含まれています。

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

他の用途の `.env` ファイルは、`.env.stage` または `.env.production` のように、`.env` とセマンティック記述子をポストフィックスにして[指定することができます](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used)。`npm` コマンドを実行する前に `REACT_APP_ENV` を設定することで、React アプリの実行時やビルド時に異なる `.env`フ ァイルを利用できます。

例えば、React アプリの `package.json` は次の `scripts` 設定を含むことができます。

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

React コンポーネントは、`REACT_APP_AEM_HOST` 環境変数を読み込みし、画像 `_dynamicUrl` の値をプレフィックスして、完全に解決可能な画像 URL を提供します。

この同じ `REACT_APP_AEM_HOST` 環境変数は、AEM から GraphQL データを取得するために使用する `useAdventureByPath(..)` カスタム useEffect フックで使用される、AEM ヘッドレスのクライアントを初期化するのに使用されます。同じ変数を使用して GraphQL API リクエストを画像 URL として作成し、React アプリが両方のユースケースで同じ AEM サービスとやり取りできるようにします。

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ iOS™ の例

ここでは、[ AEM ヘッドレス iOS™ アプリ ](../../example-apps/ios-swiftui-app.md) の例に基づいて、AEM の画像 URL を[ビルド固有の設定変数](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3)に基づき様々な AEM ホストに接続するように設定する方法の例を示します。

#### ビルド設定

XCode 設定ファイルには、デフォルトの設定詳細が含まれています。

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### 画像 URL ジェネレーター

カスタムの AEM ヘッドレスクライアント実装である `Aem.swift` では、カスタム関数 `imageUrl(..)` は、GraphQL 応答の `_dynamicUrl` フィールドで提供される画像パスを受け取り、その前に AEM のホストを付加します。その後、この関数は、画像がレンダリングされるたびに iOS ビューで呼び出されます。

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
    /// Prefixes AEM image dynamicUrl with the AEM scheme/host
    func imageUrl(dynamicUrl: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(dynamicUrl)")!
    }
    ...
}
```

#### iOS ビュー

iOS ビューは、完全に解決可能な画像 URL を提供するために、画像 `_dynamicUrl` の値をプレフィックスします。

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image dynamicUrl to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(dynamicUrl: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[新しいビルド設定ファイルを作成](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3)して、様々な AEM サービスに接続できます。`AEM_SCHEME` および `AEM_HOST` のビルド固有の値は、XCode で選択されたビルドに基づいて使用されるため、カスタム AEM ヘッドレスクライアントは正しい AEM サービスと対話します。

+++

+++ Android™ の例

この例は、[AEM ヘッドレス Android™ アプリの例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)に基づいており、ビルド固有の（またはフレーバーの）設定変数に基づいて、様々な AEM サービスに接続するように AEM イメージ URL を設定する方法を示しています。

#### ビルド設定ファイル

Android™ アプリでは、様々な用途のアーティファクトを構築するために使用される「productFlavors」を定義します。
この例では、2 つの Android™ 製品フレーバーを定義して、開発（`dev`）と実稼働（`prod`）で使用するために異なる AEM サービス ホスト（`AEM_HOST`）値を提供する方法を示しています。

アプリの `build.gradle` ファイルに、`env` という名前の新しい `flavorDimension` が作成されます。

`env` ディメンションでは、`dev` と `prod` の 2 つの `productFlavors` が定義されています。各 `productFlavor` は `buildConfigField` を使用して、接続先の AEM サービスを定義するビルド固有の変数を設定します。

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

#### AEM 画像の読み込み

Android™ では、`ImageGetter` を使用して、AEM から画像データを取得し、ローカルにキャッシュします。 `prepareDrawableFor(..)` では、アクティブなビルド設定で定義された AEM サービスホストを使用して、画像パスのプレフィックスとして AEM への解決可能な URL を作成します。

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
    public Drawable getDrawable(String dynamicUrl) {
        // Get the image data from the cache using the dynamicUrl as the key
        return drawablesByPath.get(dynamicUrl);
    }
}
```

#### Android™ ビュー

Android™ビューは、GraphQL レスポンスの `_dynamicUrl` 値を使用して `RemoteImagesCache` 経由で画像データを取得します。

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImageDynamicUrl()));
        ...
    }
...
}
```

様々な用途で Android™ アプリをビルドする場合は、`env` フレーバーを指定すると、対応する AEM ホスト値が使用されます。

+++
