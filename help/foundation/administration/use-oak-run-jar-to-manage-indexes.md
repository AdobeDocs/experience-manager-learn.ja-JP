---
title: oak-run.jar を使用したインデックスの管理
description: oak-run.jar の index コマンドは、インデックス統計の収集、インデックス整合性チェックの実行、インデックス自体の再作成/インデックス作成から、AEMで Oak インデックスを管理する多数の機能を統合します。
version: 6.4, 6.5
feature: Search
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 4%

---

# oak-run.jar を使用したインデックスの管理

[!DNL oak-run.jar]&#39;s index コマンド管理する多数の機能を統合 [!DNL Oak]AEMの 200 個のインデックス（インデックス統計の収集、インデックス整合性チェックの実行、インデックス自体の再作成/インデックス作成）。

>[!NOTE]
>
>この記事とビデオでは、インデックス作成とインデックス再作成という用語が同じ意味で使用され、同じ操作と見なされます。

## [!DNL oak-run.jar] index コマンドの基本

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* のバージョン。 [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) を使用する場合、AEMインスタンスで使用される Oak のバージョンと一致する必要があります。
* を使用したインデックスの管理 [!DNL oak-run.jar] は **[!DNL index]** コマンドに様々なフラグを指定して、異なる操作をサポートします。

   * `java -jar oak-run*.jar index ...`

## インデックス統計

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` すべてのインデックス定義、重要なインデックス統計およびインデックス内容をオフライン分析用にダンプします。
* インデックス統計の収集は、使用中のAEMインスタンスで安全に実行できます。

## インデックスの整合性チェック

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` lucene Oak インデックスが破損しているかどうかを迅速に判断します。
* 整合性チェックレベル 1 および 2 では、使用中のAEMインスタンス上で安全に整合性チェックを実行できます。

## を使用した TarMK オンラインインデックス作成 [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* のオンラインインデックス作成 [!DNL TarMK] using [!DNL oak-run.jar] 設定より速い `reindex=true` の `oak:queryIndexDefinition` ノード。 このパフォーマンスは向上しますが、 [!DNL oak-run.jar] インデックス作成を実行するには、メンテナンスウィンドウが必要です。

* のオンラインインデックス作成 [!DNL TarMK] using [!DNL oak-run.jar] should **not** は、AEMインスタンスのメンテナンスウィンドウ外でAEMインスタンスに対して実行されます。

## oak-run.jar を使用した TarMK オフラインインデックス作成

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* のオフラインインデックス作成 [!DNL TarMK] using [!DNL oak-run.jar] は最も単純な [!DNL oak-run.jar] ～に対するベースのインデックス作成手法 [!DNL TarMK] 単一の [!DNL oak-run.jar] コマンドを使用しますが、AEMインスタンスをシャットダウンする必要があります。

## oak-run.jar を使用した TarMK 帯域外インデックス作成

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* での帯域外インデックス作成 [!DNL TarMK] using [!DNL oak-run.jar] 使用中のAEMインスタンスに対するインデックス作成の影響を最小限に抑えます。
* インデックスの再作成に要する時間が使用可能なメンテナンスウィンドウを超えるAEMインストールでは、帯域外インデックス作成は、推奨されるインデックス作成アプローチです。

## oak-run.jar を使用した MongoMK オンラインインデックス作成

* 次を使用したオンラインインデックス [!DNL oak-run.jar] オン [!DNL MongoMK] および [!DNL RDBMK] は、インデックスの再作成/作成に推奨される方法です。 [!DNL MongoMK] ( および [!DNL RDBMK]) AEMのインストール。 **に対して他の方法は使用しないでください。 [!DNL MongoMK] または [!DNL RDBMK].**
* このインデックス作成は、クラスター内の 1 つのAEMインスタンスに対してのみ実行する必要があります。
* のオンラインインデックス作成 [!DNL MongoMK] は、リポジトリトラバーサルが 1 つでしか発生しないので、実行中のAEMクラスターに対して安全に実行できます [!DNL MongoDB] ノードを使用して、他のユーザーがパフォーマンスに大きな影響を与えることなく、引き続きリクエストを処理できるようにします。

この [!DNL oak-run.jar] のオンラインインデックス作成を実行するインデックスコマンド [!DNL MongoMK] が [同じ [!DNL TarMK] を使用したオンラインインデックス作成 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) が、セグメントストアパラメーターが [!DNL MongoDB] Node ストアを格納するインスタンス。

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## サポート資料

* [ダウンロード [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *ダウンロードしたバージョンが、前述のようにAEMにインストールされた Oak のバージョンと一致していることを確認します。*
* [Apache Jackrabbit Oak-run.jar インデックスコマンドドキュメント](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
