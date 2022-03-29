---
title: AEM SDK をデバッグするその他のツール
description: その他の様々なツールが、AEM SDK のローカルクイックスタートのデバッグに役立ちます。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 5%

---

# AEM SDK をデバッグするその他のツール

その他の様々なツールが、AEM SDK のローカルクイックスタートでのアプリケーションのデバッグに役立ちます。

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Liteは、JCR(AEMデータリポジトリ ) とやり取りするための Web ベースのインターフェイスです。 CRXDE Liteは、ノード、プロパティ、プロパティ値、権限など、JCR を完全に表示できます。

CRXDE Liteは次の場所にあります。

+ ツール／一般／CRXDE Lite
+ または直接 [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### コンテンツのデバッグ

CRXDE Liteは、JCR に直接アクセスできます。 CRXDE Lite経由で表示されるコンテンツは、ユーザーに付与された権限によって制限されます。つまり、アクセス権に応じて、JCR 内のすべての項目を表示または変更できない場合があります。

+ JCR 構造は、左側のナビゲーションパネルを使用してナビゲーションおよび操作します
+ 左側のナビゲーションペインでノードを選択すると、ノードプロパティのが下側のペインに表示されます。
   + プロパティは、ウィンドウから追加、削除、または変更できます
+ 左側のナビゲーションでファイルノードをダブルクリックすると、右上のペインにファイルのコンテンツが開きます
+ 左上の「すべて保存」ボタンをタップして変更内容を保存するか、「すべて保存」の横の下向き矢印をタップして未保存の変更内容を元に戻します。

![CRXDE Lite — コンテンツのデバッグ](./assets/other-tools/crxde-lite__debugging-content.png)

CRXDE Liteを介してAEM SDK に直接加えられた変更は、追跡と管理が困難な場合があります。 必要に応じて、CRXDE Liteを通じて行われた変更をAEMプロジェクトの可変コンテンツパッケージ (`ui.content`) を送信し、Git にコミットします。 CRXDE Liteを介してAEM SDK に直接変更するのではなく、すべてのアプリケーションコンテンツの変更はコードベースから始まり、デプロイメントを介してAEM SDK に送られるのが理想です。

### アクセス制御のデバッグ

CRXDE Liteは、特定のユーザーまたはグループ（プリンシパル）の特定のノードに対するアクセス制御をテストおよび評価する方法を提供します。

CRXDE Liteの「アクセス制御をテスト」コンソールにアクセスするには、次の場所に移動します。

+ CRXDE Lite/ツール/アクセス制御をテスト…

![CRXDE Lite — アクセス制御のテスト](./assets/other-tools/crxde-lite__test-access-control.png)

1. 「パス」フィールドを使用して、評価する JCR パスを選択します
1. 「プリンシパル」フィールドを使用して、パスの値を設定するユーザーまたはグループを選択します
1. 「テスト」ボタンをタップします。

結果は次のように表示されます。

+ __パス__ 評価されたパスを繰り返します
+ __プリンシパル__ パスが評価されたユーザーまたはグループを繰り返します
+ __プリンシパル__ 選択したプリンシパルが属するすべてのプリンシパルの一覧が表示されます。
   + 継承を介して権限を提供する可能性のある推移的なグループメンバーシップを理解すると役立ちます。
+ __パスでの権限__ 選択したプリンシパルが評価されたパスに対して持つすべての JCR 権限をリストします

## クエリの説明を実行

![クエリの説明を実行](./assets/other-tools/explain-query.png)

AEM SDK のローカルクイックスタートで、AEMによるクエリの解釈と実行方法に関する重要なインサイトを提供するクエリ Web ベースのツールと、AEMによるクエリがパフォーマンスの高い方法で実行されることを確認する非常に役立つツールについて説明します。

「クエリの説明を実行」は次の場所にあります。

+ ツール/診断/クエリパフォーマンス/「クエリの説明を実行」タブ
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) /「クエリの説明を実行」タブ

## QueryBuilder デバッガー

![QueryBuilder デバッガー](./assets/other-tools/query-debugger.png)

QueryBuilder Debugger は、AEMを使用した検索クエリのデバッグと理解に役立つ Web ベースのツールです。 [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) 構文と同じです。

QueryBuilder Debugger は次の場所にあります。

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
