---
title: oak-run.jar を使用したインデックスの管理
description: oak-run.jar の index コマンドには、インデックス統計の収集、インデックスの整合性チェックの実行、インデックス自体の作成／再作成など、AEM で Oak インデックスを管理するための様々な機能が統合されています。
version: 6.4, 6.5
feature: Search
doc-type: Technical Video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
duration: 726
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '427'
ht-degree: 100%

---

# oak-run.jar を使用したインデックスの管理

[!DNL oak-run.jar] の index コマンドには、インデックス統計の収集、インデックスの整合性チェックの実行、インデックス自体の作成／再作成など、AEM で [!DNL Oak]200 インデックスを管理するための様々な機能が統合されています。

>[!NOTE]
>
>この記事とビデオでは、インデックス作成とインデックス再作成という用語が同じ意味に使用され、同じ操作と見なされています。

## [!DNL oak-run.jar] index コマンドの基本

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* 使用される [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) のバージョンは、AEM インスタンスで使用される Oak のバージョンと一致する必要があります。
* [!DNL oak-run.jar] を使用してインデックスを管理する場合は、**[!DNL index]** コマンドを様々なフラグと共に活用して、様々な操作をサポートします。

   * `java -jar oak-run*.jar index ...`

## インデックスの統計

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` では、すべてのインデックス定義、重要なインデックス統計およびインデックス内容をオフライン分析用にダンプします。
* インデックス統計の収集は、使用中の AEM インスタンスで安全に実行できます。

## インデックスの整合性チェック

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` では、Lucene Oak インデックスが破損しているかどうかをすばやく判別します。
* 整合性チェックレベル 1 および 2 の場合は、使用中の AEM インスタンスで整合性チェックを安全に実行できます。

## [!DNL oak-run.jar] を使用した TarMK オンラインインデックス作成 {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* [!DNL oak-run.jar] を使用した [!DNL TarMK] のオンラインインデックス作成は、`oak:queryIndexDefinition` ノードで `reindex=true` を設定するよりも高速です。パフォーマンスは向上しますが、[!DNL oak-run.jar] を使用したオンラインインデックス作成には、インデックス作成を実行するためのメンテナンスウィンドウが必要です。

* [!DNL oak-run.jar] を使用した [!DNL TarMK] のオンラインインデックス作成を、AEM インスタンスのメンテナンスウィンドウ外で AEM インスタンスに対して実行することは&#x200B;**できません**。

## oak-run.jar を使用した TarMK オフラインインデックス作成

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* [!DNL oak-run.jar] を使用した [!DNL TarMK] のオフラインインデックス作成は、単一の [!DNL oak-run.jar] コマンドのみ必要なので、[!DNL TarMK] を対象とする [!DNL oak-run.jar] ベースの最もシンプルなインデックス作成アプローチですが、使用するには、AEM インスタンスをシャットダウンする必要があります。

## oak-run.jar を使用した TarMK アウトオブバンドインデックス作成

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* [!DNL oak-run.jar] を使用した [!DNL TarMK] でのアウトオブバンドインデックス作成により、使用中の AEM インスタンスに対するインデックス作成の影響を最小限に抑えることができます。
* アウトオブバンドインデックス作成は、インデックス作成／再作成に要する時間が使用可能なメンテナンスウィンドウを超える場合に、AEM インストールで推奨されるインデックス作成アプローチです。

## oak-run.jar を使用した MongoMK オンラインインデックス作成

* [!DNL MongoMK] や [!DNL RDBMK] で [!DNL oak-run.jar] を使用したオンラインインデックス作成は、[!DNL MongoMK]（および [!DNL RDBMK]）の AEM インストールのインデックス作成／再作成に推奨される方法です。**[!DNL MongoMK] または [!DNL RDBMK] には、それ以外の方法を使用しないでください。**
* このインデックス作成は、クラスター内の単一の AEM インスタンスに対してのみ実行する必要があります。
* [!DNL MongoMK] のオンラインインデックス作成は、リポジトリトラバーサルが 1 つの [!DNL MongoDB] ノードでのみ発生し、その他のノードがパフォーマンスへの重大な影響を受けずにリクエストを処理し続けることができるので、実行中の AEM クラスターに対して安全に実行できます。

[!DNL MongoMK] のオンラインインデックス作成を実行する [!DNL oak-run.jar] インデックスコマンドは、[ [!DNL TarMK] オンラインインデックス作成と同じ（ [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) を使用した場合）ですが、セグメントストアパラメーターがノードストアを含んだ [!DNL MongoDB] インスタンスを指している点が異なります。

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## サポート資料

* [ [!DNL oak-run.jar] のダウンロード](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *上記のように、ダウンロードしたバージョンが、AEM にインストールされている Oak のバージョンと一致していることを確認します*。
* [Apache Jackrabbit Oak oak-run.jar インデックスコマンドのドキュメント](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
