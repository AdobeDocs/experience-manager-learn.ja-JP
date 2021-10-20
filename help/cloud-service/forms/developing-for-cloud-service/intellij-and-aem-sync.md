---
title: Repo ツールを使用した IntelliJ の設定
description: AEM Cloud Ready インスタンスと同期する IntelliJ の準備
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8844
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 3%

---

# Cygwin のインストール

インストール [シグウィン](https://www.cygwin.com/). C:\cygwin64 folderにインストールしました。
>[注意]
> zip、unzip、curl、rsync パッケージを cygwin インストールと一緒にインストールしてください

[リポジトリツールのインストール].(https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo).Installing repo ツールは、repo ファイルをコピーしてc:\cloudmanger\adoberepo folderに配置するだけです。

次の内容を Path 環境変数C:\cygwin64\bin;C:\CloudManager\adoberepo；に追加します。

## 外部ツールの設定

IntelliJ を起動 Ctrl+Alt+S キーを押して設定ウィンドウを起動しますツール/外部ツールを選択し、スクリーンショットに示すように+記号をクリックし、次のように入力します。 **repo** グループ

**Put コマンド**
**プログラム**:C:\cygwin64\bin\bash
**引数**:-l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**作業方向**:\$ProjectFileDir\$
![put-command](assets/put-command.png)

**Get コマンド**
**プログラム**:C:\cygwin64\bin\bash
**引数**:-l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**作業方向**:\$ProjectFileDir\$
![get-command](assets/get-command.png)

**ステータスコマンド**
**プログラム**:C:\cygwin64\bin\bash
**引数**:-l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**作業方向**:\$ProjectFileDir\$
![status-command](assets/status-command.png)

**差分コマンド**
**プログラム**:C:\cygwin64\bin\bash
**引数**:-l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**作業方向**:\$ProjectFileDir\$
![diff-command](assets/diff-command.png)

.repo ファイルを [repo.zip](assets/repo.zip) をクリックし、AEMプロジェクトのルートフォルダーに配置します。 (C:\CloudManager\aem-banking-application)。 .repo ファイルを開き、サーバーと資格情報の設定が環境に一致していることを確認します。
.gitignore ファイルを開き、ファイルの下部に次のコードを追加して、変更を保存します。 \# repo .repo

ui.content など、aem-banking-application プロジェクト内の任意のプロジェクトを選択して右クリックすると、repo オプションが表示され、repo オプションの下に、前に追加した 4 つのコマンドが表示されます。

## AEM オーサーインスタンスのセットアップ

次の手順に従って、ローカルシステム上にクラウド対応インスタンスをすばやくセットアップできます。
* [最新のAEM SDK のダウンロード](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* [最新のAEM Formsアドオンのダウンロード](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* 次のフォルダー構造を作成しますc:\aemformscs\aem-sdk\author

* AEM SDK zip ファイルから aem-sdk-quickstart-xxxxxx.jar ファイルを抽出し、 c:\aemformscs\aem-sdk\author folder.Renameに配置して、 jar ファイルを aem-author-p4502.jar に配置します。

* コマンドプロンプトを開き、 c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui に移動します。 これにより、AEMのインストールが開始されます。
* 管理者/管理者資格情報を使用したログイン
* AEMインスタンスの停止
* 次のフォルダー構造を作成します。C:\aemformscs\aem-sdk\author\crx-quickstart\install
* aem-forms-addon-xxxxx.far を install フォルダーにコピーします。
* コマンドプロンプトを開き、 c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui に移動します。 これにより、AEMインスタンスに Forms アドオンパッケージがデプロイされます。



















