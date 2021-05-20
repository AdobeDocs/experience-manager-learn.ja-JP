---
title: oak-run.jarを使用したインデックスの管理
description: oak-run.jarのindexコマンドは、インデックス統計の収集、インデックス整合性チェックの実行、インデックス自体の再/インデックス作成など、AEMでOakインデックスを管理する多数の機能を統合します。
version: 6.4, 6.5
feature: 検索
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: パフォーマンス
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 3%

---


# oak-run.jarを使用したインデックスの管理

[!DNL oak-run.jar]のindexコマンドは、インデックス統計の収集、インデックス整合性チェックの実行、インデックス自体の再インデックス/インデックス作成から、AEMで200個のインデックスを管理する多数の機能を統合しま [!DNL Oak]す。

>[!NOTE]
>
>この記事とビデオでは、インデックス作成と再インデックスという用語が同じ意味で使用され、同じ操作と見なされます。

## [!DNL oak-run.jar] indexコマンドの基本

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* 使用する[[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0)のバージョンは、AEMインスタンスで使用するOakのバージョンと一致する必要があります。
* [!DNL oak-run.jar]を使用したインデックス管理では、様々な操作をサポートするために、様々なフラグを指定した&#x200B;**[!DNL index]**&#x200B;コマンドを利用します。

   * `java -jar oak-run*.jar index ...`

## インデックスの統計

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` は、すべてのインデックス定義、重要なインデックス統計およびオフライン分析用のインデックスコンテンツをダンプします。
* インデックス統計の収集は、使用中のAEMインスタンスで安全に実行できます。

## インデックスの整合性チェック

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` lucene Oakインデックスが破損しているかどうかをすばやく判断します。
* 整合性チェックレベル1および2の使用中のAEMインスタンスでは、整合性チェックを安全に実行できます。

## [!DNL oak-run.jar]を使用したTarMKオンラインインデックス作成 {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* [!DNL oak-run.jar]を使用した[!DNL TarMK]のオンラインインデックス作成は、`oak:queryIndexDefinition`ノードで`reindex=true`を設定するよりも高速です。 このパフォーマンスの向上にもかかわらず、[!DNL oak-run.jar]を使用したオンラインインデックス作成では、インデックス作成を実行するためにメンテナンスウィンドウが必要です。

* [!DNL oak-run.jar]を使用した[!DNL TarMK]のオンラインインデックス作成は、AEMインスタンスのメンテナンスウィンドウの外部で、AEMインスタンスに対して&#x200B;****&#x200B;実行しないでください。

## oak-run.jarを使用したTarMKオフラインインデックス作成

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* [!DNL oak-run.jar]を使用した[!DNL TarMK]のオフラインインデックス作成は、[!DNL TarMK]に対する最も簡単な[!DNL oak-run.jar]ベースのインデックス作成アプローチです。1つの[!DNL oak-run.jar]コマンドが必要ですが、AEMインスタンスをシャットダウンする必要があります。

## oak-run.jarを使用したTarMK帯域外インデックス作成

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* [!DNL oak-run.jar]を使用した[!DNL TarMK]での帯域外インデックス作成は、使用中のAEMインスタンスでのインデックス作成の影響を最小限に抑えます。
* インデックス再作成の時間が使用可能なメンテナンスウィンドウを超えるAEMインストールでは、帯域外インデックス作成が推奨されるインデックス作成アプローチです。

## oak-run.jarを使用したMongoMKオンラインインデックス作成

* [!DNL MongoMK]および[!DNL RDBMK]の[!DNL oak-run.jar]を使用したオンラインインデックスは、AEMインストールの再インデックス/インデックス作成に推奨される方法です（および[!DNL RDBMK]）。 [!DNL MongoMK]**またはに対して他のメソッドは使用しな [!DNL MongoMK] いでくだ [!DNL RDBMK]さい。**
* このインデックス作成は、クラスター内の1つのAEMインスタンスに対してのみ実行する必要があります。
* [!DNL MongoMK]のオンラインインデックス作成は、実行中のAEMクラスターに対して安全に実行できます。リポジトリトラバーサルは1つの[!DNL MongoDB]ノードでのみ発生し、他のノードはパフォーマンスに大きな影響を与えることなく要求を引き続き処理できます。

[!DNL MongoMK]のオンラインインデックス作成を実行する[!DNL oak-run.jar] indexコマンドは、[ [!DNL TarMK]  [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar)を使用したオンラインインデックス作成と同じです。ただし、セグメントストアパラメーターがNodeストアを含む[!DNL MongoDB]インスタンスを指す点が異なります。

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## サポート資料

* [ダウンロード [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *ダウンロードしたバージョンが、前述のようにAEMにインストールされたOakのバージョンと一致することを確認します。*
* [Apache Jackrabbit Oak-run.jarインデックスコマンドドキュメント](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
