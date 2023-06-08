---
title: AEM UI 拡張機能の確認
description: 実稼動環境にデプロイする前にAEM UI 拡張機能をプレビュー、テスト、検証する方法について説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603, KT-13382
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '719'
ht-degree: 20%

---

# 拡張機能の検証

AEM UI 拡張機能は、拡張機能が属するAdobe組織内の任意のAEMas a Cloud Service環境に対して検証できます。

拡張機能のテストは、そのリクエストに対してのみ、拡張機能の読み込みをAEMに指示する、特別に作成された URL を使用しておこなわれます。

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

>[!IMPORTANT]
>
> 上記のビデオでは、コンテンツフラグメントコンソール拡張機能を使用して App Builder 拡張機能のアプリのプレビューと検証を示しています。 ただし、扱う概念はすべてのAEM UI 拡張機能に適用できることに注意してください。

## AEM UI URL

![AEM コンテンツフラグメントコンソールの URL](./assets/verify/content-fragment-console-url.png){align="center"}

実稼動以外の拡張機能をAEMにマウントする URL を作成するには、拡張機能の挿入先となるAEM UI の URL を取得する必要があります。 AEMas a Cloud Service環境に移動して拡張機能を検証し、拡張機能のプレビュー先の UI を開きます。

例えば、コンテンツフラグメントコンソールの拡張機能をプレビューするには、次のようにします。

1. 目的の AEM as a Cloud Service 環境にログインします。
2. 「__コンテンツフラグメント__」アイコンを選択します。
3. AEM コンテンツフラグメントコンソールがブラウザーに読み込まれるまで待ちます。
4. AEM コンテンツフラグメントコンソールの URL をブラウザーのアドレスバーからコピーします。URL は以下のようになります。

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

この URL は、開発およびステージ検証用に URL を作成する際に以下で使用されます。 他のAEM UI に対する拡張機能を検証する場合は、これらの URL を取得し、以下と同じ手順を適用します。

## ローカル開発ビルドの検証

1. 拡張機能プロジェクトのルートへのコマンドラインを開きます。
1. ローカルの App Builder アプリとしてAEM UI 拡張機能を実行する

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

上記の `-> https://localhost:9080` として示されているローカルアプリケーションの URL をメモします

1. 次の 2 つのクエリパラメーターを [AEM UI の URL](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`、通常は `&ext=https://localhost:9080` です。

   上記の 2 つのクエリパラメーター (`devMode` および `ext`) を __first__ URL のクエリーパラメーター。 AEM extensible UI はハッシュルート (`#/@wknd/aem/...`) の代わりに、 `#` は機能しません。

   プレビュー URL は次のようになります。

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

2. プレビュー URL をコピーして、ブラウザーに貼り付けます。

   + 最初に、定期的に、 [HTTPS 証明書を受け入れる](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) ローカルアプリケーションのホスト (`https://localhost:9080`) をクリックします。

3. AEM UI は、検証用に拡張機能のローカルバージョンが挿入されて読み込まれます。

>[!IMPORTANT]
>
>この方法を使用する場合、開発中の拡張機能はエクスペリエンスにのみ影響し、AEM UI の他のすべてのユーザーは、挿入された拡張機能を使用せずに UI をエクスペリエンスに反映します。

## ステージビルドの検証

1. 拡張機能プロジェクトのルートへのコマンドラインを開きます。
1. ステージワークスペースがアクティブ（または検証に使用されるワークスペース）であることを確認します。

   ```shell
   $ aio app use -w Stage
   ```

   変更を `.env` と `.aio` に結合します。

1. 更新された拡張機能の App Builder アプリをデプロイします。ログインしていない場合は、最初に `aio login` を実行します。

   ```shell
   $ aio app deploy
   ...
   Your deployed actions:
   web actions:
     -> https://98765-123aquarat.adobeio-static.net/api/v1/web/aem-cf-console-admin-1/generic 
   To view your deployed application:
     -> https://98765-123aquarat.adobeio-static.net/index.html
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://98765-123aquarat.adobeio-static.net/index.html
   New Extension Point(s) in Workspace 'Production': 'aem/cf-console-admin/1'
   Successful deployment 🏄
   ```

1. 次の 2 つのクエリパラメーターを [AEM UI の URL](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   上記の 2 つのクエリパラメーター (`devMode` および `ext`) を __first__ 拡張可能なAEM UI ではハッシュルート (`#/@wknd/aem/...`) の代わりに、 `#` は機能しません。

   プレビュー URL は次のようになります。

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. プレビュー URL をコピーして、ブラウザーに貼り付けます。
1. AEM コンテンツフラグメントコンソールは、ステージワークスペースにデプロイされた拡張機能のバージョンを挿入します。このステージ URL は、検証のために QA またはビジネスユーザーに共有できます。

この方法を使用する場合、ステージングされた拡張機能は、作成されたステージ URL を使用してアクセスする際に AEM コンテンツフラグメントコンソールにのみ挿入されます。

1. デプロイされた拡張機能は、次を実行して更新できます： `aio app deploy` プレビュー URL を使用すると、これらの変更が自動的に反映されます。
1. 検証用に拡張機能を削除するには、を実行します。 `aio app undeploy`.

## ブックマークレットのプレビュー

上記のプレビュー URL とプレビュー URL の作成を容易にするために、拡張機能を読み込む JavaScript ブックマークレットを作成できます。

以下のブックマークレットでは、 [ローカル開発ビルド](#verify-local-development-builds) ～の延長の `https://localhost:9080`. プレビューするには [ステージビルド](#verify-stage-builds)、 `previewApp` 変数をデプロイ済みの App Builder アプリの URL に設定します。

1. ブラウザーにブックマークを作成します。
2. ブックマークを編集します。
3. ブックマークに次のような意味のある名前を付ける `AEM UI Extension Preview (localhost:9080)`.
4. ブックマークの URL を次のコードに設定します。

   ```javascript
   javascript: (() => {
       /* Change this to the URL of the local App Builder app if not using https://localhost:9080 */
       const previewApp = 'https://localhost:9080';
   
       const repo = new URL(window.location.href).searchParams.get('repo');
   
       if (window.location.href.match(/https:\/\/experience\.adobe\.com\/.*\/aem\/cf\/(editor|admin)\/.*/i)) {
           window.location = `https://experience.adobe.com/?devMode=true&ext=${previewApp}&repo=${repo}${window.location.hash}`;
       } 
   })();
   ```

5. 拡張可能なAEM UI に移動してプレビュー拡張機能を読み込み、ブックマークレットをクリックします。

>[!TIP]
>
> App Builder 拡張機能が読み込まれない場合、 `&ext=https://localhost:9080`で、そのホストとポートを直接ブラウザータブで開き、自己署名証明書を受け入れます。 次に、ブックマークレットを再度お試しください。
