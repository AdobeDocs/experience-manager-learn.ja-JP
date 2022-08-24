---
title: Repo ツールを使用した IntelliJ の設定
description: AEM Cloud Ready インスタンスと同期する IntelliJ の準備
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8844
exl-id: 9a7ed792-ca0d-458f-b8dd-9129aba37df6
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 2%

---

# Cygwin のインストール


Cygwin は、Microsoft Windows 上でネイティブに動作する POSIX 互換のプログラミングおよびランタイム環境です。
インストール [シグウィン](https://www.cygwin.com/). C:\cygwin64 folderでをインストールしました。
>[!NOTE]
> zip、unzip、curl、rsync パッケージを cygwin のインストールと一緒にインストールしてください。

c:\cloudmanagerフォルダーの下に adoberepo というフォルダーを作成します。

[リポジトリツールのインストール].(https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo).Installing repo ツールは、repo ファイルをコピーしてc:\cloudmanger\adoberepo folderに配置するだけです。

Path 環境変数C:\cygwin64\bin;C:\CloudManager\adoberepo；に以下を追加します。

## 外部ツールを設定

* IntelliJ の起動
* Ctrl + Alt + S キーを押して、設定ウィンドウを起動します。
* ツール/外部ツールを選択し、スクリーンショットに示すように、「+」記号をクリックして、次のように入力します。
   ![rep](assets/repo.png)
* 必ず、「グループ」ドロップダウンフィールドに「repo」と入力して、repo という名前のグループを作成し、作成するすべてのコマンドを **repo** グループ


**Put コマンド**
**プログラム**:C:\cygwin64\bin\bash
**引数**:-l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**作業ディレクトリ**:\$ProjectFileDir\$
![put-command](assets/put-command.png)

**Get コマンド**
**プログラム**:C:\cygwin64\bin\bash
**引数**:-l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**作業ディレクトリ**:\$ProjectFileDir\$
![get-command](assets/get-command.png)

**ステータスコマンド**
**プログラム**:C:\cygwin64\bin\bash
**引数**:-l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**作業ディレクトリ**:\$ProjectFileDir\$
![status-command](assets/status-command.png)

**差分コマンド**
**プログラム**:C:\cygwin64\bin\bash
**引数**:-l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**作業ディレクトリ**:\$ProjectFileDir\$
![diff-command](assets/diff-command.png)

.repo ファイルを次の場所から抽出します。 [repo.zip](assets/repo.zip) AEMプロジェクトのルートフォルダーに配置します。 (C:\CloudManager\aem-banking-application)。 .repo ファイルを開き、サーバーと資格情報の設定が環境と一致していることを確認します。
.gitignore ファイルを開き、ファイルの下部に次の内容を追加して、変更を保存します\# repo .repo

ui.content など、aem-banking-application プロジェクト内の任意のプロジェクトを選択し、右クリックすると repo オプションが表示され、repo オプションの下には、前に追加した 4 つのコマンドが表示されます。

## AEM オーサーインスタンスを設定

次の手順に従って、ローカルシステム上にクラウド対応インスタンスをすばやくセットアップできます。
* [最新のAEM SDK をダウンロード](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* [最新のAEM Formsアドオンをダウンロード](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* 次のフォルダー構造を作成しますc:\aemformscs\aem-sdk\author

* AEM SDK zip ファイルから aem-sdk-quickstart-xxxxxx.jar ファイルを展開し、 c:\aemformscs\aem-sdk\author folder.Renameに配置して、 jar ファイルを aem-author-p4502.jar に配置します。

* コマンドプロンプトを開き、 c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui に移動します。 これにより、AEMのインストールが開始されます。
* 管理者/管理者資格情報を使用してログイン
* AEMインスタンスを停止
* 次のフォルダー構造を作成します。C:\aemformscs\aem-sdk\author\crx-quickstart\install
* aem-forms-addon-xxxxx.far を install フォルダーにコピーします。
* コマンドプロンプトを開き、 c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui に移動します。 これにより、AEMインスタンスに forms アドオンパッケージがデプロイされます。
