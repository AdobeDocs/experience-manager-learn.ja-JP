---
title: LinuxにAEM Formsをインストールする。
description: 32ビット版のライブラリをインストールし、Linuxのインストールで動作するようにAEM Forms向け。
feature: アダプティブフォーム
audience: developer
doc-type: article
activity: setup
version: 6.4, 6.5
topic: 開発
role: Developer
level: Beginner
kt: 7593
translation-type: tm+mt
source-git-commit: da7837d45a9d5f614a4f6527b7bfe98aaf980d4f
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 0%

---


# 32ビット版の共有ライブラリのインストール

AEM FORMSOSGiまたはAEM Formsj2EEをLinuxにデプロイする場合は、32ビット版の共有ライブラリがインストールされ、使用可能になっていることを確認する必要があります。  説明はパッケージ自体に記載されています。

* expat （XMLを解析するためのストリーム指向XMLパーサーCライブラリ、James Clarkによって記述）
* fontconfig（システム内でフォントを検索し、アプリケーションが指定する要件に従ってフォントを選択するように設計されたフォント設定およびカスタマイズライブラリ）
* freetype(様々なプラットフォームおよび環境に対して高度なフォントサポートを提供するために開発されたフォントレンダリングエンジン。 フォントファイルを開いて管理したり、個々のグリフの読み込み、ヒント化、レンダリングを効率的に行ったりできます。 フォントサーバーでも、完全なテキストレンダリングライブラリでもありません)。
* glibc （GNUシステムとGNU/Linuxシステムのコアライブラリと、Linuxをカーネルとして使う他の多くのシステム）
* libcurl（クライアント側URL転送ライブラリ）
* libICE （クライアント間Exchangeライブラリ）
* libicu（堅牢でフル機能のUnicodeとロケールをサポートするライブラリ — Unicode用の国際コンポーネント）。 このライブラリの64ビット版と32ビット版の両方が必要です
* libSM （X11 Session Managementライブラリ）
* libuuid (DCE互換のUniversally Unique Identifierライブラリ — ローカルシステム以外からアクセス可能なオブジェクトの一意の識別子を生成するために使用)
* libX11 （X11クライアント側ライブラリ）
* libXau （X11認証プロトコル — クライアントのアクセスをディスプレイに制限するのに役立つ）
* libxcb （XプロトコルC言語バインディング — XCB）
* libXext （X11プロトコルの一般的な拡張のためのライブラリ）
* libXinerama(X11の拡張機能。複数のディスプレイにまたがるデスクトップの拡張をサポート。 名前は、複数のプロジェクタを使用したワイドスクリーンムービー形式のCineramaでパンです。 libXineramaはRandR拡張子とインタフェースするライブラリです)。
* libXrandr （Xineramaの拡張は現在ではほとんど廃止されており、RandRの拡張に置き換えられています）
* libXrender （X Rendering Extensionクライアントライブラリ）
nss-softokn-freebl （Network Security Services用フリーブルライブラリ）
* zlib （汎用、特許不要、可逆のデータ圧縮ライブラリ）

Red Hat Enterprise Linux 6以降では、32ビット版のライブラリはファイル名拡張子。686を持ち、64ビット版は.x86_64を持ちます。 例えば、expat.i686. RHEL 6より前の32ビット版では、拡張子.i386が付いていました。 32ビット版をインストールする前に、最新の64ビット版がインストールされていることを確認してください。 64ビット版のライブラリが、インストールされている32ビット版より古い場合は、次のようなエラーが表示されます。

0mエラー：保護されたマルチライブラリバージョン：libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError:マルチライブラリバージョンの問題が見つかりました。]

## 初回インストール

Red Hat Enterprise Linuxでは、以下に示すように、YellowDog Update Modifier(YUM)を使用してインストールします。

1. yum install expat.i686
2. yum install fontconfig.i686
3. yum install freetype.i686
4. yum install glibc.i686
5. yum install libcurl.i686
6. yum install libICE.i686
7. yum install libicu.i686
8. yum install libic
9. yum install libSM.i686
10. yum install libuuid.i686
11. yum install libX11.i686
12. yumインストールlibXau.i686
13. yum install libxcb.i686
14. yum install libXext.i686
15. yum install libXinerama.i686
16. yum install libXrandr.i686
17. yum install libXrender.i686
18. yum install nss-softokn-freebl.i686
19. yum install zlib.i686

## Symlinks

さらに、libcurl.so、libcrypto.so、libssl.soの各シンボリックリンクを作成し、それぞれlibcurl、libcrypto、libsslの最新32ビット版を参照する必要があります。 ファイルは/usr/lib/にあります
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## 既存のシステムの更新

次のように、アップデート中にx86_64とi686のアーキテクチャの間で競合が発生する可能性があります。
エラー：トランザクションチェックエラー：
glibc-2.28-72.el8.i686のインストール時の/lib/ld-2.28.soファイルは、パッケージglibc32-2.28-42.1.el8.x86_64のファイルと競合しています。

この状況が発生した場合は、この場合のように、最初に、問題のあるパッケージをアンインストールします。
yum remove glibc32-2.28-42.1.el8.x86_64

これらは全て同じで、x86_64とi686のバージョンは、例えば次の出力からコマンドに渡されるのと同じにする必要があります。
yum info glibc

最後のメタデータの有効期限の確認：0:41:33前（米国東部標準時の2020年1月18日午前11:37:08）
インストール済みパッケージ
名前：glibc
バージョン：2.28
リリース：72.el8
アーキテクチャ：i686
サイズ：13 M
ソース：glibc-2.28-72.el8.src.rpm
リポジトリ：@System
レポから：BaseOS
概要：GNU libcライブラリ
URL :http://www.gnu.org/software/glibc/
ライセンス：例外を持つLGPLv2+とLGPLv2+、例外を持つGPLv2+とGPLv2+、BSD、Inner-Net、ISC、パブリックドメインとGFDL
説明：glibcパッケージには標準ライブラリが含まれており、これらはで使用される。複数のプログラムが存在する。 ディスク領域を節約するため、および：メモリは、アップグレードを容易にするだけでなく、一般的なシステムコードを次のようにします。1か所に保持され、プログラム間で共有されます。 このパッケージは次のとおりです。には、最も重要な共有ライブラリのセットが含まれます。標準C:ライブラリと標準の数学ライブラリ。 この2つのライブラリがない場合、は、Linuxシステムは機能しません。

名前：glibc
バージョン：2.28
リリース：72.el8
アーキテクチャ：x86_64
サイズ：15 M
ソース：glibc-2.28-72.el8.src.rpm
リポジトリ：@System
レポから：BaseOS
概要：GNU libcライブラリ
URL :http://www.gnu.org/software/glibc/
ライセンス：例外を持つLGPLv2+とLGPLv2+、例外を持つGPLv2+とGPLv2+、BSD、Inner-Net、ISC、パブリックドメインとGFDL
説明：glibcパッケージには標準ライブラリが含まれており、これらはで使用される。複数のプログラムが存在する。 ディスク領域を節約するため、および：メモリは、アップグレードを容易にするだけでなく、一般的なシステムコードを次のようにします。1か所に保持され、プログラム間で共有されます。 このパッケージは次のとおりです。には、最も重要な共有ライブラリのセットが含まれます。標準C:ライブラリと標準の数学ライブラリ。 この2つのライブラリがない場合、は、Linuxシステムは機能しません。

## 便利なyumコマンド

yumリストがインストールされています
yum search [part_of_package_name]
[package_name]を提供するyum what
yum install [package_name]
yum reinstall [package_name]
yum info [package_name]
yum deplist [package_name]
yum remove [package_name]
yum check-update [package_name]
yum update [package_name]
