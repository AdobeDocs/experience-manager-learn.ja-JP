---
title: Webチャネルドキュメントの配信の設定
seo-title: Webチャネルドキュメントの配信の設定
description: これは、最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの最後の部分です。 ここでは、Eメールを使用したWebチャネルドキュメントの配信について見ていきます。
seo-description: これは、最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの最後の部分です。 ここでは、Eメールを使用したWebチャネルドキュメントの配信について見ていきます。
uuid: c1066600-1abd-4401-b04f-b93c28603cc7
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 2%

---


# Webチャネルドキュメントの配信の設定{#setting-up-the-delivery-of-web-channel-document}


ここでは、Eメールを使用したWebチャネルドキュメントの配信について見ていきます。

Webチャネルのインタラクティブ通信ドキュメントを定義し、テストしたら、受信者にWebチャネルドキュメントを配信する配信メカニズムが必要です。

Webチャネルドキュメントの配信メカニズムとして電子メールを使用するには、フォームデータモデルに小さな変更を加える必要があります。

[Eメールを使用したWebチャネルの配信の詳細を確認するには](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

AEM Formsにログインします。

* Forms/データ統合に移動します。

* RetiermentAccountStatementデータモデルを編集モードで開きます。

* balancesオブジェクトを選択し、「編集」ボタンをクリックします。

* 「鉛筆」アイコンを選択して、id引数を編集モードで開きます。

* バインドを「RequestAttribute」に変更します。

* 次に示すように、連結値にaccountnumberを設定します。

* この方法では、request属性を通じてaccountnumberをフォームデータモデルに渡します

* 必ず変更を保存してください。
   ![fdm](assets/requestattribute.gif)

## Webチャネルドキュメントの電子メール配信のテスト{#test-email-delivery-of-web-channel-document}

* [パッケージマネージャーを使用したサンプルアセットのインストール](assets/webchanneldelivery.zip)
* [crxにログインします。](http://localhost:4502/crx/de/index.jsp#)

* /home/usersに移動します。

* ユーザーのノードの下でadminユーザーを検索します。

* adminユーザーのprofileノードを選択します。

* 「accountnumber」という名前のプロパティを作成します。 プロパティタイプが文字列であることを確認します。

* このaccountnumberプロパティの値を「3059827」に設定します。 この値は任意の乱数に設定できます。

* [getad.htmlを開きます。](http://localhost:4502/content/getad.html)

* このURLに関連付けられたコードは、ログインしたユーザーのアカウント番号を取得します。 次に、このアカウント番号がrequestattributeとしてFDMに渡されます。 次に、FDMは、このアカウント番号に関連付けられたデータを取得し、Webチャネルドキュメントに入力します。

>[!NOTE]
>
>crxの&#x200B;**/apps/AEMForms/fetchad/GET.jsp**&#x200B;ファイルを見てください。 String変数webChannelDocumentが有効な通信ドキュメントパスを指していることを確認してください。
