---
title: Tomcatのインストールと設定
seo-title: Tomcatのインストールと設定
description: これは、最初の対話型通信ドキュメントを作成するためのマルチステップチュートリアルのパート1です。このパートでは、TOMCATをインストールし、sampleRest.warファイルをTOMCATにデプロイします。 このWARファイルで公開されるRESTエンドポイントは、データソースとフォームデータモデルの基本になります。
seo-description: これは、最初の対話型通信ドキュメントを作成するためのマルチステップチュートリアルのパート1です。このパートでは、TOMCATをインストールし、sampleRest.warファイルをTOMCATにデプロイします。 このWARファイルで公開されるRESTエンドポイントは、データソースとフォームデータモデルの基本になります。
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
topic: 開発
role: デベロッパー
level: 初心者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 1%

---


# Tomcatのインストールと設定{#install-and-configure-tomcat}

この部分では、TOMCATをインストールし、TOMCATにsampleRest.warファイルをデプロイします。 このWARファイルで公開されるRESTエンドポイントは、データソースとフォームデータモデルの基本になります。

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

tomcatを設定するには、次の手順に従います。

1. JDK1.8をダウンロードしてインストールします。
2. JAVA_HOMEをJDK1.8を指すように設定します。
3. [tomcat](https://tomcat.apache.org/)をダウンロードします。 このwarファイルは、Tomcatバージョン8.5.xおよび9.0.xでテストされています。
4. 環境設定のtomcatバージョンをダウンロードします。 64ビット版のwindows zipはコアセクションからダウンロードできます。
5. 内容をc:\tomcatフォルダーに解凍します。
6. tomcatのバージョンに応じて、cドライブ&#x200B;**c:\tomcat\apache-tomcat-8.5.27**&#x200B;に次のようなものが表示されます。
7. &quot;CATALINA_HOME&quot;という名前の環境変数を作成し、その値をtomcatのインストールフォルダ例c:\tomcat\apache- tomcat-8.5.27に設定します。
8. TomcatインストールのwebappsフォルダーにSampleRest.warファイルをコピーします。
9. 開始の新しいコマンドプロンプトウィンドウ
10. &lt;tomcat install folder>\binに移動し、startup.batを実行します。
11. tomcatを起動したら、[ここをクリックして](http://localhost:8080/SampleRest/webapi/getStatement/9586)、WARファイルで公開されたエンドポイントをテストします
12. この呼び出しの結果として、サンプルデータを取得する必要があります。

これで完了です !!!. tomcatを設定し、SampleRest.warファイルをデプロイした。

次のビデオでは、Tomcatでのサンプルアプリケーションのデプロイメントについて説明します
>[!VIDEO](https://video.tv.adobe.com/v/37815)