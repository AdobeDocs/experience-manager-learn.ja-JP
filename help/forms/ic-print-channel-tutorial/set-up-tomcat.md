---
title: Tomcatのインストールと設定
seo-title: Tomcatのインストールと設定
description: これは、最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの第1部です。この部分では、TOMCATをインストールし、TOMCATにsampleRest.warファイルをデプロイします。 このWARファイルで公開されるRESTエンドポイントは、データソースとフォームデータモデルの基礎となります。
seo-description: これは、最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの第1部です。この部分では、TOMCATをインストールし、TOMCATにsampleRest.warファイルをデプロイします。 このWARファイルで公開されるRESTエンドポイントは、データソースとフォームデータモデルの基礎となります。
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
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 1%

---


# Tomcat {#install-and-configure-tomcat}のインストールと設定

このパートでは、TOMCATをインストールし、TOMCATにsampleRest.warファイルをデプロイします。 このWARファイルで公開されるRESTエンドポイントは、データソースとフォームデータモデルの基礎となります。

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

Tomcatを設定するには、次の手順に従います。

1. JDK1.8をダウンロードしてインストールします。
2. JAVA_HOMEをJDK1.8を指すように設定します。
3. [tomcat](https://tomcat.apache.org/)をダウンロードします。 このwarファイルは、Tomcatバージョン8.5.xおよび9.0.xでテストされています。
4. 好みのTomcatバージョンをダウンロードします。 64ビットのwindows zipはコアセクションでダウンロードできます。
5. c:\tomcatフォルダーにコンテンツを解凍します。
6. Tomcatのバージョンに応じて、cドライブ&#x200B;**c:\tomcat\apache-tomcat-8.5.27**&#x200B;に次のような内容が表示されます。
7. 「CATALINA_HOME」という環境変数を作成し、その値をtomcatインストールフォルダの例c:\tomcat\apache- tomcat-8.5.27に設定します。
8. Tomcatインストール環境のwebappsフォルダーにSampleRest.warファイルをコピーします。
9. 新しいコマンドプロンプトウィンドウを起動します。
10. &lt;tomcat install folder>\binに移動し、 startup.batを実行します。
11. Tomcatが起動したら、[ここ](http://localhost:8080/SampleRest/webapi/getStatement/9586)をクリックして、WARファイルで公開されたエンドポイントをテストします。
12. この呼び出しの結果として、サンプルデータが取得されます。

これで完了です !!!. tomcatを設定し、SampleRest.warファイルをデプロイしました。

次のビデオでは、Tomcatでのサンプルアプリケーションのデプロイメントを説明します
>[!VIDEO](https://video.tv.adobe.com/v/37815)