---
title: アダプティブフォームのトリガーAEMワークフローの概要
description: フォーム送信時にAEMワークフローをトリガーする際のペイロードオプションの設定
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: 40258.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 9f1dbd02-774a-4b84-90fa-02d4e468cbac
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 7%

---

# アダプティブフォームのトリガーAEM Workflow への設定

## 前提条件

このワークフローで使用されるサンプルフォームは、AEMサーバーに読み込む必要があるカスタムアダプティブフォームテンプレートに基づいています。 指定されたサンプルフォームは、テンプレートの読み込み後に読み込む必要があります。

### アダプティブフォームテンプレートの取得

* ダウンロード [アダプティブフォームテンプレート](assets/af-form-template.zip)
* [パッケージマネージャーを使用したテンプレートのインポート](http://localhost:4502/crx/packmgr/index.jsp)
* アダプティブフォームテンプレートをアップロードしてインストールする

### サンプルのアダプティブフォームを取得する

* ダウンロード [アダプティブフォーム](assets/peak-application-form.zip)
* 参照先 [フォームとドキュメント](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 作成/ファイルのアップロードをクリックします。
* サンプルのアダプティブフォームは、 [アプリケーションForms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

次のビデオでは、AEMワークフローをトリガーするアダプティブフォームの設定方法を説明します
>[!VIDEO](https://video.tv.adobe.com/v/40258?quality=12&learn=on)

次のビデオでは、ワークフローのペイロードと、crx リポジトリのその他の詳細を示します

>[!VIDEO](https://video.tv.adobe.com/v/40259?quality=12&learn=on)
