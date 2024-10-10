---
title: Linux での AEM Forms のインストール
description: Linux のインストールで動作するために AEM Forms 用の 32 ビットライブラリをインストールする方法を説明します。
feature: Adaptive Forms
type: Tutorial
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-7593
exl-id: b9809561-e9bd-4c67-bc18-5cab3e4aa138
last-substantial-update: 2019-06-09T00:00:00Z
duration: 204
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '938'
ht-degree: 100%

---

# 32 ビットバージョンの共有ライブラリのインストール

AEM Forms OSGi または AEM Forms j2EE を Linux にデプロイする場合は、32 ビットバージョンの共有ライブラリセットがインストールされ、使用可能になっていることを確認する必要があります。  説明はパッケージのものです。

* expat（James Clark が記述した XML 解析用のストリーム指向 XML パーサー C ライブラリ）
* fontconfig（システム内のフォントを特定し、アプリケーションで指定された要件に応じてフォントを選択するように設計されたフォント設定およびカスタマイズのライブラリ）
* freetype（フォントレンダリングエンジン。各種のプラットフォームおよび環境に対する高度なフォントサポートを提供するために開発されました。 フォントファイルを開いて管理し、個々のグリフを効率的に読み込み、ヒント化し、レンダリングできます。 フォントサーバーや完全なテキストレンダリングライブラリではありません）
* glibc（GNU システムと GNU／Linux システムのコアライブラリと、Linux をカーネルとして使用する他の多くのシステム）
* libcurl（クライアントサイド URL 転送ライブラリ）
* libICE（クライアント間 Exchange ライブラリ）
* libicu（堅牢でフル機能の Unicode とロケールのサポートを提供するライブラリ（Unicode 用国際コンポーネント）） 。このライブラリの 64 ビット版と 32 ビット版の両方が必要です
* libSM（X11 セッション管理ライブラリ）
* libuuid（DCE 互換のユニバーサル固有識別子ライブラリ（UUID）- ローカルシステムを超えてアクセス可能なオブジェクトに対して一意の識別子を生成するために使用）
* libX11（X11 クライアントサイドライブラリ）
* libXau（X11 認証プロトコル - クライアントのアクセスをディスプレイに制限するのに有用）
* libxcb（X プロトコル C 言語バインディング - XCB）
* libXext（X11 プロトコルの共通の拡張用ライブラリ）
* libXinerama（複数のディスプレイ全体でデスクトップの拡張をサポートする X11 拡張。名前は、複数のプロジェクタを使用したワイドスクリーンムービー形式の Cinerama のパンです。libXinerama は、RandR 拡張機能とインターフェイスを取るライブラリです）
* libXrandr（Xinerama 拡張機能は現在ほとんどが古くなり、RandR 拡張機能に置き換えられています）
* libXrender（X レンダリング拡張機能クライアントライブラリ）
nss-softokn-freebl（Network Security Services 用の Freebl ライブラリ）
* zlib（汎用、特許なし、ロスレスデータ圧縮ライブラリ）

Red Hat Enterprise Linux 6 以降では、ライブラリの 32 ビット版のファイル名拡張子は .686 で、64 ビット版のファイル名拡張子は .x86_64 になります（例：expat.i686）。RHEL 6 より前の 32 ビット版では、拡張子は .i386 でした。32 ビット版をインストールする前に、最新の 64 ビット版がインストールされていることを確認してください。ライブラリの 64 ビット版が、インストールされている 32 ビット版より古い場合は、次のようなエラーが表示されます。

0mError: Protected multilib versions: libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError: Multilib version problems found.]

## 初回インストール

次のように、Red Hat Enterprise Linux では YellowDog Update Modifier（YUM）を使用してインストールします。

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

さらに、libcurl.so、libcrypto.so および libssl.so シンボリックリンクを作成し、それぞれが libcurl、libcrypto および libssl ライブラリの最新の 32 ビットバージョンを指すように設定する必要があります。これらのファイルは /usr/lib/ にあります。シンボリックリンクの設定方法は次のとおりです。
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## 既存のシステムの更新

更新時に、x86_64 と i686 のアーキテクチャの間で次のような競合が発生する可能性があります。
Error: Transaction check error:
file /lib/ld-2.28.so from install of glibc-2.28-72.el8.i686 conflicts with file from package glibc32-2.28-42.1.el8.x86_64（エラー：トランザクションチェックエラー：インストールされている glibc-2.28-72.el8.i686 内のファイル /lib/ld-2.28.so がパッケージ glibc32-2.28-42.1.el8.x86_64 内のファイルと競合しています）

このような状況に遭遇した場合は、問題のあるパッケージをまずアンインストールします。この例では次のようにします。
yum remove glibc32-2.28-42.1.el8.x86_64

つまり、x86_64 と i686 のバージョンを正確に同じにします（例えば、次のコマンドの出力を参照してください）。
yum info glibc

直近のメタデータの有効期限の確認：0:41:33 日前 2020年1月18日 土 11:37:08 AM EST。
インストール済みパッケージ
名前：glibc
バージョン：2.28
リリース：72.el8
アーキテクチャ： i686
サイズ：13 M
ソース：glibc-2.28-72.el8.src.rpm
リポジトリ：システム内
元リポジトリ：ベース OS
概要：GNU libc ライブラリ
URL：http://www.gnu.org/software/glibc/
ライセンス：LGPLv2+ および LGPLv2+（例外あり）、GPLv2+ および GPLv2+（例外あり）、BSD、Inner-Net、ISC、Public Domain、GFDL
説明：glibc パッケージには、システム上の複数のプログラムが使用する標準ライブラリが含まれています。ディスク容量とメモリを節約しアップグレードを容易にするために、共通のシステムコードが一元化され、プログラム間で共有されます。このパッケージには、最も重要な共有ライブラリセットである標準 C ライブラリと標準数学ライブラリが含まれています。この 2 つのライブラリがないと。Linux システムは機能しません。

名前：glibc
バージョン：2.28
リリース：72.el8
アーキテクチャ：x86_64
サイズ：15 M
ソース：glibc-2.28-72.el8.src.rpm
リポジトリ：システム内
元レポジトリ：ベース OS
概要：GNU libc ライブラリ
URL：http://www.gnu.org/software/glibc/
ライセンス：LGPLv2+ and LGPLv2+（例外あり）、GPLv2+、GPLv2+（例外あり）、BSD、Inner-Net、ISC、Public Domain、GFDL
説明：glibc パッケージには、システム上の複数のプログラムが使用する標準ライブラリが含まれています。ディスク容量とメモリを節約しアップグレードを容易にするために、共通のシステムコードが一元化され、プログラム間で共有されます。このパッケージには、最も重要な共有ライブラリセットである標準 C ライブラリと標準数学ライブラリが含まれています。この 2 つのライブラリがないと。Linux システムは機能しません。

## 便利な yum コマンド

yum list installed
yum search [part_of_package_name]
yum whatprovides [package_name]
yum install [package_name]
yum reinstall [package_name]
yum info [package_name]
yum deplist [package_name]
yum remove [package_name]
yum check-update [package_name]
yum update [package_name]
