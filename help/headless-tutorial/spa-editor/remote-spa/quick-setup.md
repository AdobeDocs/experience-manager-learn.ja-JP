---
title: クイックセットアップSPA Editor とリモートSPA
description: リモートSPAとAEM SPA Editor を 15 分で使い始める方法を説明します。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 6%

---

# クイックセットアップ

クイックセットアップは、WKND アプリとリモートSPAをインストールして実行し、AEM SPA Editor を使用して作成する方法を示す、迅速なウォークスルーです。

クイックセットアップを使用すると、このチュートリアルの終了状態に直接移動します。

>[!VIDEO](https://video.tv.adobe.com/v/333181/?quality=12&learn=on)

_クイックセットアップのビデオウォークスルー_

## 前提条件

このチュートリアルでは、次の項目が必要です。

+ [AEM as a Cloud Service の SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ja)
+ [Node.js v16 以降](https://nodejs.org/ja/)
+ [npm v8+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6 以降](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ macOSのみの前提条件
   + [Xcode](https://developer.apple.com/xcode/) または [Xcode コマンドラインツール](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip 以降](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql ソースコード ( ブランチ：feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


このチュートリアルでは、次を前提としています。

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) IDE として
+ の作業ディレクトリ `~/Code/wknd-app`
+ AEM SDK as a Author サービスを `http://localhost:4502`
+ ローカルでのAEM SDK の実行 `admin` パスワードを持つアカウント `admin`
+ でのSPAの実行 `http://localhost:3000`

## AEM SDK Quickstart の起動

デフォルトで、ポート 4502 にAEM SDK Quickstart をダウンロードしてインストールする `admin/admin` 資格情報。

1. [最新のAEM SDK をダウンロード](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. AEM SDK をに解凍します。 `~/aem-sdk`
1. AEM SDK Quickstart Jar の実行

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK が起動し、次の日に自動的に起動 [http://localhost:4502](http://localhost:4502). 次の資格情報を使用してログインします。

+ ユーザー名: `admin`
+ パスワード: `admin`

## WKND サイトパッケージをダウンロードしてインストールする

このチュートリアルは、 __WKND 2.1.0 以降__ プロジェクト（コンテンツ用）

1. [最新バージョンのをダウンロード `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. AEM SDK のパッケージマネージャー ( ) にログインします。 [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) と `admin` 資格情報。
1. __アップロード__ の `aem-guides-wknd.all.x.x.x.zip` 手順 1 でダウンロード済み
1. 次をタップします。 __インストール__ エントリのボタン `aem-guides-wknd.all-x.x.x.zip`

## WKND App SPAパッケージをダウンロードしてインストールする

簡単なセットアップを実行するために、チュートリアルの最終的なAEM設定とコンテンツを含むAEMパッケージがここに用意されています。

1. [ダウンロード ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [ダウンロード ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. AEM SDK のパッケージマネージャー ( ) にログインします。 [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) と `admin` 資格情報。
1. __アップロード__ の `wknd-app.all.x.x.x.zip` 手順 1 でダウンロード済み
1. 次をタップします。 __インストール__ エントリのボタン `wknd-app.all.x.x.x.zip`
1. __アップロード__ の `wknd-app.ui.content.sample.x.x.x.zip` 手順 2 でダウンロード
1. 次をタップします。 __インストール__ エントリのボタン `wknd-app.ui.content.sample.x.x.x.zip`

## WKND アプリソースのダウンロード

Github.com から WKND アプリのソースコードをダウンロードし、このチュートリアルで実行するSPAへの変更を含むブランチを切り替えます。

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## SPAアプリケーションを起動します。

プロジェクトのルートから、SPA projects npm の依存関係をインストールし、アプリケーションを実行します。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

実行時にエラーが発生した場合 `npm install` 次の手順を試します。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

SPAが [http://localhost:3000](http://localhost:3000).

## AEM SPA Editor でのコンテンツのオーサリング

コンテンツをオーサリングする前に、AEM オーサー (`http://localhost:4502`) が左に、リモートSPA (`http://localhost:3000`) は右側で実行されます。 このように配置すると、AEMソースのコンテンツに対する変更がSPAに直ちに反映されます。

1. にログインします。 [AEM SDK Author サービス](http://localhost:4502) as `admin`
1. に移動します。 __サイト/ WKND アプリ/米国/en__
1. 編集 __WKND アプリのホームページ__
1. 切り替え先 __編集__ mode

### ホームビューの固定コンポーネントのオーサリング

1. テキストをタップ __WKND アドベンチャ__ 固定タイトルコンポーネントをアクティブにするには (SPAのホームビューにハードコードされます ):
1. 次をタップします。 __レンチ__ タイトルコンポーネントのアクションバーのアイコン
1. タイトルコンポーネントの内容を変更して保存する
1. 実行中のSPAの更新 `http://localhost:3000` そして変化が反映されていることを確認する

### ホームビューのコンテナコンポーネントのオーサリング

1. を編集中 __WKND アプリのホームページ__...
1. を展開します。 __SPAエディターのサイドバー__ （左側）
1. 次をタップします。 __コンポーネント__ アイコン
1. WKND ロゴの下、および固定タイトルコンポーネントの上に配置されるコンテナコンポーネントに、コンポーネントを追加、変更または削除します。
1. 実行中のSPAの更新 `http://localhost:3000` そして変化が反映されていることを確認する

### 動的ルート上のコンテナコンポーネントのオーサリング

1. 切り替え先 __プレビュー__ SPA Editor のモード
1. をタップします。 __バリサーフキャンプ__ カードを開き、動的ルートに移動します。
1. 上の __旅程__ 見出し
1. 実行中のSPAの更新 `http://localhost:3000` そして変化が反映されていることを確認する

新しいAEMページ ( __WKND アプリのホームページ/アドベンチャー__ _必須_ 対応するアドベンチャーのコンテンツフラグメントの名前に一致するAEMページ名がある。 これは、AEMページマッピングへのSPAルートが、ルートの最後のセグメント（コンテンツフラグメントの名前）に基づいているからです。

## おめでとうございます。

AEM SPA Editor で、制御可能な編集可能な領域を使用してSPAを強化する方法を簡単に理解できました。 興味のある方は、残りのチュートリアルをご覧ください。ただし、必ず新しく開始してください。このクイックセットアップでは、ローカル開発環境がチュートリアルの最後の状態になっているからです。
