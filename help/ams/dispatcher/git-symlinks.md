---
title: GIT への Symlinks の適切な追加
description: Dispatcher 設定で作業する際のシンボリックリンクの追加方法と追加場所に関する説明です。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: df3afc60f765c18915eca3bb2d3556379383fafc
workflow-type: tm+mt
source-wordcount: '1252'
ht-degree: 1%

---


# GIT への Symlinks の追加

[目次](./overview.md)

[&lt; — 前：Dispatcher ヘルスチェック](./health-check.md)

AMS には、Dispatcher のソースコードが格納された、事前に設定された GIT リポジトリが用意され、開発とカスタマイズを開始する準備が整います。

最初の `.vhost` ファイルまたは最上位レベル `farm.any` ファイルを作成する場合は、 `available_*` ディレクトリの `enabled_*` ディレクトリ。 Cloud Manager パイプラインを通じて適切なリンクタイプを使用することが、デプロイメントを成功させるうえで重要になります。 このページでは、この方法を説明します。

## Dispatcher アーキタイプ

AEM開発者は、通常、プロジェクトを [AEM archetype](https://github.com/adobe/aem-project-archetype)

使用されているシンボリックリンクが表示されるソースコードの領域の例を以下に示します。

```
$ tree dispatcher
dispatcher
└── src
   ├── conf.d
.....SNIP.....
    │   └── available_vhosts
    │   │   ├── 000_unhealthy_author.vhost
    │   │   ├── 000_unhealthy_publish.vhost
    │   │   ├── aem_author.vhost
    │   │   ├── aem_flush.vhost
    │   │   ├── aem_health.vhost
    │   │   ├── aem_lc.vhost
    │   │   └── aem_publish.vhost
    └── dispatcher_vhost.conf
    │   └── enabled_vhosts
    │   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
    │   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
    │   │   ├── aem_health.vhost -> ../available_vhosts/aem_health.vhost
    │   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
.....SNIP.....
    └── conf.dispatcher.d
    │   ├── available_farms
    │   │   ├── 000_ams_author_farm.any
    │   │   ├── 001_ams_lc_farm.any
    │   │   └── 002_ams_publish_farm.any
.....SNIP.....
    │   └── enabled_farms
    │   │   ├── 000_ams_author_farm.any -> ../available_farms/000_ams_author_farm.any
    │   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
.....SNIP.....
17 directories, 60 files
```

例えば、 `/etc/httpd/conf.d/available_vhosts/` ディレクトリにステージ済みの潜在的な情報が含まれています `.vhost` 実行設定で使用できるファイル。

有効 `.vhost` ファイルは相対パスとして表示されます `symlinks` 内側 `/etc/httpd/conf.d/enabled_vhosts/` ディレクトリ。

## symlink の作成

Apache Web サーバーが宛先ファイルを同じファイルとして扱えるように、ファイルへのシンボリックリンクを使用します。  両方のディレクトリでファイルを複製しないようにします。  一方のディレクトリ（シンボリックリンク）からもう一方のディレクトリへのショートカットの代わりに、

デプロイした設定が Linux ホストをターゲットにすることを認識します。  ターゲット・システムと互換性のない symlink を作成すると、障害が発生し、望ましくない結果が生じます。

ワークステーションが Linux マシンでない場合は、リンクを正しく作成して GIT にコミットできるように、どのコマンドを使用すればよいのか疑問に思うかもしれません。

> `TIP:` Apache Web サーバーのローカルコピーをインストールし、異なるインストールベースを持っていた場合でも、リンクは引き続き機能するので、相対リンクを使用することが重要です。  絶対パスを使用する場合、ワークステーションや他のシステムは、同じディレクトリ構造と一致する必要があります。

### OSX/Linux

symlinks はこれらのオペレーティングシステムに固有のものであり、これらのリンクの作成方法の例を以下に示します。  お気に入りのターミナルアプリケーションを開き、次のサンプルコマンドを使用してリンクを作成します。

```
$ cd <LOCATION OF CLONED GIT REPO>\src\conf.d\enabled_vhosts
$ ln -s ../available_vhosts/<Destination File Name> <Target File Name>
```

次に、参照用の入力済みコマンドの例を示します。

```
$ git clone https://github.com/adobe/aem-project-archetype.git
$ cd aem-project-archetype/src/main/archetype/dispatcher.ams/src/conf.d/enabled_vhosts/
$ ln -s ../available_vhosts/aem_flush.vhost aem_flush.vhost
```

次に、 `ls` コマンド：

```
ls -l
total 0
lrwxrwxrwx. 1 root root 35 Oct 13 21:38 aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
```

### Windows

> `Note:` MS Windows （より良い、NTFS）はシンボリックリンクをサポートしています。Windows Vista!

![mklink コマンドのヘルプ出力を示す Windows コマンドプロンプトの画像](./assets/git-symlinks/windows-terminal-mklink.png)

> `Warning:` mklink コマンドを使用して symlink を作成するには、管理者権限が必要です。 管理者アカウントの場合でも、開発者モードが有効になっていない限り、コマンドプロンプト「管理者として」を実行する必要があります
> <br/>不適切な権限：
> ![アクセス許可が原因でコマンドが失敗したことを示す Windows コマンドプロンプトの画像](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>適切な権限：
> ![管理者として実行された Windows コマンドプロンプトの画像](./assets/git-symlinks/windows-mklink-properpriv.png)

次に、リンクを作成するコマンドを示します。

```
C:\<PATH TO SRC>\enabled_vhosts> mklink <Target File Name> ..\available_vhosts\<Destination File Name>
```


次に、参照用の入力済みコマンドの例を示します。

```
C:\> git clone https://github.com/adobe/aem-project-archetype.git
C:\> cd aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts\
C:\aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts> mklink aem_flush.vhost ..\available_vhost\aem_flush.vhost
symbolic link created for aem_flush.vhost <<===>> ..\available_vhosts\aem_flush.vhost
```

#### 開発者モード ( Windows 10 )

を [開発者モード](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development)Windows 10 を使用すると、開発中のアプリをより簡単にテストし、Ubuntu Bash シェル環境を使用し、開発者に焦点を当てた様々な設定を変更し、その他の操作を行うことができます。

Microsoftは、開発者モードに機能を追加し続けているようです。また、より広範に採用され、安定したと見なされた場合、デフォルトでこれらの機能の一部を有効にしています（例えば、Creators Update では、Ubuntu Bash Shell 環境は、開発者モードを必要としません）。

symlinks はどうですか？ 開発者モードを有効にすると、管理者権限でコマンドプロンプトを実行してシンボリックリンクを作成する必要がなくなります。 したがって、開発者モードを有効にすると、誰でもシンボリックリンクを作成できます。

> デベロッパーモードを有効にした後、ユーザーは、変更を有効にするにはログオフ/ログオンする必要があります。

これで、管理者としてを実行せずに、コマンドが機能するようになりました

![開発者モードを有効にして、通常のユーザーとして実行する Windows コマンドプロンプトの画像](./assets/git-symlinks/windows-mklink-devmode.png)

#### 代替/プログラム的アプローチ

特定のユーザーがシンボリックリンクを作成できるようにする特定のポリシーがあります→ [シンボリックリンクの作成 (Windows 10) - Windows セキュリティ | Microsoftドキュメント](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

PRO:
- これは、各デバイスで手動で開発者モードを有効にする必要なく、組織内のすべての開発者 (Active Directory) に対するシンボリックリンクの作成をプログラムで許可する場合に利用できます。
- さらに、このポリシーは、デベロッパーモードを提供しない以前のバージョンの MS Windows で利用できるはずです。

CON:
- このポリシーは、Administrators グループに属するユーザーには影響を与えないようです。 管理者は、管理者権限を持つコマンドプロンプトを実行する必要があります。 変だ。

> ローカル/グループポリシーの変更を有効にするには、ユーザーのログオフ/ログオンが必要です。

実行 `gpedit.msc`、必要に応じてユーザーを追加または変更します。 管理者はデフォルトで存在します

![調整する権限を示す [ グループポリシーエディタ ] ウィンドウ](./assets/git-symlinks/windows-localpolicies-symlinks.png)

#### GIT での Symlinks の有効化

Git は、 core.symlinks オプションに従ってシンボリックリンクを処理します

ソース： [Git - git-config ドキュメント](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*core.symlinks が false の場合、シンボリックリンクは、リンクテキストを含む小さなプレーンファイルとしてチェックアウトされます。 `git-update-index[1]` および `git-add[1]` は、記録されたタイプを通常のファイルに変更しません。 シンボリックリンクをサポートしない FAT などのファイルシステムで役立ちます。
デフォルトは true です（例外は）。 `git-clone[1]` または `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.` ほとんどの場合、Git では Windows が symlinks に対して適切でないと見なされ、これが false に設定されます。*

Windows での Git の動作については、次の節で詳しく説明します。シンボリックリンク・ git-for-windows/git Wiki ・ GitHub

> `Info`:上記のドキュメントに記載されている前提は、Windows 上でのAEM Developer の設定が可能で、特に NTFS に関するもので、また、symlinks と Directory symlinks のみを使用しているという点では、問題ないと思われます。

これが良い知らせだ [Windows 版 Git バージョン2.10.2](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1) インストーラーに [シンボリックリンクのサポートを有効にする明示的なオプション。](https://github.com/git-for-windows/git/issues/921)

> `Warning`:core.symlink オプションは、リポジトリのクローン作成時に実行時に提供できます。それ以外の場合は、グローバル設定として保存できます。

![GIT インストーラーが symlinks のサポートを表示しています](./assets/git-symlinks/windows-git-install-symlink.png)

Git for Windows は、グローバル環境設定を `"C:\Program Files\Git\etc\gitconfig"` . これらの設定は、他の Git デスクトップクライアントアプリでは考慮されない場合があります。
以下は、すべての開発者が Git ネイティブクライアント (Git Cmd、Git Bash) を使用するわけではなく、一部の Git デスクトップアプリ（GitHub Desktop、Atlassian Sourcetree など）では、異なる設定/デフォルトで System または組み込み Git を使用する場合の問題です

以下は、の内部にあるものの例です。 `gitconfig` ファイル

```
[diff "astextplain"]
    textconv = astextplain
[filter "lfs"]
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge -- %f
    process = git-lfs filter-process
    required = true
[http]
    sslBackend = openssl
    sslCAInfo = C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
[core]
    autocrlf = true
    fscache = true
    symlinks = true
[pull]
    rebase = false
[credential]
    helper = manager-core
[credential "https://dev.azure.com"]
    useHttpPath = true
[init]
    defaultBranch = master
```

#### Git コマンドラインに関するヒント

新しいシンボリックリンクを作成する必要がある場合があります（例：新しいホストや新しいファームの追加）。

上記のドキュメントでは、Windows がシンボリックリンクを作成するための「mklink」コマンドを提供しています。

Git Bash 環境で作業する場合は、代わりに標準の Bash コマンドを使用できます `ln -s` ただし、次の例のような特別な命令が先頭に付く必要があります。

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### 概要

Microsoft Windows OS で ( 少なくとも現在のAEM Dispatcher 設定ベースラインの範囲に対して )Git でシンボリックリンクを正しく処理するには、次が必要です。

| 項目 | 最小バージョン/設定 | 推奨バージョン/設定 |
|------|---------------------------------|-------------------------------------|
| オペレーティングシステム | Windows Vista 以降 | Windows 10 Creator Update 以降 |
| ファイルシステム | NTFS | NTFS |
| Windows ユーザーのシンボリックリンクを処理する機能 | `"Create symbolic links"` グループ/ローカルポリシー `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | Windows 10 開発者モードが有効になっています |
| GIT | ネイティブクライアントバージョン 1.5.3 | ネイティブクライアントバージョン2.10.2以降 |
| Git 設定 | `--core.symlinks=true` オプションを使用して、コマンドラインから git クローンを作成する | Git のグローバル設定<br/>`[core]`<br/>    symlinks = true <br/> ネイティブ Git クライアント設定パス： `C:\Program Files\Git\etc\gitconfig` <br/>Git デスクトップクライアントの標準的な場所： `%HOMEPATH%\.gitconfig` |

> `Note:` 既にローカルリポジトリがある場合は、元の場所から新しくクローンを作成する必要があります。 新しい場所にクローンを作成し、コミットされていないローカル変更やステージされていないローカル変更を新しくクローンされたリポジトリに手動でマージできます。