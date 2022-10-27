---
title: HTML5 Formsを作成
description: Forms5 の作成とHTML
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 12%

---

# HTML5 フォームの作成

HTML5 フォームは、Adobe Experience Managerの新しい機能で、XFA フォームテンプレート (xdp) のHTML5 形式でのレンダリングを提供します。 この機能により、XFA ベースの PDF がサポートされていないモバイルデバイスおよびデスクトップブラウザー上のフォームのレンダリングが可能です。HTML5 フォームは XFA フォームテンプレートの既存の機能をサポートしているだけでなく、モバイルデバイスのための新しい機能（例えば手書き署名）も追加します。

## 前提条件

AEM Formsのインスタンスが正常に動作していることを確認してください。 詳しくは、 [インストールガイド](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) AEM Formsをインストールして設定する

## 最初のHTML5 フォームを作成

1. [zip ファイルの内容をダウンロードして抽出します。](assets/assets.zip). zip ファイルには xdp とデータファイルが含まれます
2. [Formsとドキュメントに移動します。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. 作成/ファイルのアップロードをクリックします。
4. 手順 2 でダウンロードした xdp テンプレートを選択します。

## HTML としてプレビュー

xdp は、HTML5 形式またはPDF形式でプレビューできます。 xdp を Preview5 形式でHTMLするには、次の手順に従います

* 新しくアップロードされた xdp をタップし、 _プレビュー/HTMLとしてプレビュー_. xdp がHTML5 としてレンダリングされているのを確認します。

>[!NOTE]
>次を選択した場合： _プレビューをPDF_ オプションAcrobatPDFが必要なダイナミック pdf のレンダリングがAEM Formsに必要なので、レンダリングされたPDFはブラウザーに表示されません。表示するには、プラグインをダウンロードし、Adobe Acrobat/Readerを使用して開く必要があります


## データを使用してプレビュー

データファイルを含むHTML5 形式の xdp をプレビューするには、次の手順に従います。

* 新しくアップロードされた xdp をタップし、 _プレビュー/データを使用してプレビュー_. データファイルを参照して選択し、「 _プレビュー_.
* テンプレートがHTML5 形式でレンダリングされ、データが事前に入力されているのが確認できます。

## xdp テンプレートの詳細プロパティの参照

xdp テンプレートの詳細プロパティを使用すると、発行日、送信ハンドラー、フォームのレンダリングプロファイル、事前入力サービスなどを指定できます。 テンプレートの高度なプロパティを表示するには、xdp をタップし、 _プロパティ/詳細_. ここには、多数のプロパティが表示されます。 これらのプロパティの一部をここで説明します。

**送信 URL**  — これは、HTML5 フォームの送信を処理する URL です。 次のレッスンでは、これについて説明します。 ここで送信 URL が指定されていない場合は、デフォルトの送信ハンドラーが呼び出され、フォームデータがブラウザーに返されます。

**HTMLレンダリングプロファイル** -HTML5 フォームは、フォームテンプレートのモバイルレンダリングを可能にする REST エンドポイントとして公開されるプロファイルの概念を持っています。 デフォルトのレンダリングプロファイルでは、フォームをレンダリングするのに十分な時間がほとんどです。 デフォルトのレンダリングプロファイルがニーズを満たさない場合、 [カスタムプロファイル](https://experienceleague.adobe.com/docs/experience-manager-64/forms/html5-forms/custom-profile.html) を作成し、フォームに関連付けることができます。

**事前入力サービス**  — 事前入力サービスは、通常、バックエンドデータソースから取得したデータをフォームに入力するために使用されます。
