---
title: IntelliJ コミュニティエディションのインストール
description: AEMプロジェクトをインストールして IntelliJ にインポートします
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 0%

---

# IntelliJ のインストール

インストール [IntelliJ コミュニティエディション](https://www.jetbrains.com/idea/download/#section=windows). インストール時に推奨されるデフォルトの設定を受け入れることができます。

## AEMプロジェクトをインポート

* IntelliJ の起動
* 前の手順で作成したAEMプロジェクトを読み込みます。 プロジェクトが読み込まれると、画面は次のようになります ![aem-banking-app](assets/aem-banking-app.png). 通常は、core,ui.apps,ui.config および ui.content サブプロジェクトを使用します。
* Maven とターミナルのウィンドウが表示されない場合は、view->Tools Window に移動し、Maven とターミナルを選択してください。

## フォントモジュールの追加

PDFファイルでカスタムフォントを使用する場合は、カスタムフォントをAEM Forms CS インスタンスにプッシュする必要があります。 次の手順に従ってください

* という名前のフォルダーを作成します。 **フォント** C:\CloudManager\aem-banking-application
* コンテンツを抽出 [font.zip](assets/fonts.zip) 新しく作成されたフォントフォルダーに
* フォントモジュールには、いくつかのカスタムフォントが含まれています。組織のカスタムフォントをC:\CloudManager\aem-banking-application\fonts\src\main\resources folder of the fonts moduleフォントに追加できます
* C:\CloudManager\aem-banking-application\pom.xmlファイルを開きます。
* 次の行を追加します。  ```<module>fonts</module>``` pom.xml のモジュールセクション
* pom.xml を保存します。
* IntelliJ での aem-banking-application プロジェクトの更新

フォントモジュールを含むプロジェクト構造
![fonts-module](assets/fonts-module.png)

プロジェクト POM に含まれるフォントモジュール
![fonts-pom](assets/fonts-module-pom.png)
