---
title: HTML5 Formsの作成
description: HTML5フォームの作成と設定
feature: 'モバイルフォーム '
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: 開発
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 12%

---


# HTML5フォームの作成

HTML5フォームはAdobe Experience Managerの新しい機能で、HTML5形式のXFAフォームテンプレート(xdp)のレンダリングを提供します。 この機能により、XFA ベースの PDF がサポートされていないモバイルデバイスおよびデスクトップブラウザー上のフォームのレンダリングが可能です。 HTML5 フォームは XFA フォームテンプレートの既存の機能をサポートしているだけでなく、モバイルデバイスのための新しい機能（例えば手書き署名）も追加します。

## 前提条件

AEM Formsのインスタンスが動作していることを確認してください。 [インストールガイド](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html)に従って、AEM Formsをインストールし、設定してください。

## 最初のHTML5フォームを作成する

1. [zipファイルの内容をダウンロードして抽出します](assets/assets.zip)。zipファイルにはxdpとデータファイルが含まれます
2. [Forms and Documentsに移動します。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. 作成/ファイルのアップロードをクリックします。
4. 手順2でダウンロードしたxdpテンプレートを選択します。

## HTML としてプレビュー

xdpはHTML5形式またはPDF形式でプレビューできます。 HTML5形式のxdpをプレビューするには、次の手順に従います

* 新しくアップロードされたxdpをタップし、_プレビュー/HTMLでプレビュー_&#x200B;をクリックします。 HTML5としてレンダリングされたxdpが表示されます

>[!NOTE]
>「_PDFとしてプレビュー_」オプションを選択した場合、Acrobatプラグインが必要なダイナミックPDFがAEM Formsにレンダリングされるので、レンダリングされたPDFはブラウザーに表示されません。PDFをダウンロードし、Adobe Acrobat/Readerを使用して開く必要があります


## データを使用したプレビュー

データファイルを使用してHTML5形式のxdpをプレビューするには、次の手順に従います。

* 新しくアップロードされたxdpをタップし、_プレビュー —>データを使用してプレビュー_&#x200B;をクリックします。 データファイルを参照して選択し、「_プレビュー_」をクリックします。
* HTML5形式でレンダリングされたテンプレートが、データで事前入力されて表示されます

## xdpテンプレートの詳細プロパティの参照

xdpテンプレートの高度なプロパティを使用すると、発行日、送信ハンドラー、フォームのレンダリングプロファイル、事前入力サービスなどを指定できます。 テンプレートの詳細設定プロパティを表示するには、xdpをタップし、_properties -> Advanced_&#x200B;をクリックします。 ここには、多数のプロパティが表示されます。 これらのプロパティの一部については、こちらで説明します。

**送信URL**  - HTML5フォームの送信を処理するURLです。次のレッスンでは、これについて説明します。 ここで送信URLが指定されていない場合は、デフォルトの送信ハンドラーが呼び出され、フォームデータがブラウザーに返されます。

**HTMLレンダリングプロファイル**  - HTML5フォームは、フォームテンプレートのモバイルレンダリングを可能にするために、RESTエンドポイントとして公開されるプロファイルの概念を持っています。多くの場合、デフォルトのレンダリングプロファイルでフォームをレンダリングすれば十分です。 デフォルトのレンダリングプロファイルがニーズを満たさない場合は、[カスタムプロファイル](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html)を作成し、フォームに関連付けることができます。

**事前入力サービス**  — 事前入力サービスは、通常、バックエンドデータソースから取得したデータをフォームに入力するために使用されます。

