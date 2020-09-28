---
title: oak-run.jarを使用したインデックスの管理
description: oak-run.jarのindexコマンドは、インデックス統計の収集、インデックス整合性チェックの実行、インデックス自体の再インデックス作成から、AEMでOakインデックスを管理する多数の機能を統合します。
version: 6.4, 6.5
feature: oak
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
translation-type: tm+mt
source-git-commit: e19e177589df7ce6a56c0be3f9d590cbca2f8ce7
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

* 使用する [[!DNL oak-run.jar]のバージョンは、AEMインスタンスで使用するOakのバージョンと一致する必要があります](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) 。
* を使用したインデックスの管理では、様々な操作をサポートするために、様々なフラグを付けた [!DNL oak-run.jar]**[!DNL index]** コマンドを使用します。

   * `java -jar oak-run*.jar index ...`

## インデックスの統計

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` は、すべてのインデックス定義、重要なインデックス統計およびオフライン分析用のインデックスコンテンツをダンプします。
* インデックス統計の収集は、使用中のAEMインスタンスで安全に実行できます。

## インデックスの整合性チェック

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` lucene Oakインデックスが壊れているかどうかをすばやく確認します。
* 整合性チェックは、整合性チェックレベル1と2の使用中のAEMインスタンスで安全に実行できます。

## TarMKオンラインインデックス [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* を使用したオンラインインデックス [!DNL TarMK] の方が、 [!DNL oak-run.jar] ノードでの設定よりも高速 `reindex=true``oak:queryIndexDefinition` です。 このパフォーマンスは向上しますが、を使用したオンラインインデックス作成には、インデックス作成を実行するた [!DNL oak-run.jar] めのメンテナンスウィンドウが必要です。

* を使用したオンラインインデックス [!DNL TarMK] は、AEMインスタンスメンテナンスウィンドウの外部にあるAEMインスタンスに対して実行しないで [!DNL oak-run.jar] ください **** 。

## oak-run.jarを使用したTarMKオフラインインデックス

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* オフラインでのインデックス作成 [!DNL TarMK] は、1つの [!DNL oak-run.jar] コマンドが必要なので、に対する最もシンプルな [!DNL oak-run.jar][!DNL TarMK][!DNL oak-run.jar] ベースのインデックス作成アプローチですが、AEMインスタンスをシャットダウンする必要があります。

## TarMK oak-run.jarを使用した帯域外インデックス

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* 使用時の帯域外インデックス作成は、使用中のAEMインスタンス [!DNL TarMK] に対するインデックス作成の影響を最小限 [!DNL oak-run.jar] に抑えます。
* 帯域外インデックスは、再インデックス処理に要する時間が利用可能なメンテナンス期間を超えるAEMでのインデックス処理に推奨される方法です。

## oak-run.jarを使用したMongoMKオンラインインデックス

* オンのオンラインインデックス [!DNL oak-run.jar] と [!DNL MongoMK] は、AEMでのインデックスの再作成 [!DNL RDBMK] (および [!DNL MongoMK][!DNL RDBMK])に推奨される方法です。 **またはに対して他のメソッドを使用し[!DNL MongoMK]ないでくだ[!DNL RDBMK]さい。**
* このインデックス作成は、クラスター内の1つのAEMインスタンスに対してのみ実行する必要があります。
* オンラインインデックス [!DNL MongoMK] の作成は、実行中のAEMクラスタに対して安全に実行できます。リポジトリトラバーサルは1つの [!DNL MongoDB] ノードでのみ発生し、他のノードはパフォーマンスに大きな影響を与えることなく要求を処理し続けることができます。

のオンラインインデックス作成を実行する [!DNL oak-run.jar] インデックスコマンド [!DNL MongoMK] は [、セグメントストアパラメータがNode storeを含む [!DNL TarMK]  [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar)[!DNL MongoDB] インスタンスを指すのとは異なる値を持つ、Onlineインデックスと同じです。

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
