---
title: クイックセットアップSPA EditorとリモートSPA
description: リモートSPAとAEM SPA Editorを15分で使い始める方法を説明します。
topic: ヘッドレス、SPA、開発
feature: SPAエディター、コアコンポーネント、API、開発
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
source-git-commit: 73c75f8dac85615f4ed2dfdcc2ee4d0e9e5d161a
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 6%

---


# クイックセットアップ

クイックセットアップは、WKNDアプリをインストールしてリモートSPAとして実行し、AEM SPA Editorを使用して作成する方法を示す、迅速なウォークスルーです。

クイックセットアップでは、このチュートリアルの終了状態に直接移動します。

>[!VIDEO](https://video.tv.adobe.com/v/333181/?quality=12&learn=on)

_クイックセットアップのビデオウォークスルー_

## 前提条件

このチュートリアルでは、次の項目が必要です。

+ [AEM as a Cloud Service の SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ja)
+ [Node.js v14 以降](https://nodejs.org/ja/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6以降](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ macOSのみの前提条件
   + [](https://developer.apple.com/xcode/) XcodeまたはXcode [コマンドラインツール](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all.0.3.0.zip以降](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphqlソースコード](https://github.com/adobe/aem-guides-wknd-graphql)


このチュートリアルでは、次の点を前提としています。

+ [IDEとしてのMicrosoft® Visual Studio ](https://visualstudio.microsoft.com/) Codeas
+ `~/Code/wknd-app`の作業ディレクトリ
+ `http://localhost:4502`でのAEM SDKのオーサーサービスとしての実行
+ パスワード`admin`を使用したローカル`admin`アカウントでのAEM SDKの実行
+ `http://localhost:3000`でのSPAの実行

## AEM SDKクイックスタートの起動

デフォルトの`admin/admin`資格情報を使用して、AEM SDK Quickstartをポート4502にダウンロードし、インストールします。

1. [最新のAEM SDKのダウンロード](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. AEM SDKを`~/aem-sdk`に解凍します。
1. AEM SDK Quickstart Jarの実行

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDKは[http://localhost:4502](http://localhost:4502)で起動し、自動的に起動します。 次の資格情報を使用してログインします。

+ ユーザー名: `admin`
+ パスワード: `admin`

## WKNDサイトパッケージをダウンロードしてインストールする

このチュートリアルは、 __WKND 0.3.0+の__&#x200B;プロジェクトに依存しています（コンテンツ用）。

1. [の最新バージョンをダウンロードする  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)にあるAEM SDKのパッケージマネージャーに、`admin`資格情報を使用してログインします。
1. ____ 手順1でダウンロ `aem-guides-wknd.all.x.x.x.zip` ードしたをアップロードします。
1. エントリ`aem-guides-wknd.all-x.x.x.zip`の&#x200B;__「__&#x200B;をインストール」ボタンをタップします

## WKND App SPAパッケージのダウンロードとインストール

簡単なセットアップを実行するために、チュートリアルの最終的なAEM設定とコンテンツを含むAEMパッケージが用意されています。

1. [ダウンロード ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [ダウンロード ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.0.zip)
1. [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)にあるAEM SDKのパッケージマネージャーに、`admin`資格情報を使用してログインします。
1. ____ 手順1でダウンロ `wknd-app.all.x.x.x.zip` ードしたをアップロードします。
1. エントリ`wknd-app.all.x.x.x.zip`の&#x200B;__「__&#x200B;をインストール」ボタンをタップします
1. ____ 手順2でダウンロ `wknd-app.ui.content.sample.x.x.x.zip` ードしたをアップロードします。
1. エントリ`wknd-app.ui.content.sample.x.x.x.zip`の&#x200B;__「__&#x200B;をインストール」ボタンをタップします

## WKNDアプリソースのダウンロード

Github.comからWKNDアプリのソースコードをダウンロードし、このチュートリアルで実行するSPAに対する変更を含むブランチを切り替えます。

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
$ git checkout -b feature/spa-editor
$ git pull origin feature/spa-editor
```

## SPAアプリケーションの起動

プロジェクトのルートから、SPA projects npmの依存関係をインストールし、アプリケーションを実行します。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

`npm install`の実行時にエラーが発生した場合は、次の手順を実行してください。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

SPAが[http://localhost:3000](http://localhost:3000)で実行されていることを確認します。

## AEM SPA Editorでのコンテンツのオーサリング

コンテンツをオーサリングする前に、AEMオーサー(`http://localhost:4502`)が左側に配置され、リモートSPA(`http://localhost:3000`)が右側に表示されるように、ブラウザーウィンドウを配置します。 この構成により、AEMソースのコンテンツに対する変更がSPAに即座に反映される方法を確認できます。

1. [AEM SDKオーサーサービス](http://localhost:4502)に`admin`としてログインします。
1. __サイト/WKND App > us > en__&#x200B;に移動します。
1. __WKNDアプリのホームページ__&#x200B;を編集
1. __編集__&#x200B;モードに切り替えます。

### ホームビューの固定コンポーネントのオーサリング

1. テキスト「__WKND Adventures__」をタップして、固定タイトルコンポーネントをアクティブ化します(SPAのホームビューにハードコード化)。
1. タイトルコンポーネントのアクションバーにある&#x200B;__レンチ__&#x200B;アイコンをタップします。
1. タイトルコンポーネントの内容を変更して保存する
1. `http://localhost:3000`上で実行中のSPAを更新し、変更が反映されていることを確認します。

### ホームビューのコンテナコンポーネントのオーサリング

1. __WKNDアプリのホームページ__&#x200B;を編集中…
1. __SPA Editorのサイドバー__（左側）を展開します。
1. __コンポーネント__&#x200B;アイコンをタップします。
1. WKNDロゴの下、および固定タイトルコンポーネントの上にあるコンテナコンポーネントに、コンポーネントを追加、変更または削除します。
1. `http://localhost:3000`上で実行中のSPAを更新し、変更が反映されていることを確認します。

### 動的ルートでのコンテナコンポーネントのオーサリング

1. SPAエディターで&#x200B;__プレビュー__&#x200B;モードに切り替えます。
1. __Bali Surf Camp__&#x200B;カードをタップし、動的なルートに移動します。
1. __旅程__&#x200B;の見出しの上にあるコンテナコンポーネントに対して、コンポーネントを追加、変更、削除します。
1. `http://localhost:3000`上で実行中のSPAを更新し、変更が反映されていることを確認します。

__WKND App Home page > Adventure__ _の下の新しいAEMページには、対応するアドベンチャーのコンテンツフラグメントの名前と一致するAEMページ名が必要です。_ これは、SPAのAEMページマッピングへのルートは、ルートの最後のセグメント（コンテンツフラグメントの名前）に基づいているからです。

## バリデーターが

AEM SPA Editorで、制御可能な編集可能な領域を使用してSPAを強化する方法を簡単に理解できました。 興味がある場合は、残りのチュートリアルを確認して、必ず新規に開始してください。このクイックセットアップでは、ローカル開発環境がチュートリアルの最後の状態になっているからです。
