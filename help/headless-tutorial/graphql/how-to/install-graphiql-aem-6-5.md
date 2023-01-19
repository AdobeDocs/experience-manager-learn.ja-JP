---
title: AEM 6.5.X への GraphiQL IDE のインストール
description: AEM 6.5.X バージョンに GraphiQL IDE をインストールして設定する方法を説明します。
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 11614
thumbnail: KT-10253.jpeg
source-git-commit: ae27cbc50fc5c4c2e8215d7946887b99d480d668
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 24%

---


# AEM 6.5.X への GraphiQL IDE のインストール

AEM 6.5 では、GraphiQL IDE ツールを手動でインストールする必要があります。インストールと設定の手順は次のとおりです。

1. **[ソフトウェア配布ポータル](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)**／**AEM as a Cloud Service** に移動します。
1. 「GraphiQL」を検索します（**GraphiQL** の **i** は必ず入れてください）。
1. 最新の **GraphiQL コンテンツパッケージ v.x.x.x** をダウンロードします。

   ![GraphiQL パッケージをダウンロード](assets/graphiql/software-distribution.png)

   zip ファイルは、直接インストールできるAEMパッケージです。

1. 「AEM Start」メニューから、に移動します。 **ツール** > **導入** > **パッケージ**.
1. 「**パッケージをアップロード**」をクリックし、前の手順でダウンロードしたパッケージを選択します。「**インストール**」をクリックして、パッケージをインストールします。

   ![GraphiQL パッケージのインストール](assets/graphiql/install-graphiql-package.png)

1. に移動します。 **CRXDE Lite** > **リポジトリパネル** > 選択 `/content/graphiql` ノード ( 例： <http://localhost:4502/crx/de/index.jsp#/content/graphiql>) をクリックします。
1. 内 **プロパティ** タブの値の変更 `endpoint` プロパティを `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![エンドポイントプロパティ値の変更](assets/graphiql/endpoint-prop-value-change.png)

1. 次に移動： **Web コンソール設定** UI /を検索 **CSRF フィルタ** 設定 ( 例：<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. 内 `Excluded Paths` プロパティ名フィールドが更新されました。WKND GraphQLエンドポイントのパスは次のとおりです。 `/content/cq:graphql/wknd-shared/endpoint`.
   ![Exclude Paths プロパティ値の変更](assets/graphiql/exclude-paths-value-change.png)

1. 次を使用して GraphiQL エディターにアクセスします。 `//HOST:PORT/content/graphiql.html`をクリックし、新しいクエリを作成できるか、既存のクエリを実行できるかを確認します。 ( 例： <http://localhost:4502/content/graphiql.html>)

![GraphiQL エディタ](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>プロジェクト固有のGraphQLスキーマとクエリの実行をサポートするには、 `endpoint` および `Excluded Paths` 上記の手順の値。
