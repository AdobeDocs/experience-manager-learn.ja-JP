---
title: DAM フォルダーのコンテンツの選択とダウンロード
description: チェックボックスコンポーネントに DAM フォルダーのコンテンツを入力し、ユーザーが選択したコンテンツをダウンロードできるようにするチュートリアルです。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: a2bbb26751c9182056b4fe6d36eeeec964001df8
workflow-type: ht
source-wordcount: '99'
ht-degree: 100%

---

# はじめに

一般的なユースケースとしては、チェックボックスコンポーネントを使用して DAM フォルダーのコンテンツ（pdf ファイル、Word ドキュメントなど）をリストアップし、ユーザーがダウンロードするファイルを選択できるようにすることが挙げられます。選択したファイルは、ユーザーがダウンロードできる 1 つのファイルに組み立てられます。

![ユースケース](assets/newsletters-download1.png)

## 前提条件

以下が必要です。

* フォームアドオンパッケージがインストールされた AEM の実用的なインスタンス

* [このドキュメントに従って設定された開発環境](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=ja)



