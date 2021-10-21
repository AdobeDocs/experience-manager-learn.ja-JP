---
title: 前提条件のインストール
description: 開発環境のセットアップに必要なソフトウェアをインストールします
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8842
source-git-commit: d38da94bd4164163a16899b565c90b159194580a
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 2%

---


# 必要なソフトウェアのインストール

このチュートリアルでは、AEM Formsプロジェクトを作成し、IntelliJ とリポジトリツールを使用してAEM FormsプロジェクトをローカルAEMインスタンスと同期するために必要な手順を説明します。 また、プロジェクトをローカル Git リポジトリに追加し、ローカル Git リポジトリを Cloud Manager リポジトリにプッシュする方法についても説明します。




このチュートリアルでは、今後、このフォルダー構造について説明します。

* [JDK 11 のインストール](https://www.oracle.com/java/technologies/downloads/#java11-windows). jdk-11.0.6_windows-x64_bin.zip をダウンロードしました。
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html)例えば、Maven をc:\maven folderにインストールした場合は、M2_HOME という環境変数をC:\maven\apache-maven-3.6.0 の値で作成し、M2_HOME\bin をパスに追加して、設定を保存する必要があります。

## AEM Project Archetype を使用した Maven プロジェクトの作成

* という名前のフォルダーを作成します。 **cloudmanager**（任意の名前を付けることができます）
* コマンドプロンプトウィンドウを開き、に移動します。 **c:\cloudmanager**
* の内容をコピーして貼り付けます。 [テキストファイル](assets/creating-maven-project.txt) をクリックします。 DarchetypeVersion=30 は、 [最新バージョン](https://github.com/adobe/aem-project-archetype/releases). この記事の執筆時点での最新バージョンは 30 でした。
* Enter キーを押してコマンドを実行します。すべてが正しく行われた場合は、ビルド成功メッセージが表示されます。





