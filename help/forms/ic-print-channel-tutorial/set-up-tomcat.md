---
title: Tomcat のインストールと設定ビデオ
seo-title: Install and Configure Tomcat
description: これは、最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの第 1 部です。
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
topic: Development
role: Developer
level: Beginner
exl-id: faa9ca2d-6cfa-4abf-be5e-3e549202853a
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 0%

---

# Tomcat のインストールと設定 {#install-and-configure-tomcat}

ここでは、TOMCAT をインストールし、TOMCAT に sampleRest.war ファイルをデプロイします。 この WAR ファイルで公開される REST エンドポイントは、データソースとフォームデータモデルの基礎となります。

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

Tomcat を設定するには、次の手順に従います。

1. JDK1.8 をダウンロードしてインストールします。
2. JAVA_HOME を JDK1.8 を指すように設定します。
3. ダウンロード [tomcat](https://tomcat.apache.org/). この war ファイルは、Tomcat バージョン 8.5.x および 9.0.x でテストされています。
4. 好みの tomcat バージョンをダウンロードします。 64 ビット版の windows zip は、コアセクションでダウンロードできます。
5. コンテンツをc:\tomcatに解凍します。
6. c ドライブには次のようなものが表示されます。 **c:\tomcat\apache-tomcat-8.5.27** tomcat のバージョンに応じて
7. 「CATALINA_HOME」という環境変数を作成し、その値を tomcat のインストールフォルダの例c:\tomcat\apache- tomcat-8.5.27 に設定します。
8. Tomcat インストールの webapps フォルダーに SampleRest.war ファイルをコピーします。
9. 新しいコマンドプロンプトウィンドウを起動します。
10. に移動します。 &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin をクリックし、startup.bat を実行します。
11. Tomcat が起動したら、次の方法で WAR ファイルによって公開されたエンドポイントをテストします。 [ここをクリック](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. この呼び出しの結果、サンプルデータが取得されるはずです。

おめでとう!!!。 tomcat を設定し、SampleRest.war ファイルをデプロイしました。

次のビデオでは、Tomcat でのサンプルアプリケーションのデプロイメントについて説明します
>[!VIDEO](https://video.tv.adobe.com/v/37815)
