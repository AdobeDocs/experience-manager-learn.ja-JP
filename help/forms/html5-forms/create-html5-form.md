---
title: HTML5 フォームの作成
description: HTML5 フォームの作成と設定
feature: Mobile Forms
doc-type: article
version: 6.5
jira: KT-4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
duration: 101
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 100%

---

# HTML5 フォームの作成

HTML5 フォームは、Adobe Experience Manager の新機能で、XFA フォームテンプレート（xdp）を HTML5 形式でレンダリングできます。この機能により、XFA ベースの PDF がサポートされていないモバイルデバイスおよびデスクトップブラウザー上のフォームのレンダリングが可能です。HTML5 フォームは、XFA フォームテンプレートの既存の機能をサポートしているだけでなく、モバイルデバイス用に手書き署名などの新しい機能もあります。

## 前提条件

AEM Forms のインスタンスが機能していることを確認してください。[インストールガイド](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html?lang=ja)に従って、AEM Forms をインストールおよび設定してください。

## 最初の HTML5 フォームの作成

1. [zip ファイルの内容をダウンロードして解凍します](assets/assets.zip)。zip ファイルには、xdp とデータファイルが含まれています
2. [フォームとドキュメントに移動します](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. 作成／ファイルをアップロードをクリックします。
4. 手順 2 でダウンロードした xdp テンプレートを選択します

## HTML としてプレビュー

xdp は、HTML5 形式または PDF 形式でプレビューできます。HTML5 形式の xdp をプレビューするには、次の手順に従ってください。

* 新しくアップロードされた xdp をタップし、_プレビュー／HTML としてプレビュー_&#x200B;をクリックします。HTML5 としてレンダリングされた xdp が表示されます。

>[!NOTE]
>「_PDF としてプレビュー_」オプションを選択すると、AEM Forms は Acrobat プラグインを必要とする動的 PDF をレンダリングするので、レンダリングされた PDF はブラウザーに表示されません。表示するには、PDF をダウンロードし、Adobe Acrobat／Reader を使用して開く必要があります


## データを使用してプレビュー

データファイルを含む HTML5 形式の xdp をプレビューするには、次の手順に従ってください。

* 新しくアップロードされた xdp をタップし、_プレビュー／データを使用してプレビュー_&#x200B;をクリックします。データファイルを参照して選択し、「_プレビュー_」をクリックします。
* データが事前に入力された HTML5 形式でレンダリングされたテンプレートが表示されます。

## xdp テンプレートの詳細プロパティの探索

xdp テンプレートの詳細プロパティを使用すると、公開日、送信ハンドラー、フォームのレンダリングプロファイル、事前入力サービスなどを指定できます。テンプレートの詳細プロパティを表示するには、xdp をタップし、_プロパティ／詳細_&#x200B;をクリックします。ここには、多数のプロパティが表示されます。ここでは、これらのプロパティの一部について説明します。

**送信 URL** - これは、HTML5 フォームの送信を処理する URL です。次のレッスンでは、これについて説明します。ここで送信 URL が指定されていない場合、デフォルトの送信ハンドラーが呼び出され、フォームデータがブラウザーに返されます。

**HTML レンダリングプロファイル** - HTML5 フォームには、フォームテンプレートのモバイルレンダリングを可能にするため、REST エンドポイントとして公開されるプロファイルの概念があります。ほとんどの場合、デフォルトのレンダリングプロファイルで十分にフォームをレンダリングできます。デフォルトのレンダリングプロファイルがニーズを満たさない場合は、[カスタムプロファイル](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html?lang=ja)を作成してフォームに関連付けることができます。

**事前入力サービス** - 事前入力サービスは通常、バックエンドデータソースから取得したデータをフォームに入力するために使用されます。
