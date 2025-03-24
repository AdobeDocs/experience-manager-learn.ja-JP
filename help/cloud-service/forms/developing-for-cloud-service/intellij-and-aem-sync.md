---
title: リポジトリツールを使用した IntelliJ の設定
description: AEM クラウド対応インスタンスと同期するための IntelliJ の準備
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8844
exl-id: 9a7ed792-ca0d-458f-b8dd-9129aba37df6
duration: 92
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 100%

---

# Cygwin のインストール


Cygwin は、Microsoft Windows 上でネイティブに動作する POSIX 互換のプログラミングおよびランタイム環境です。
[Cygwin](https://www.cygwin.com/) をインストールします。C:\cygwin64 フォルダーにインストールしました。
>[!NOTE]
> cygwin のインストール時に zip、unzip、curl、rsync パッケージするようにしてください。

c:\cloudmanager の下に adoberepo というフォルダーを作成します。

[リポジトリツールのインストール](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo) リポジトリツールのインストールは、リポジトリファイルをコピーしてc:\cloudmanger\adoberepo folderフォルダーに配置するだけです。

パス環境変数 C:\cygwin64\bin、C:\CloudManager\adoberepo に以下を追加します。

## 外部ツールの設定

* IntelliJ の起動
* Ctrl + Alt + S キーを押して、設定ウィンドウを起動します。
* ツール/外部ツールを選択して「+」記号をクリックし、スクリーンショットのように入力します。
  ![rep](assets/repo.png)
* 必ず、「グループ」ドロップダウンフィールドに「repo」と入力して、repo という名前のグループを作成し、作成するすべてのコマンドが **repo** グループに属するようにします。


**Put コマンド**
**プログラム**：C:\cygwin64\bin\bash
**引数**：- C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**作業ディレクトリ**：\$ProjectFileDir\$
![put-command](assets/put-command.png)

**Get コマンド**
**プログラム**：C:\cygwin64\bin\bash
**引数**：-l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**作業ディレクトリ**：\$ProjectFileDir\$
![get-command](assets/get-command.png)

**ステータスコマンド**
**プログラム**：C:\cygwin64\bin\bash
**引数**：-l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**作業ディレクトリ**：\$ProjectFileDir\$
![status-command](assets/status-command.png)

**差分コマンド**
**プログラム**：C:\cygwin64\bin\bash
**引数**：-l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**作業ディレクトリ**：\$ProjectFileDir\$
![diff-command](assets/diff-command.png)

.repo ファイルを [repo.zip](assets/repo.zip) から抽出し、AEM プロジェクトのルートフォルダーに配置します （C:\CloudManager\aem-banking-application）。.repo ファイルを開き、サーバーと資格情報の設定が環境と一致していることを確認します。
.gitignore ファイルを開き、ファイルの下部に次の内容を追加して、変更を保存します
\# repo 
.repo

ui.content など、aem-banking-application プロジェクト内の任意のプロジェクトを選択して右クリックすると repo オプションが表示され、repo オプションの下に以前追加した 4 つのコマンドが表示されます。

## AEM オーサーインスタンスをセットアップします。{#set-up-aem-author-instance}

次の手順に従って、ローカルシステム上にクラウド対応インスタンスをすばやくセットアップできます。
* [最新の AEM SDK をダウンロード](https://experience.adobe.com/#/downloads/content/software-distribution/jp/aemcloud.html)

* [最新の AEM Forms アドオンをダウンロード](https://experience.adobe.com/#/downloads/content/software-distribution/jp/aemcloud.html)

* 次のフォルダー構造を作成します

c:\aemformscs\aem-sdk\author

* AEM SDK zip ファイルから aem-sdk-quickstart-xxxxxx.jar ファイルを抽出し、 c:\aemformscs\aem-sdk\author folder に配置して、JAR ファイルの名前を aem-author-p4502.jar に変更します。

* コマンドプロンプトを開き、 c:\aemformscs\aem-sdk\author に移動してコマンド java -jar aem-author-p4502.jar -gui を入力します。 
これにより、AEM のインストールが開始されます。
* 管理者／管理者資格情報を使用してログイン
* AEM インスタンスを停止
* フォルダー構造 C:\aemformscs\aem-sdk\author\crx-quickstart\install を作成します。
* aem-forms-addon-xxxxx.far をインストールフォルダーにコピーします。
* コマンドプロンプトを開き、 c:\aemformscs\aem-sdk\author に移動してコマンド java -jar aem-author-p4502.jar -gui を入力します。 
これにより、AEM インスタンスに forms アドオンパッケージがデプロイされます。

## 次の手順

[AEMのフォームおよびテンプレートとAEMプロジェクトの同期](./deploy-your-first-form.md)
