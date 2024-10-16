---
title: 前提条件のインストール
description: 開発環境を設定するために必要なソフトウェアをインストールする
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
duration: 44
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '222'
ht-degree: 100%

---

# 必要なソフトウェアのインストール

このチュートリアルでは、AEM Forms プロジェクトを作成し、IntelliJ とリポジトリツールを使用して AEM Forms プロジェクトをローカル AEM インスタンスと同期するために必要な手順を説明します。 また、プロジェクトをローカル Git リポジトリに追加し、ローカル Git リポジトリを Cloud Manager リポジトリにプッシュする方法についても説明します。





このチュートリアルでは、このフォルダー構造について説明します。

* [JDK 11 をインストールします](https://www.oracle.com/java/technologies/downloads/#java11-windows)。jdk-11.0.6_windows-x64_bin.zip をダウンロードしました
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html)。例えば、Maven を c:\maven フォルダーにインストールした場合は、M2_HOME という環境変数をC:\maven\apache-maven-3.6.0 の値で作成し、その後、M2_HOME\bin をパスに追加して、設定を保存する必要があります。

## AEM Project Archetype を使用した Maven プロジェクトの作成

* C ドライブに **cloudmanager** という名前のフォルダーを作成します（任意の名前を付けることができます）。
* コマンドプロンプトウィンドウを開き、**c:\cloudmanager** に移動します。 
* [テキストファイル](assets/creating-maven-project.txt)の内容をコピーしてコマンドプロンプトウィンドウに貼り付けます。 [最新バージョン](https://github.com/adobe/aem-project-archetype/releases)によっては、DarchetypeVersion=30 を変更する必要があります。 この記事の作成時点での最新バージョンは 30 でした。
* Enter キーを押してコマンドを実行します。すべてが正しく行われた場合は、ビルド成功のメッセージが表示されます。

## 次の手順

[IntelliJ のインストール](./intellij-set-up.md)