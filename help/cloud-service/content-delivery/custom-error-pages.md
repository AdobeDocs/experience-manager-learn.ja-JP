---
title: カスタムエラーページ
description: AEM as a Cloud Serviceがホストする web サイトにカスタムエラーページを実装する方法について説明します。
version: Cloud Service
feature: Brand Experiences, Configuring, Developing
topic: Content Management, Development
role: Developer
level: Beginner, Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-09-24T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
source-git-commit: d11b07441d8c46ce9a352e4c623ddc1781b9b9be
workflow-type: tm+mt
source-wordcount: '1355'
ht-degree: 1%

---


# カスタムエラーページ

AEM as a Cloud Serviceがホストする web サイトにカスタムエラーページを実装する方法について説明します。

このチュートリアルでは、次の内容について学習します。

- デフォルトのエラーページ
- エラーページの提供元
   - AEM サービスのタイプ – オーサー、パブリッシュ、プレビュー
   - Adobeが管理する CDN
- エラーページをカスタマイズするためのオプション
   - ErrorDocument Apache ディレクティブ
   - ACS AEM Commons - エラーページハンドラー
   - CDN エラーページ

## デフォルトのエラーページ

エラーページを表示するタイミング、デフォルトのエラーページ、およびエラーページの提供元を見てみましょう。

エラーページは、次の場合に表示されます。

- ページが存在しません（404）
- ページへのアクセスが許可されていません（403）
- コードの問題またはサーバーに到達できないため、サーバーエラー（500）が発生しました。

AEM as a Cloud Serviceでは、上記のシナリオ向けに _デフォルトのエラーページ_ を提供しています。 これは汎用のページで、ブランドと一致しません。

デフォルトのエラーページ _提供されます_ は、_AEM サービスの種類_ オーサー、パブリッシュ、プレビュー）または _Adobeが管理する CDN_ から提供されます。 詳しくは、次の表を参照してください。

| エラーページの提供元 | 詳細 |
|---------------------|:-----------------------:|
| AEM サービスのタイプ – オーサー、パブリッシュ、プレビュー | ページリクエストがAEM サービスタイプによって提供される場合、エラーページはAEM サービスタイプから提供されます。 |
| Adobeが管理する CDN | Adobeが管理する CDN _AEM サービスの種類に到達できません_ （オリジンサーバー）の場合、エラーページはAdobeが管理する CDN から提供されます。 **ありそうもない出来事だが、言及する価値がある。** |

ただし、AEM サービスのタイプとAdobe管理の両方を _カスタマイズ_ CDN エラーページをブランドに合わせてカスタマイズし、より良いユーザーエクスペリエンスを提供することができます。

## エラーページをカスタマイズするためのオプション

エラーページをカスタマイズするには、次のオプションを使用できます。

| 適用先 | オプション名 | 説明 |
|---------------------|:-----------------------:|:-----------------------:|
| AEM サービスタイプ – 公開とプレビュー | ErrorDocument ディレクティブ | Apache 設定ファイルの [ErrorDocument](https://httpd.apache.org/docs/2.4/custom-error.html) ディレクティブを使用して、カスタムエラーページのパスを指定します。 AEM サービスタイプ（パブリッシュとプレビュー）にのみ適用されます。 |
| AEM サービスタイプ – オーサー、パブリッシュ、プレビュー | ACS AEM Commons エラーページハンドラー | [ACS AEM Commons Error Page Handler](https://adobe-consulting-services.github.io/acs-aem-commons/features/error-handler/index.html) を使用すると、すべてのAEM サービスタイプでエラーをカスタマイズできます。 |
| Adobeが管理する CDN | CDN エラーページ | Adobeが管理する CDN がAEM サービスの種類（オリジンサーバー）に到達できない場合は、CDN エラーページを使用してエラーページをカスタマイズできます。 |


## 前提条件

このチュートリアルでは、_ErrorDocument_ ディレクティブ、_ACS AEM Commons Error Page Handler_ および _CDN Error Pages_ オプションを使用してエラーページをカスタマイズする方法を説明します。 このチュートリアルに従うには、次が必要です。

- [ ローカル AEM開発環境 ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview)AEM as a Cloud Service環境 「_CDN エラーページ_」オプションは、AEM as a Cloud Service環境に適用できます。

- エラーページをカスタマイズする ](https://github.com/adobe/aem-guides-wknd)0}AEM WKND プロジェクト }。[

## 設定

- 次の手順に従って、AEM WKND プロジェクトのクローンを作成し、ローカル AEM開発環境にデプロイします。

  ```
  # For local AEM development environment
  $ git clone git@github.com:adobe/aem-guides-wknd.git
  $ cd aem-guides-wknd
  $ mvn clean install -PautoInstallSinglePackage -PautoInstallSinglePackagePublish
  ```

- AEM as a Cloud Service環境の場合は、[ フルスタックパイプライン ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline) を実行してAEM WKND プロジェクトをデプロイします。[ 実稼動以外のパイプライン ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/cloud-manager/cicd-non-production-pipeline) の例を参照してください。

- WKND サイトページが正しくレンダリングされていることを確認します。

## エラーページをカスタマイズするための ErrorDocument Apache ディレクティブ{#errordocument-directive}

[AEM WKND](https://github.com/adobe/aem-guides-wknd) プロジェクトで `ErrorDocument` Apache ディレクティブを使用してカスタムエラーページを表示する方法を見てみましょう。

- `ui.content.sample` モジュールには、ブランド [ エラーページ ](https://github.com/adobe/aem-guides-wknd/tree/main/ui.content.sample/src/main/content/jcr_root/content/wknd/language-masters/en/errors) @ `/content/wknd/language-masters/en/errors` が含まれています。 [ ローカル AEM](http://localhost:4502/sites.html/content/wknd/language-masters/en/errors) またはAEM as a Cloud Service `https://author-p<ID>-e<ID>.adobeaemcloud.com/ui#/aem/sites.html/content/wknd/language-masters/en/errors` 環境でレビューします。

- `dispatcher` モジュールの `wknd.vhost` ファイルには、次が含まれます。
   - 上記の [ エラーページ ](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L139-L143) を指す [ErrorDocument ディレクティブ ](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/variables/custom.vars#L7-L8)。
   - [DispatcherPassError](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L133) 値は 1 に設定されているので、Dispatcherは Apache がすべてのエラーを処理できます。

  ```
  ...
  # ErrorDocument directive in wknd.vhost file
  ErrorDocument 404 ${404_PAGE}
  ErrorDocument 500 ${500_PAGE}
  ErrorDocument 502 ${500_PAGE}
  ErrorDocument 503 ${500_PAGE}
  ErrorDocument 504 ${500_PAGE}
  
  ...
  # DispatcherPassError value in wknd.vhost file
  <IfModule disp_apache2.c>
      ...
      DispatcherPassError        1
  </IfModule>
  
  # Custom error pages path in custom.vars file
  Define 404_PAGE /content/wknd/us/en/errors/404.html
  Define 500_PAGE /content/wknd/us/en/errors/500.html
  ...
  ```

- お使いの環境で間違ったページ名またはパス（例：[https://publish-p105881-e991000.adobeaemcloud.com/us/en/foo/bar.html](https://publish-p105881-e991000.adobeaemcloud.com/us/en/foo/bar.html) を入力して、WKND サイトのカスタムエラーページを確認します。

## ACS AEM Commons-Error Page Handler：エラーページをカスタマイズします。{#acs-aem-commons-error-page-handler}

ACS AEM Commons Error Page Handler を使用してエラーページをカスタマイズするには、「[ 使用方法 ](https://adobe-consulting-services.github.io/acs-aem-commons/features/error-handler/index.html#how-to-use) の節を参照してください。

## エラーページをカスタマイズするための CDN エラーページ{#cdn-error-pages}

Adobeが管理する CDN がAEM サービスの種類（オリジンサーバー）に到達できない場合に、CDN エラーページを実装してエラーページをカスタマイズします。

>[!IMPORTANT]
>
> Adobeが管理する CDN がAEM サービスタイプ（オリジンサーバー）に到達できない可能性は低いですが、計画する価値があります。


### CDN エラーページの概要

CDN エラーページは、Adobeが管理する CDN によって、シングルページアプリケーション（SPA）として実装されます。

WKND 固有のブランドコンテンツは、JavaScript ファイルを使用して動的に生成する必要があります。 JavaScript ファイルは、公開してアクセスできる場所でホストされている必要があります。 したがって、次の静的ファイルを開発し、公開アクセス可能な場所にホストする必要があります。

1. **jsUrl**:HTML要素を動的に作成してエラーページコンテンツをレンダリングするJavaScript ファイルの絶対 URL です。
1. **cssUrl**：エラーページのコンテンツのスタイルを設定する CSS ファイルの絶対 URL です。
1. **icoUrl**:favicon の絶対 URL。

### カスタムエラーページの作成

WKND 固有のブランドエラーページコンテンツをシングルページアプリケーション（SPA）として開発しましょう。

デモのために、[React](https://react.dev/) を使用しますが、任意のJavaScript フレームワークまたはライブラリを使用できます。

- 次のコマンドを実行して、新しい React プロジェクトを作成します。

  ```
  $ npx create-react-app aem-cdn-error-page
  ```

- お気に入りのコードエディターでプロジェクトを開き、以下のファイルを更新します。

   - `src/App.js`: エラーページのコンテンツをレンダリングするメインコンポーネントです。

     ```javascript
     import logo from "./wknd-logo.png";
     import "./App.css";
     
     function App() {
       return (
         <>
           <div className="App">
             <div className="container">
             <img src={logo} className="App-logo" alt="logo" />
             </div>
           </div>
           <div className="container">
             <div className="error-code">CDN Error Page</div>
             <h1 className="error-message">Ruh-Roh! Page Not Found</h1>
             <p className="error-description">
               We're sorry, we are unable to fetch this page!
             </p>
           </div>
         </>
       );
     }
     
     export default App;
     ```

   - `src/App.css`: エラーページのコンテンツのスタイルを設定します。

     ```css
     .App {
       text-align: left;
     }
     
     .App-logo {
       height: 14vmin;
       pointer-events: none;
     }
     
     
     body {
       margin-top: 0;
       padding: 0;
       font-family: Arial, sans-serif;
       background-color: #fff;
       color: #333;
       display: flex;
       justify-content: center;
       align-items: center;
     }
     
     .container {
       text-align: letf;
       padding-top: 10px;
     }
     
     .error-code {
       font-size: 4rem;
       font-weight: bold;
       color: #ff6b6b;
     }
     
     .error-message {
       font-size: 2.5rem;
       margin-bottom: 10px;
     }
     
     .error-description {
       font-size: 1rem;
       margin-bottom: 20px;
     }
     ```

   - `wknd-logo.png` ファイルを `src` フォルダーに追加します。 [ ファイル ](https://github.com/adobe/aem-guides-wknd/blob/main/ui.frontend/src/main/webpack/resources/images/favicons/favicon-512.png) を `wknd-logo.png` のようにコピーします。

   - `favicon.ico` ファイルを `public` フォルダーに追加します。 [ ファイル ](https://github.com/adobe/aem-guides-wknd/blob/main/ui.frontend/src/main/webpack/resources/images/favicons/favicon-32.png) を `favicon.ico` のようにコピーします。

   - プロジェクトを実行して、WKND ブランド CDN エラーページの内容を確認します。

     ```
     $ npm start
     ```

     ブラウザーを開き、`http://localhost:3000/` に移動して CDN エラーページのコンテンツを確認します。

   - 静的ファイルを生成するプロジェクトを構築します。

     ```
     $ npm run build
     ```

     静的ファイルは、`build` フォルダーに生成されます。


または、上記の React プロジェクトファイルを含んだ [aem-cdn-error-page.zip](./assets/aem-cdn-error-page.zip) ファイルをダウンロードすることもできます。

次に、上記の静的ファイルを公開アクセス可能な場所にホストします。

### CDN エラーページに必要なホストの静的ファイル

Azure Blob Storage で静的ファイルをホストします。 ただし、[Netlify](https://www.netlify.com/)、{Vercel](https://vercel.com/)、[AWS S3[ などの任意の静的ファイルホスティングサービスを使用 ](https://aws.amazon.com/s3/) きます。

- 公式の [Azure Blob Storage](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal) ドキュメントに従って、コンテナを作成し、静的ファイルをアップロードします。

  >[!IMPORTANT]
  >
  >サービスをホストする他の静的ファイルを使用している場合は、静的ファイルをホストするためのドキュメントに従ってください。

- 静的ファイルが公開でアクセスできることを確認します。 WKND デモ固有のストレージアカウント設定は、次のとおりです。

   - **ストレージ アカウント名**: `aemcdnerrorpageresources`
   - **コンテナ名**: `static-files`

  ![Azure Blob Storage](./assets/azure-blob-storage-settings.png)

- 上記のコンテナ `static-files` は、以下のファイル（`build` フォルダーから）がアップロードされます。

   - `error.js`: `build/static/js/main.<hash>.js` ファイルの名前が `error.js` に変更され [ パブリックにアクセス可能 ](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.js) になりました。
   - `error.css`: `build/static/css/main.<hash>.css` ファイルの名前が `error.css` に変更され [ パブリックにアクセス可能 ](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.css) になりました。
   - `favicon.ico`:`build/favicon.ico` ファイルは、そのままアップロードされ、[ 公開アクセス可能 ](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/favicon.ico) になります。

次に、CDN ルール（errorPages）を設定し、上記の静的ファイルを参照します。

### CDN ルールの設定

上記の静的ファイルを使用して CDN エラーページのコンテンツをレンダリングする `errorPages` CDN ルールを設定します。

1. AEM プロジェクトのメイン `config` フォルダーから `cdn.yaml` ファイルを開きます。 例えば、[WKND プロジェクトの cdn.yaml](https://github.com/adobe/aem-guides-wknd/blob/main/config/cdn.yaml) ファイルです。

1. `cdn.yaml` ファイルに次の CDN ルールを追加します。

   ```yaml
   kind: "CDN"
   version: "1"
   metadata:
     envTypes: ["dev", "stage", "prod"]
   data:
     # The CDN Error Page configuration. 
     # The error page is displayed when the Adobe-managed CDN is unable to reach the origin server.
     # It is implemented as a Single Page Application (SPA) and WKND branded content must be generated dynamically using the JavaScript file 
     errorPages:
       spa:
         title: Adobe AEM CDN Error Page # The title of the error page
         icoUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/favicon.ico # The PUBLIC URL of the favicon
         cssUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.css # The PUBLIC URL of the CSS file
         jsUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.js # The PUBLIC URL of the JavaScript file
   ```

1. 変更内容を保存、コミットし、Adobeのアップストリームリポジトリーにプッシュします。

### CDN ルールのデプロイ

最後に、Cloud Manager パイプラインを使用して、設定済みの CDN ルールをAEM as a Cloud Service環境にデプロイします。

1. Cloud Managerで、「**パイプライン** セクションに移動します。

1. 新しいパイプラインを作成するか、**Config** ファイルのみをデプロイする既存のパイプラインを選択します。 詳しい手順については、[ 設定パイプラインの作成 ](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager) を参照してください。

1. 「**実行**」ボタンをクリックして、CDN ルールをデプロイします。

![CDN ルールのデプロイ ](./assets/deploy-cdn-rule-for-cdn-error-pages.png)

### CDN エラーページのテスト

CDN エラーページをテストするには、次の手順に従います。

- ブラウザーを開き、Publish環境の URL に移動するか、`cdnstatus?code=404` を URL に追加します（例：[https://publish-p105881-e991000.adobeaemcloud.com/cdnstatus?code=404](https://publish-p105881-e991000.adobeaemcloud.com/cdnstatus?code=404)。または、[ カスタムドメイン URL](https://wknd.enablementadobe.com/cdnstatus?code=404) を使用してアクセスします

  ![WKND - CDN エラーページ ](./assets/wknd-cdn-error-page.png)

- サポートされているコードは、403、404、406、500 および 503 です。

- ブラウザーの「ネットワーク」タブを確認し、静的ファイルが Azure Blob ストレージから読み込まれていることを確認します。 Adobeが管理する CDN が提供するHTMLドキュメントには最小限のコンテンツが含まれており、JavaScript ファイルはブランド化されたエラーページコンテンツを動的に作成します。

  ![CDN エラーページの「ネットワーク」タブ ](./assets/wknd-cdn-error-page-network-tab.png)

## 概要

このチュートリアルでは、AEM as a Cloud Serviceがホストする web サイトにカスタムエラーページを実装する方法を学びました。

また、Adobeが管理する CDN がAEM サービスの種類（オリジンサーバー）に到達できない場合にエラーページをカスタマイズするための CDN エラーページオプションの詳細な手順も説明しました。



