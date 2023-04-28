---
title: AEM コンテンツフラグメントコンソール拡張機能のテスト
description: 実稼動環境にデプロイする前に AEM コンテンツフラグメントコンソール拡張機能をテストする方法について説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '582'
ht-degree: 100%

---

# 拡張機能のテスト

AEM コンテンツフラグメントコンソール拡張機能は、拡張機能が属する Adobe 組織内の任意の AEM as a Cloud Service 環境に対してテストできます。

拡張機能のテストは、AEM コンテンツフラグメントコンソールに拡張機能を読み込むように指示する、特別に作成された URL を介して行われます。

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

## AEM コンテンツフラグメントコンソールの URL

![AEM コンテンツフラグメントコンソールの URL](./assets/test/content-fragment-console-url.png){align="center"}

実稼動以外の拡張機能を AEM コンテンツフラグメントコンソールにマウントする URL を作成するには、目的の AEM コンテンツフラグメントコンソールの URL を収集する必要があります。AEM as a Cloud Service 環境に移動して拡張機能をテストし、その AEM コンテンツフラグメントコンソールの URL をコピーします。

1. 目的の AEM as a Cloud Service 環境にログインします。

   + [開発ビルドのテスト](#testing-development-builds)に AEM 開発環境を使用
   + [ステージビルドのテスト](#testing-stage-builds)に AEM ステージまたは QA 開発環境を使用

1. 「__コンテンツフラグメント__」アイコンを選択します。
1. AEM コンテンツフラグメントコンソールがブラウザーに読み込まれるまで待ちます。
1. AEM コンテンツフラグメントコンソールの URL をブラウザーのアドレスバーからコピーします。URL は以下のようになります。

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

この URL は、開発およびステージテスト用の URL を作成する際に以下で使用します。

## ローカル開発ビルドのテスト

1. 拡張機能プロジェクトのルートへのコマンドラインを開きます。
1. AEM コンテンツフラグメントコンソール拡張機能をローカルの App Builder アプリとして実行します

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

1. 次の 2 つのクエリパラメーターを [AEM コンテンツフラグメントコンソールの URL](#aem-content-fragment-console-url) に追加します
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`、通常は `&ext=https://localhost:9080` です。

   コンテンツフラグメントコンソールはハッシュルート（`#/@wknd/aem/...`）を使用するので、上記の 2 つのクエリパラメーター（`devMode` と `ext`）を URL の&#x200B;__最初__&#x200B;のクエリパラメーターとして追加します。そのため、誤って `#` の後にパラメーターを後置すると機能しません。

   テスト URL は次のようになります。

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. テスト URL をコピーして、ブラウザーに貼り付けます。

   + 最初に、それからは定期的に、ローカルアプリケーションのホスト（`https://localhost:9080`）の [HTTPS 証明書を受け入れる](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users)必要があります。

1. AEM コンテンツフラグメントコンソールは、テスト用に挿入された拡張機能のローカルバージョンで読み込まれ、ローカルの App Builder アプリケーションが実行中である限り、変更をホットリロードします。

>[!IMPORTANT]
>
>この方法を使用する場合、開発中の拡張機能はご自身のエクスペリエンスにのみ影響します。AEM コンテンツフラグメントコンソールを使用するその他のユーザーは、挿入された拡張機能なしでその拡張機能にアクセスします。


## テストステージのビルド

1. 拡張機能プロジェクトのルートへのコマンドラインを開きます。
1. ステージワークスペースがアクティブ（またはテストに使用するワークスペース）であることを確認します。

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

1. 次の 2 つのクエリパラメーターを [AEM コンテンツフラグメントコンソールの URL](#aem-content-fragment-console-url) に追加します
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   コンテンツフラグメントコンソールはハッシュルート（`#/@wknd/aem/...`）を使用するので、上記の 2 つのクエリパラメーター（`devMode` と `ext`）を URL の&#x200B;__最初__&#x200B;のクエリパラメーターとして追加します。そのため、誤って `#` の後にパラメーターを後置すると機能しません。

   テスト URL は次のようになります。

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. テスト URL をコピーして、ブラウザーに貼り付けます。
1. AEM コンテンツフラグメントコンソールは、ステージワークスペースにデプロイされた拡張機能のバージョンを挿入します。このステージ URL は、テスト用に QA またはビジネスユーザーに共有できます。

この方法を使用する場合、ステージングされた拡張機能は、作成されたステージ URL を使用してアクセスする際に AEM コンテンツフラグメントコンソールにのみ挿入されます。

1. デプロイされた拡張機能は、`aio app deploy` を再度実行することで更新できます。これらの変更は、テスト URL を使用すると自動的に反映されます。
1. テスト用に拡張機能を削除するには、`aio app undeploy` を実行します。
