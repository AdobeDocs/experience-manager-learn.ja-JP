---
title: HTML5Formsの作成
description: HTML5フォームの作成と設定
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 11%

---


# HTML5フォームの作成

HTML5フォームは、オファーがHTML5形式でXFAフォームテンプレート(xdp)をレンダリングする、Adobe Experience Managerの新しい機能です。 この機能により、XFA ベースの PDF がサポートされていないモバイルデバイスおよびデスクトップブラウザー上のフォームのレンダリングが可能です。 HTML5 フォームは XFA フォームテンプレートの既存の機能をサポートしているだけでなく、モバイルデバイスのための新しい機能（例えば手書き署名）も追加します。

## 前提条件

AEM Formsの作業インスタンスがあることを確認してください。 インストー [ルガイドに従って](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) 、AEM Formsをインストールし、構成してください。

## 最初のHTML5フォームの作成

1. [zipファイルの内容をダウンロードして抽出します](assets/assets.zip)。 zipファイルにはxdpとデータファイルが含まれます
2. [Formsとドキュメントに移動](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. 「作成/ファイルのアップロード」をクリックします。
4. 手順2でダウンロードしたxdpテンプレートを選択します

## HTML としてプレビュー

xdpは、HTML5形式またはPDF形式でプレビューできます。 HTML5形式でxdpをプレビューするには、次の手順に従います

* 新しくアップロードされたxdpをタップし、「 _プレビュー/プレビューをHTMLとしてアップロード_」をクリックします。 xdpはHTML5としてレンダリングされます

>[!NOTE]
>「 _プレビューをPDF_ 」オプションを選択した場合、Acrobatプラグインを必要とするAEM FormsレンダリングダイナミックPDFが必要なので、レンダリングされたPDFはブラウザーに表示されません。PDFをダウンロードし、Adobe Acrobat/Readerを使用して表示に開く必要があります


## データを使用してプレビュー

HTML5形式のxdpをデータファイルとプレビューするには、次の手順に従います。

* 新しくアップロードされたxdpをタップし、「 _プレビュー/データとプレビュー_」をクリックします。 データファイルを参照して選択し、「 _プレビュー_」をクリックします。
* HTML5形式でレンダリングされたテンプレートには、データが事前に入力されています

## xdpテンプレートの高度なプロパティの参照

xdpテンプレートの詳細なプロパティを使用すると、発行日、送信ハンドラー、フォームのレンダリングプロファイル、事前入力サービスなどを指定できます。 テンプレートの詳細なプロパティを表示するには、xdpをタップし、 _プロパティ/詳細をクリックします_。 多数のプロパティが表示されます。 これらのプロパティの一部を以下に示します。

**送信URL** — これは、HTML5フォームの送信を処理するURLです。 これについては、次のレッスンで説明します。 ここで送信URLが指定されていない場合は、デフォルトの送信ハンドラーが呼び出され、フォームデータがブラウザーに返されます。

**HTMLレンダリングプロファイル** - HTML5フォームには、フォームテンプレートのモバイルレンダリングを可能にするために、RESTエンドポイントとして公開されるプロファイルの概念があります。 ほとんどの場合、デフォルトのレンダリングプロファイルでフォームをレンダリングできます。 デフォルトのレンダリングプロファイルがニーズを満たさない場合は、 [カスタムプロファイル](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html) を作成してフォームに関連付けることができます。

**事前入力サービス** — 事前入力サービスは、通常、バックエンドデータソースから取得したデータをフォームに埋め込むために使用されます。

