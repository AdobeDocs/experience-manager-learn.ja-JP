---
title: SPA エディターとリモート SPA のクイックセットアップ
description: リモート SPA と AEM SPA Editor を 15 分で使い始める方法について説明します。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer
level: Beginner
jira: KT-7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
duration: 647
hide: true
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 93%

---

# クイックセットアップ

{{spa-editor-deprecation}}

クイックセットアップの手順は簡単で、WKND アプリをリモート SPA としてインストールして実行し、AEM SPA Editor を使用してオーサリングします。

クイックセットアップを使用すると、このチュートリアルの最終状態に直接進むことができます。

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_クイックセットアップの手順を紹介するビデオ_

## 前提条件

このチュートリアルでは、次の項目が必要です。

+ [AEM as a Cloud Service の SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ja)
+ [Node.js v18](https://nodejs.org/ja/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ macOS のみの前提条件
   + [Xcode](https://developer.apple.com/xcode/) または [Xcode コマンドラインツール](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip 以降](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql ソースコード（ブランチ：feature/spa-editor）](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


このチュートリアルでは、次を前提としています。

+ IDE としての [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/)
+ `~/Code/wknd-app` の作業ディレクトリ
+ AEM SDK を `http://localhost:4502` でオーサーサービスとして実行
+ `admin`パスワードを持つローカルアカウント`admin`での AEM SDK の実行
+ `http://localhost:3000` での SPA の実行

## AEM SDK クイックスタートの開始

デフォルトの `admin/admin` 資格情報を使用して、ポート 4502 に AEM SDK クイックスタートをダウンロードしてインストールします。

1. [最新の AEM SDK をダウンロードします](https://experience.adobe.com/#/downloads/content/software-distribution/jp/aemcloud.html?fulltext=AEM*+SDK*&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=1)
1. AEM SDK を `~/aem-sdk` に解凍します
1. AEM SDK Quickstart Jar を実行します

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDKが開始し、[http://localhost:4502](http://localhost:4502) に自動的に開始します。 次の資格情報を使用してログインします。

+ ユーザー名：`admin`
+ パスワード：`admin`

## WKND サイトパッケージのダウンロードとインストール

このチュートリアルは __WKND 2.1.0+ の__&#x200B;プロジェクト（コンテンツ用）に依存しています。

1. [`aem-guides-wknd.all.x.x.x.zip` の最新バージョンをダウンロードします](https://github.com/adobe/aem-guides-wknd/releases)
1. [&#x200B; の資格情報で、:4502http://localhost](http://localhost:4502/crx/packmgr)/crx/packmgr`admin` のAEM SDK パッケージマネージャーにログインします。
1. 手順 1 でダウンロードした `aem-guides-wknd.all.x.x.x.zip` を&#x200B;__アップロードします__
1. エントリ `aem-guides-wknd.all-x.x.x.zip` の「__インストール__」ボタンをクリックします

## WKND App SPA パッケージのダウンロードとインストール

簡単な設定を実行するために、チュートリアルの最終的な AEM 設定とコンテンツを含む AEM パッケージがここに用意されています。

1. [ダウンロード &#x200B;](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [ダウンロード &#x200B;](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. [&#x200B; の資格情報で、:4502http://localhost](http://localhost:4502/crx/packmgr)/crx/packmgr`admin` のAEM SDK パッケージマネージャーにログインします。
1. 手順 1 でダウンロードした `wknd-app.all.x.x.x.zip` を&#x200B;__アップロードします__
1. エントリ `wknd-app.all.x.x.x.zip` の「__インストール__」ボタンをクリックします
1. 手順 2 でダウンロードした `wknd-app.ui.content.sample.x.x.x.zip` を __アップロード__&#x200B;します
1. エントリ `wknd-app.ui.content.sample.x.x.x.zip` の「__インストール__」ボタンをタップする

## WKND アプリソースのダウンロード

Github.com から WKND アプリのソースコードをダウンロードし、このチュートリアルで実行する SPA への変更を含むブランチを切り替えます。

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## SPA アプリケーションの起動

プロジェクトのルートから、SPA projects npm の依存関係をインストールし、アプリケーションを実行します。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

`npm install` を実行した際にエラーが発生した場合は、次の手順を試します。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

SPA が [http://localhost:3000](http://localhost:3000) で動作していることを確認します。

## AEM SPA エディターでのコンテンツのオーサリング

コンテンツをオーサリングする前に、AEM オーサー（`http://localhost:4502`）は左側で、リモート SPA（`http://localhost:3000`）は右側で実行されます。このように配置すると、AEM ソースのコンテンツに対する変更が SPA にすぐに反映されます。

1. [AEM SDK オーサーサービス](http://localhost:4502)に `admin` としてログインする
1. __Sites／WKND アプリ／米国／en__ に移動する
1. __WKND アプリのホームページ__&#x200B;を編集する
1. __編集__&#x200B;モードへ切り替える

### ホームビューの固定コンポーネントのオーサリング

1. テキスト「__WKND Adventures__」をタップして、固定タイトルコンポーネント（SPA のホームビューにハードコードされています）を有効にします。
1. タイトルコンポーネントのアクションバーにある&#x200B;__レンチ__&#x200B;アイコンをタップする
1. タイトルコンポーネントの内容を変更して保存する
1. `http://localhost:3000` 上で動作している SPA をリフレッシュして、変更が反映されていることを確認する

### ホームビューのコンテナコンポーネントのオーサリング

1. __WKND アプリのホームページ__&#x200B;を編集中...
1. __SPA エディターのサイドバー__（左側）を展開する
1. 「__コンポーネント__」アイコンをタップする
1. WKND ロゴの下、固定タイトルコンポーネントの上に位置するコンテナコンポーネントへのコンポーネントの追加、変更、削除
1. `http://localhost:3000` 上で動作している SPA を更新して、変更が反映されていることを確認する

### 動的ルート上のコンテナコンポーネントのオーサリング

1. SPA エディターの&#x200B;__プレビュー__&#x200B;モードへの切り替え
1. __Bali Surf Camp__ カードをタップして、その動的ルートに移動します。
1. __Itinerary__ 見出しの上に位置するコンテナコンポーネントを追加、変更、削除する
1. `http://localhost:3000` 上で動作している SPA を更新して、変更が反映されていることを確認する

__WKND アプリのホームページ／アドベンチャー___必須_&#x200B;の下に新規作成する AEM ページには、対応するアドベンチャーのコンテンツフラグメントの名前と同じ AEM ページ名を付ける必要があります。これは、AEM ページマッピングへの SPA ルートが、ルートの最後のセグメント（コンテンツフラグメントの名前）に基づいているからです。

## おめでとうございます。

AEM SPA エディターで、制御可能な編集可能な領域を使用して SPA を強化する方法を容易に体験していただけたと思います。興味のある方は、残りのチュートリアルをご覧ください。ただし、このクイック設定では、お使いのローカル開発環境がチュートリアルの最後の状態になっているため、必ず最初から開始するようにしてください。
