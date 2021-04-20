---
title: oak-run.jarを使用したインデックスの管理
description: oak-run.jarのindexコマンドは、インデックス統計の収集、インデックス整合性チェックの実行、インデックス自体の再インデックス作成から、AEMでOakインデックスを管理する多数の機能を統合します。
version: 6.4, 6.5
feature: 検索
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: パフォーマンス
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 3%

---


# oak-run.jarを使用したインデックスの管理

[!DNL oak-run.jar]&#39;s indexコマンドは、インデックス統計の収集、インデックス整合性チェックの実行、インデックス自体の再作成/インデックス作成から、AEMで [!DNL Oak]200個のインデックスを管理する多数の機能を統合します。

>[!NOTE]
>
>この記事およびビデオでは、インデックス作成と再インデックスという用語が同じ意味で使用され、同じ操作と見なされます。

## [!DNL oak-run.jar] indexコマンドの基本

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* 使用する[[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0)のバージョンは、AEMインスタンスで使用するOakのバージョンと一致する必要があります。
* [!DNL oak-run.jar]を使用したインデックスの管理では、**[!DNL index]**&#x200B;コマンドを様々なフラグと共に使用して、異なる操作をサポートします。

   * `java -jar oak-run*.jar index ...`

## インデックスの統計

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` は、すべてのインデックス定義、重要なインデックス統計およびオフライン分析用のインデックスコンテンツをダンプします。
* インデックス統計の収集は、使用中のAEMインスタンスで安全に実行できます。

## インデックスの整合性チェック

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` lucene Oakインデックスが壊れているかどうかをすばやく確認します。
* 整合性チェックは、整合性チェックレベル1と2の使用中のAEMインスタンスで安全に実行できます。

## [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}でのTarMKオンラインインデックス

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* [!DNL oak-run.jar]を使用した[!DNL TarMK]のオンラインインデックスは、`oak:queryIndexDefinition`ノードで`reindex=true`を設定するよりも高速です。 このパフォーマンスは向上しますが、[!DNL oak-run.jar]を使用したオンラインインデックス作成では、インデックス作成を実行するためにメンテナンスウィンドウが必要です。

* [!DNL oak-run.jar]を使用した[!DNL TarMK]のオンラインインデックス作成は、AEMインスタンスメンテナンスウィンドウの外部にあるAEMインスタンスに対しては、****&#x200B;実行しないでください。

## oak-run.jarを使用したTarMKオフラインインデックス

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* [!DNL oak-run.jar]を使用した[!DNL TarMK]のオフラインインデックス作成は、1つの[!DNL oak-run.jar]コマンドが必要な[!DNL TarMK]の場合に、[!DNL oak-run.jar]に基づく最も簡単なインデックス作成アプローチですが、AEMインスタンスをシャットダウンする必要があります。

## TarMK oak-run.jarを使用した帯域外インデックス

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* [!DNL TarMK]で[!DNL oak-run.jar]を使用してに対する帯域外インデックスを作成すると、使用中のAEMインスタンスに対するインデックス作成の影響を最小限に抑えることができます。
* 帯域外インデックスは、再インデックス処理に要する時間が利用可能なメンテナンス期間を超えるAEMでのインデックス処理に推奨される方法です。

## oak-run.jarを使用したMongoMKオンラインインデックス

* [!DNL MongoMK]の[!DNL oak-run.jar]と[!DNL RDBMK]のオンラインインデックスは、[!DNL MongoMK] （および[!DNL RDBMK]） AEMのインストールの再/インデックス付けに推奨される方法です。 **またはに対して他のメソッドを使用し [!DNL MongoMK] ないでください [!DNL RDBMK]。**
* このインデックス作成は、クラスター内の1つのAEMインスタンスに対してのみ実行する必要があります。
* [!DNL MongoMK]のオンラインインデックスは、実行中のAEMクラスタに対して安全に実行できます。リポジトリトラバーサルは1つの[!DNL MongoDB]ノードで行われ、他のノードはパフォーマンスに大きな影響を与えることなく要求を処理し続けます。

[!DNL MongoMK]のオンラインインデックスを実行する[!DNL oak-run.jar] indexコマンドは、[ [!DNL TarMK]  [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar)を使用したオンラインインデックスと同じです。ただし、セグメントストアパラメーターがNode Storeを含む[!DNL MongoDB]インスタンスを指すのとは異なります。

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## サポート資料

* [ダウンロード [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *ダウンロードしたバージョンが、上記の説明に従ってAEMにインストールされたOakのバージョンと一致することを確認します*
* [Apache Jackrabbit Oak-run.jarインデックスコマンドドキュメント](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
