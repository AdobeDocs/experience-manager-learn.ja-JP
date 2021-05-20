---
title: LinuxへのAEM Formsのインストール
description: Linuxのインストールで動作するAEM Forms用の32ビットライブラリをインストールする方法を説明します。
feature: アダプティブフォーム
audience: developer
doc-type: article
activity: setup
version: 6.4, 6.5
topic: 開発
role: Developer
level: Beginner
kt: 7593
source-git-commit: 9583006352ca6a20a763c9d5ec7ba15c3791e897
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---


# 32ビットバージョンの共有ライブラリのインストール

AEM FORMS OSGiまたはAEM Forms j2EEをLinuxにデプロイする場合は、32ビットバージョンの共有ライブラリセットがインストールされ、使用可能になっていることを確認する必要があります。  説明は、パッケージ自体に基づいています。

* expat （James Clarkが記述したXML解析用のストリーム指向XMLパーサCライブラリ）
* fontconfig（システム内のフォントを検索し、アプリケーションで指定された要件に従ってフォントを選択するように設計されたフォント設定およびカスタマイズライブラリ）
* freetype (フォントレンダリングエンジン。多様なプラットフォームや環境に対して高度なフォントサポートを提供するために開発されました。 フォントファイルを開いて管理するだけでなく、個々のグリフを効率的に読み込み、ヒント化、レンダリングすることができます。 フォントサーバーや完全なテキストレンダリングライブラリではありません)。
* glibc （GNUシステムとGNU/Linuxシステム、およびLinuxをカーネルとして使用する他の多くのシステム用のコアライブラリ）
* libcurl（クライアント側URL転送ライブラリ）
* libICE （クライアント間Exchangeライブラリ）
* libicu （堅牢でフル機能のUnicodeとロケールのサポートを提供するライブラリ — Unicode用国際コンポーネント）。 このライブラリの64ビット版と32ビット版の両方が必要です
* libSM（X11セッション管理ライブラリ）
* libuuid（DCE互換のUIライブラリ — ローカルシステム以外からアクセス可能なオブジェクトの一意のIDを生成するために使用）
* libX11（X11クライアント側ライブラリ）
* libXau（X11認証プロトコル — クライアントのアクセスをディスプレイに制限するのに役立つ）
* libxcb （XプロトコルC言語バインディング — XCB）
* libXext （X11プロトコルの共通拡張用ライブラリ）
* libXinerama(X11拡張機能。複数のディスプレイにまたがるデスクトップの拡張をサポートします。 名前は、複数のプロジェクタを使用したワイドスクリーンムービー形式のCinerama上のパンです。 libXineramaは、RandR拡張とインターフェイスするライブラリです)
* libXrandr （Xinerama拡張は、現在はほとんど使用されていません。 RandR拡張に置き換えられています）
* libXrender（Xレンダリング拡張クライアントライブラリ）
nss-softokn-freebl （Network Security Services用のFreeblライブラリ）
* zlib（汎用、特許なし、可逆のデータ圧縮ライブラリ）

Red Hat Enterprise Linux 6以降では、ライブラリの32ビット版にはファイル名拡張子。686が付き、64ビット版には.x86_64が付きます。 例えば、expat.i686のように指定します。 RHEL 6以前の32ビット版では、拡張子は.i386でした。 32ビット版をインストールする前に、最新の64ビット版がインストールされていることを確認してください。 ライブラリの64ビット版がインストールされている32ビット版より古い場合は、次のようなエラーが発生します。

0mError:保護されたマルチライブラリバージョン：libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError:マルチライブラリバージョンの問題が見つかりました。]

## 初回インストール

Red Hat Enterprise Linuxでは、次に示すように、YellowDog Update Modifier(YUM)を使用してインストールします。

1. yum install expat.i686
2. yum install fontconfig.i686
3. yum install freetype.i686
4. yum install glibc.i686
5. yum install libcurl.i686
6. yum install libICE.i686
7. yum install libicu.i686
8. yum install libicu
9. yum install libSM.i686
10. yum install libuuid.i686
11. yum install libX11.i686
12. yum install libXau.i686
13. yum install libxcb.i686
14. yum install libXext.i686
15. yum install libXinerama.i686
16. yum install libXrandr.i686
17. yum install libXrender.i686
18. yum install nss-softokn-freebl.i686
19. yum install zlib.i686

## シンボリックリンク

さらに、libcurl、libcrypto、libssl.soシンボリックリンクを作成し、それぞれ最新の32ビットバージョンのlibcurl、libcrypto、libsslライブラリを参照する必要があります。 ファイルは/usr/lib/にあります
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## 既存システムの更新

更新時にx86_64とi686アーキテクチャの間で次のような競合が発生する可能性があります。
エラー：トランザクションチェックエラー：
glibc-2.28-72.el8.i686のインストール時のファイル/lib/ld-2.28.soは、パッケージglibc32-2.28-42.1.el8.x86_64のファイルと競合する

これに遭遇した場合は、次のように、最初に問題のあるパッケージをアンインストールします。
yum remove glibc32-2.28-42.1.el8.x86_64

これらすべてを実行した場合、x86_64とi686のバージョンは、次の出力からコマンドに出力されるのと同じにする必要があります。
yum info glibc

最後のメタデータの有効期限の確認：0:41:33前2020年1月18日（土） 11:37:08 AM EST。
インストール済みパッケージ
名前：glibc
バージョン：2.28
リリース：72.el8
アーキテクチャ：i686
サイズ：13 M
ソース：glibc-2.28-72.el8.src.rpm
リポジトリ：@System
リポから：BaseOS
概要：GNU libcライブラリ
URL :http://www.gnu.org/software/glibc/
ライセンス：LGPLv2+とLGPLv2+（例外あり）とGPLv2+とGPLv2+（例外あり）、BSD、Inner-Net、ISC、パブリック・ドメイン、GFDL
説明：glibcパッケージには、が使用する標準ライブラリが含まれています。システム上の複数のプログラム。 ディスク領域を節約するために、およびを実行します。メモリと、アップグレードを容易にするために、一般的なシステムコードは次のとおりです。1か所に保存され、プログラム間で共有されます。 このパッケージは次のとおりです。には、最も重要な共有ライブラリのセットが含まれます。標準のC :ライブラリと標準の数学ライブラリ。 これら2つのライブラリがない場合、はになります。Linuxシステムは機能しません。

名前：glibc
バージョン：2.28
リリース：72.el8
アーキテクチャ：x86_64
サイズ：15 M
ソース：glibc-2.28-72.el8.src.rpm
リポジトリ：@System
リポから：BaseOS
概要：GNU libcライブラリ
URL :http://www.gnu.org/software/glibc/
ライセンス：LGPLv2+とLGPLv2+（例外あり）とGPLv2+とGPLv2+（例外あり）、BSD、Inner-Net、ISC、パブリック・ドメイン、GFDL
説明：glibcパッケージには、が使用する標準ライブラリが含まれています。システム上の複数のプログラム。 ディスク領域を節約するために、およびを実行します。メモリと、アップグレードを容易にするために、一般的なシステムコードは次のとおりです。1か所に保存され、プログラム間で共有されます。 このパッケージは次のとおりです。には、最も重要な共有ライブラリのセットが含まれます。標準のC :ライブラリと標準の数学ライブラリ。 これら2つのライブラリがない場合、はになります。Linuxシステムは機能しません。

## 便利なyumコマンド

YUMリストがインストール済み
yum検索[part_of_package_name]
yum whatprovides [package_name]
yum install [package_name]
yum再インストール[package_name]
yum info [package_name]
yumは[package_name]をデプリストします。
yum remove [package_name]
yum check-update [package_name]
yum update [package_name]
