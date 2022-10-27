---
title: Linux へのAEM Formsのインストール
description: Linux のインストールで動作するためにAEM Forms用の 32 ビットライブラリをインストールする方法を説明します。
feature: Adaptive Forms
type: Tutorial
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
kt: 7593
exl-id: b9809561-e9bd-4c67-bc18-5cab3e4aa138
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 0%

---

# 32 ビットバージョンの共有ライブラリのインストール

AEM FORMS OSGi またはAEM Forms j2EE を Linux にデプロイする場合は、32 ビットバージョンの共有ライブラリセットがインストールされ、使用可能になっていることを確認する必要があります。  説明は、パッケージ自体に基づいています。

* expat （James Clark が記述した XML 解析用のストリーム指向 XML パーサー C ライブラリ）
* fontconfig （システム内のフォントを検索し、アプリケーションで指定された要件に従ってフォントを選択するように設計されたフォント設定およびカスタマイズライブラリ）
* freetype ( フォントレンダリングエンジン。各種のプラットフォームおよび環境に対する高度なフォントサポートを提供するために開発されました。 フォントファイルを開いて管理し、個々のグリフを効率的に読み込み、ヒント化し、レンダリングできます。 フォントサーバーや完全なテキストレンダリングライブラリではありません )。
* glibc （GNU システムと GNU/Linux システムのコアライブラリと、Linux をカーネルとして使用する他の多くのシステム）
* libcurl（クライアント側 URL 転送ライブラリ）
* libICE （クライアント間 Exchange ライブラリ）
* libicu（堅牢でフル機能の Unicode とロケールのサポートを提供するライブラリ — Unicode 用国際コンポーネント）。 このライブラリの 64 ビット版と 32 ビット版の両方が必要です
* libSM （X11 セッション管理ライブラリ）
* libuuid（DCE 互換のユニバーサル固有識別子ライブラリ — ローカルシステムを超えてアクセス可能なオブジェクトに対して一意の識別子を生成するために使用）
* libX11 （X11 クライアント側ライブラリ）
* libXau（X11 認証プロトコル — クライアントのアクセスをディスプレイに制限するのに役立つ）
* libxcb （X プロトコル C 言語バインディング — XCB）
* libXext （X11 プロトコルの共通の拡張用ライブラリ）
* libXinerama（X11 拡張）：複数のディスプレイにわたるデスクトップの拡張をサポートします。 名前は、複数のプロジェクタを使用したワイドスクリーンムービー形式の Cinerama のパンです。 libXinerama は、RandR 拡張機能とインターフェイスを取るライブラリです )
* libXrandr （Xinerama 拡張機能は、現在はほとんど古くなっています。RandR 拡張機能に置き換えられています）
* libXrender （X Rendering Extension クライアントライブラリ） nss-softokn-freebl （Network Security Services 用の Freebl ライブラリ）
* zlib（汎用、特許不要、ロスレスデータ圧縮ライブラリ）

Red Hat Enterprise Linux 6 以降では、ライブラリの 32 ビット版のファイル名拡張子は。686 で、64 ビット版のファイル名は.x86_64 です。 例： expat.i686 RHEL 6 以前の 32 ビット版では、拡張子は.i386 でした。 32 ビット版をインストールする前に、最新の 64 ビット版がインストールされていることを確認してください。 ライブラリの 64 ビット版がインストールされている 32 ビット版より古い場合、次のようなエラーが表示されます。

0mError:保護されたマルチライブラリバージョン：libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError:マルチライブラリバージョンの問題が見つかりました。]

## 初回インストール

Red Hat Enterprise Linux では、次に示すように、YellowDog Update Modifier(YUM) を使用してインストールします。

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

## Symlinks

さらに、libcurl.so、libcrypto.so、および libssl.so シンボリックリンクを作成し、それぞれ libcurl、libcrypto、libssl ライブラリの最新 32 ビットバージョンを指す必要があります。 /usr/lib/ ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.soにファイルがあります。

## 既存システムの更新

更新時に、x86_64 と i686 のアーキテクチャの間で次のような競合が発生する可能性があります。エラー：トランザクションチェックエラー：file /lib/ld-2.28.so from glibc-2.28-72.el8.i686 は、パッケージ glibc32-2.28-42.1.el8.x86_64 のファイルと競合する

これに遭遇した場合は、次のように、最初に問題のあるパッケージをアンインストールします。yum remove glibc32-2.28-42.1.el8.x86_64

これで、x86_64 と i686 のバージョンは、例えば次の出力からコマンドに出力されるのと同じにしたいと思います。yum info glibc

最後のメタデータの有効期限の確認：0:41:33 前 2020 年 1 月 18 日 11 日:37:東部標準時の午前 08 時。
インストール済みパッケージ名：glibc のバージョン：2.28 リリース：72.el8 アーキテクチャ：i686 サイズ：13 M ソース：glibc-2.28-72.el8.src.rpm リポジトリ：@System From repo :BaseOS サマリ：GNU libc ライブラリの URL は次の通りです。http://www.gnu.org/software/glibc/ライセンス：LGPLv2+と LGPLv2+（例外あり）と GPLv2+と GPLv2+（例外あり）と BSD と Inner-Net と ISC と Public Domain and GFDL の説明：glibc パッケージには、が使用する標準ライブラリが含まれています。システム上の複数のプログラム。 ディスク容量を節約するには、次の手順を実行します。アップグレードを容易にするだけでなく、メモリに関する一般的なシステムコードは次のとおりです。1 か所に保持され、プログラム間で共有されます。 このパッケージは、次のとおりです。には、共有ライブラリの最も重要なセットが含まれています。標準の C は次のようになります。ライブラリと標準数学ライブラリ。 この 2 つのライブラリがない場合、はになります。Linux システムは機能しません。

名前：glibc のバージョン：2.28 リリース：72.el8 アーキテクチャ：x86_64 サイズ：15 M ソース：glibc-2.28-72.el8.src.rpm リポジトリ：@System From repo :BaseOS サマリ：GNU libc ライブラリの URL は次の通りです。http://www.gnu.org/software/glibc/ライセンス：LGPLv2+と LGPLv2+（例外あり）と GPLv2+と GPLv2+（例外あり）と BSD と Inner-Net と ISC と Public Domain and GFDL の説明：glibc パッケージには、が使用する標準ライブラリが含まれています。システム上の複数のプログラム。 ディスク容量を節約するには、次の手順を実行します。アップグレードを容易にするだけでなく、メモリに関する一般的なシステムコードは次のとおりです。1 か所に保持され、プログラム間で共有されます。 このパッケージは、次のとおりです。には、共有ライブラリの最も重要なセットが含まれています。標準の C は次のようになります。ライブラリと標準数学ライブラリ。 この 2 つのライブラリがない場合、はになります。Linux システムは機能しません。

## 便利な yum コマンド

yum リストがインストールされた yum 検索 [part_of_package_name]
何を提供しているか [package_name]
yum install [package_name]
yum 再インストール [package_name]
yum 情報 [package_name]
yum deplist [package_name]
yum の削除 [package_name]
yum check-update [package_name]
yum 更新 [package_name]
