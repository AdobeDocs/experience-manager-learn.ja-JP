---
title: 最新のアーキタイプで Cloud Service プロジェクトを更新
description: 最新のアーキタイプでの AEM Forms Cloud Service プロジェクトの更新
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: AEM Project Archetype
jira: KT-9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
duration: 67
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 100%

---

# 古い aem アーキタイプからの移行

既存の AEM Forms プロジェクトを最新の Maven アーキタイプで更新するには、コードや設定などを古いプロジェクトから新しいプロジェクトに手動でコピーする必要があります。

次の手順に従って、アーキタイプ 30 を使用して作成したプロジェクトをアーキタイプ 33 プロジェクトに移行しました

## 最新のアーキタイプを使用した Maven プロジェクトの作成

* コマンドプロンプトを開き、c:\cloudmanager に移動します。
* 最新のアーキタイプを使用して Maven プロジェクトを作成します。
* [テキストファイル](assets/creating-maven-project.txt)の内容をコピーしてコマンドプロンプトウィンドウに貼り付けます。 [最新バージョン](https://github.com/adobe/aem-project-archetype/releases)によっては、DarchetypeVersion=33 を変更する必要があります。アーキタイプ 33 には AEM Forms の新しいテーマが含まれています。
既に aem-banking-application プロジェクトが存在する cloudmanager フォルダーに新しい Maven プロジェクトを作成するので、**DartifactId** を aem-banking-application から別のものに変更する必要があります。 この記事では aem-banking-application1 を使用しました。

>[!NOTE]
>
>この新しいプロジェクトをそのままデプロイする場合、Cloud Service インスタンスには HandleFormSubmission と SubmitToAEMServlet がなくなります。 これは、Cloud Manager を使用してプロジェクトをデプロイするたびに、`/apps` フォルダーにあるものが削除されて上書きされるからです。

## Java コードのコピー

プロジェクトが正常に作成されたら、古いプロジェクトからこの新しいプロジェクトにコードや設定などをコピーできます。

* ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```から次の場所に HandleFormSubmission サーブレットをコピーします

  ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* CustomSubmit を
  ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` を aem-banking-application から aem-banking-application1 プロジェクトへコピーします

* IntelliJ に新しいプロジェクトを読み込みます

* 次の行を含めるように、aem-banking-application1 プロジェクトの ui.apps モジュールの filter.xml を更新します
  ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

すべてのコードを新しいプロジェクトにコピーしたら、このプロジェクトを Cloud Manager にプッシュできます。

>[!NOTE]
>
>コンテンツ（アダプティブフォーム、フォームデータモデルなど）を新しいプロジェクトに同期するには、IntelliJ プロジェクトに適切なフォルダー構造を作成し、リポジトリツールの GET コマンドを使用して IntelliJ プロジェクトと AEM インスタンスを同期する必要があります。
