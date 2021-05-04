---
title: クイックセットアップSPAエディタとリモートSPA
description: 15分でリモートのSPAおよびAEM SPAエディタを起動し、実行する方法を学びます。
topic: ヘッドレス、SPA、開発
feature: SPAエディター，コアコンポーネント， API，開発
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: kt-7629.jpeg
translation-type: tm+mt
source-git-commit: 2efb7050b0b0c783c5f34c1f2e375cf21fa7a6cd
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 5%

---


# クイックセットアップ

クイックセットアップは、WKND Appをインストールして実行する方法、およびリモートSPAとして実行する方法を示す、AEM SPA Editorを使用して作成する迅速なウォークスルーです。

クイックセットアップでは、このチュートリアルの終了状態を直接確認できます。

## 前提条件

このチュートリアルでは、以下が必要です。

+ [AEM as a Cloud Service の SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14 以降](https://nodejs.org/ja/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip以降](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphqlソースコード](https://github.com/adobe/aem-guides-wknd-graphql)

このチュートリアルでは、次の点を前提としています。

+ [IDEのMicrosoft® Visual Studio](https://visualstudio.microsoft.com/) コード
+ `~/Code/wknd-app`の作業ディレクトリ
+ `http://localhost:4502`上での作成者サービスとしてのAEM SDKの実行
+ ローカルの`admin`アカウントとパスワード`admin`を使用したAEM SDKの実行
+ `http://localhost:3000`上でSPAを実行する

## AEM SDKクイックスタートの開始

デフォルトの`admin/admin`資格情報を使用して、ポート4502にAEM SDK Quickstartをダウンロードしてインストールします。

1. [最新のAEM SDKをダウンロード](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. AEM SDKを`~/aem-sdk`に解凍
1. AEM SDK Quickstart Jarの実行

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDKは開始し、[http://localhost:4502](http://localhost:4502)で自動的に起動します。 次の資格情報を使用してログインします。

+ ユーザー名: `admin`
+ パスワード: `admin`

## WKNDサイトパッケージのダウンロードとインストール

このチュートリアルは、__WKND 0.3.0+&#39;s__&#x200B;プロジェクト（コンテンツ用）に依存しています。

1. [最新バージョンの  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)にあるAEM SDKのPackage Managerに、`admin`資格情報を使用してログインします。
1. __手順1で__  `aem-guides-wknd.all.x.x.x.zip` ダウンロードした
1. __「__&#x200B;をインストール」ボタンをタップして、`aem-guides-wknd.all-x.x.x.zip`というエントリを作成します

## WKND App SPAパッケージのダウンロードとインストール

簡単なセットアップを実行するために、チュートリアルの最終的なAEM設定とコンテンツを含むAEMパッケージが用意されています。

1. [ダウンロード `wknd-app.all.x.x.x.zip`](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [ダウンロード `wknd-app.ui.content.sample.x.x.x.zip`](./assets/quick-setup/wknd-app.ui.content.sample-1.0.0.zip)
1. [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)にあるAEM SDKのPackage Managerに、`admin`資格情報を使用してログインします。
1. __手順1で__  `wknd-app.all.x.x.x.zip` ダウンロードした
1. __「__&#x200B;をインストール」ボタンをタップして、`wknd-app.all.x.x.x.zip`というエントリを作成します
1. __手順2で__  `wknd-app.ui.content.sample.x.x.x.zip` ダウンロードした
1. __「__&#x200B;をインストール」ボタンをタップして、`wknd-app.ui.content.sample.x.x.x.zip`というエントリを作成します

## WKND Appソースのダウンロード

Github.comからWKNDアプリのソースコードをダウンロードし、このチュートリアルで実行するSPAに対する変更を含むブランチを切り替えます。

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
$ git checkout -b feature/spa-editor
$ git pull origin feature/spa-editor
```

## SPAアプリケーションの開始

プロジェクトのルートから、SPAプロジェクトのnpm依存関係をインストールし、アプリケーションを実行します。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

`npm install`の実行中にエラーが発生した場合は、次の手順を実行してください。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

SPAが[http://localhost:3000](http://localhost:3000)で実行されていることを確認します。

## AEM SPA Editorでのコンテンツの作成

コンテンツをオーサリングする前に、AEM作成者(`http://localhost:4502`)が左側に、リモートSPA(`http://localhost:3000`)が右側に表示されるようにブラウザーウィンドウを調整します。 この配置により、AEMソースのコンテンツに対する変更がSPAに直ちに反映される方法を確認できます。

1. [AEM SDK作成者サービス](http://localhost:4502)に`admin`としてログインします
1. __サイト/WKND App > us > en__&#x200B;に移動します。
1. __WKNDアプリホームページ__&#x200B;を編集
1. __編集__&#x200B;モードに切り替え

### ホーム表示の固定コンポーネントのオーサリング

1. テキスト&#x200B;__WKND Adventures__&#x200B;をタップして、固定タイトルコンポーネントをアクティブにします(SPA Home表示にハードコード化)
1. タイトルコンポーネントのアクションバーにある&#x200B;__レンチ__&#x200B;アイコンをタップします
1. Titleコンポーネントのコンテンツを変更し、保存します
1. `http://localhost:3000`上で実行しているSPAを更新し、変更が反映されていることを確認します

### ホーム表示のコンテナコンポーネントの作成

1. __WKNDアプリホームページ__&#x200B;を編集中…
1. __SPAエディターのサイドバー__&#x200B;を展開します（左側）。
1. __コンポーネント__&#x200B;アイコンをタップします
1. WKNDロゴの下、追加および固定タイトルコンポーネントの上に配置されたコンテナコンポーネントから、コンポーネントを、変更、または削除します。
1. `http://localhost:3000`上で実行しているSPAを更新し、変更が反映されていることを確認します

### 動的ルートでのコンテナコンポーネントの作成

1. SPAエディタで&#x200B;__プレビュー__&#x200B;モードに切り替え
1. __Bali Surf Camp__&#x200B;カードをタップし、動的ルートに移動します
1. 追加&#x200B;__Itinerary__&#x200B;の見出しの上にあるコンテナコンポーネントのコンポーネント、変更、または削除
1. `http://localhost:3000`上で実行しているSPAを更新し、変更が反映されていることを確認します

## バリデーターが

AEM SPA Editorで、制御可能な領域を使用してSPAを拡張する方法を簡単に理解できました。 もし興味があるなら、残りのチュートリアルを見て、開始は必ず新しくしてください。この簡単なセットアップで、ローカル開発環境はチュートリアルの最後の状態になっているからです。
