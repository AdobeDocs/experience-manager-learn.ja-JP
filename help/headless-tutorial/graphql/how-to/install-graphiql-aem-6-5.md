---
title: AEM 6.5 への GraphiQL IDE のインストール
description: AEM 6.5 に GraphiQL IDE をインストールして設定する方法を説明します
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-11614
thumbnail: KT-10253.jpeg
exl-id: 04fcc24c-7433-4443-a109-f01840ef1a89
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '208'
ht-degree: 100%

---

# AEM 6.5 への GraphiQL IDE のインストール

AEM 6.5 では、GraphiQL IDE ツールを手動でインストールする必要があります。

1. **[ソフトウェア配布ポータル](https://experience.adobe.com/#/downloads/content/software-distribution/jp/aemcloud.html)**／**AEM as a Cloud Service** に移動します。
1. 「GraphiQL」を検索します（**GraphiQL** の **i** は必ず入れてください）。
1. 最新の **GraphiQL コンテンツパッケージ v.x.x.x** をダウンロードします。

   ![GraphiQL パッケージのダウンロード](assets/graphiql/software-distribution.png)

   この zip ファイルは、直接インストールできる AEM のパッケージです。

1. AEM のスタートメニューから、**ツール**／**デプロイメント**／**パッケージ**&#x200B;に移動します。
1. 「**パッケージをアップロード**」をクリックし、前の手順でダウンロードしたパッケージを選択します。「**インストール**」をクリックして、パッケージをインストールします。

   ![GraphiQL パッケージのインストール](assets/graphiql/install-graphiql-package.png)

1. **CRXDE Lite**／**リポジトリパネル**&#x200B;に移動し、`/content/graphiql` ノード（<http://localhost:4502/crx/de/index.jsp#/content/graphiql> など）を選択します。
1. 「**プロパティ**」タブで、`endpoint` プロパティの値を `/content/_cq_graphql/wknd-shared/endpoint.json` に変更します。
   ![エンドポイントプロパティ値の変更](assets/graphiql/endpoint-prop-value-change.png)

1. **Web コンソール設定** UI に移動し、**CSRF フィルター**&#x200B;設定（<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)> など）を検索します。
1. `Excluded Paths` プロパティ名フィールドで、WKND GraphQL エンドポイントのパスを `/content/cq:graphql/wknd-shared/endpoint` に更新します。

![Exclude Paths プロパティ値の変更](assets/graphiql/exclude-paths-value-change.png)

1. `//HOST:PORT/content/graphiql.html` を使用して GraphiQL エディターにアクセスし、新しいクエリを作成できるか、または既存のクエリを実行できるか確認します（例 <http://localhost:4502/content/graphiql.html>）。

![GraphiQL エディター](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>プロジェクト固有の GraphQL スキーマとクエリの実行に対応するには、上記の手順で `endpoint` と `Excluded Paths` の値に対応する変更を加える必要があります。
