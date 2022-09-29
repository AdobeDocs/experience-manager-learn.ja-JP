---
title: 最新のアーキタイプで Cloud Service プロジェクトを更新しています
description: 最新のアーキタイプでのAEM Forms Cloud Service プロジェクトの更新
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 1%

---

# 古い aem アーキタイプからの移行

既存のAEM Formsプロジェクトを最新の Maven アーキタイプで更新するには、コードや設定などを古いプロジェクトから新しいプロジェクトに手動でコピーする必要があります。

次の手順に従って、アーキタイプ 30 を使用して作成したプロジェクトをアーキタイプ 33 プロジェクトに移行しました

## 最新のアーキタイプを使用した Maven プロジェクトの作成

* コマンドプロンプトを開き、 c:\cloudmanagerに移動します。
* 最新のアーキタイプを使用して Maven プロジェクトを作成します。
* の内容をコピーして貼り付けます。 [テキストファイル](assets/creating-maven-project.txt) をクリックします。 DarchetypeVersion=33 を [最新バージョン](https://github.com/adobe/aem-project-archetype/releases). アーキタイプ 33 には新しいAEM Formsテーマが含まれています。
既に aem-banking-application プロジェクトが存在する cloudmanager フォルダーに新しい Maven プロジェクトを作成するので、 **DartifactId** aem-banking-application から別のものへ。 この記事では aem-banking-application1 を使用しました。

>[!NOTE]
>
>この新しいプロジェクトをそのままデプロイする場合、クラウドサービスインスタンスには HandleFormSubmission と SubmitToAEMServlet がありません。 これは、Cloud Manager を使用してプロジェクトをデプロイするたびに、 `/apps` フォルダーが削除され、上書きされます。

## Java コードをコピーする

プロジェクトが正常に作成されたら、古いプロジェクトからこの新しいプロジェクトにコードや設定などをコピーできます。

* HandleFormSubmission サーブレットを次の場所にコピーします。 ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
から

   ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* 次の CustomSubmit をコピーする
   ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` aem-banking-application から aem-banking-application1 プロジェクトへ

* 新しいプロジェクトを IntelliJ にインポート

* 次の行を含めるように、aem-banking-application1 プロジェクトの ui.apps モジュールの filter.xml を更新します。
   ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

すべてのコードを新しいプロジェクトにコピーしたら、このプロジェクトを Cloud Manager にプッシュできます。

>[!NOTE]
>
>コンテンツ ( アダプティブForms、フォームデータモデルなど ) を新しいプロジェクトに同期するには、IntelliJ プロジェクトに適切なフォルダー構造を作成し、リポジトリツールの「取得」コマンドを使用して IntelliJ プロジェクトとAEMインスタンスを同期する必要があります。
