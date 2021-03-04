---
title: Webチャネルドキュメントの配信の設定
seo-title: Webチャネルドキュメントの配信の設定
description: これは、最初の対話型通信ドキュメントを作成するためのマルチステップチュートリアルの最後の部分です。 ここでは、EメールでのWebチャネルドキュメントの配信について見てみます。
seo-description: これは、最初の対話型通信ドキュメントを作成するためのマルチステップチュートリアルの最後の部分です。 ここでは、EメールでのWebチャネルドキュメントの配信について見てみます。
uuid: c1066600-1abd-4401-b04f-b93c28603cc7
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: 開発
role: デベロッパー
level: 初心者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---


# Webチャネルドキュメントの配信の設定{#setting-up-the-delivery-of-web-channel-document}


ここでは、EメールでのWebチャネルドキュメントの配信について見てみます。

Webチャネルの対話型通信ドキュメントを定義してテストした後は、Webチャネルドキュメントを受信者に配信する配信メカニズムが必要です。

Webチャネルドキュメントの配信メカニズムとして電子メールを使用するには、フォームデータモデルを少し変更する必要があります。

[電子メールでのWebチャネルの配信の詳細を参照するには](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

AEM Formsにログインします。

* Forms/データ統合に移動します。

* RetiarementAccountStatementデータモデルを編集モードで開きます。

* balancesオブジェクトを選択し、「編集」ボタンをクリックします。

* 「鉛筆」アイコンを選択し、id引数を編集モードで開きます。

* 連結を「RequestAttribute」に変更します。

* 次に示すように、連結値にaccountnumberを設定します。

* この方法で、request属性を通してフォームデータモデルにaccountnumberを渡します。

* 変更を保存してください。
   ![fdm](assets/requestattribute.gif)

## Webチャネルドキュメントの電子メール配信のテスト{#test-email-delivery-of-web-channel-document}

* [パッケージマネージャーを使用したサンプルアセットのインストール](assets/webchanneldelivery.zip)
* [crxにログイン](http://localhost:4502/crx/de/index.jsp#)

* /home/usersに移動します。

* ユーザーのノードで「admin user」を検索します。

* 管理者ユーザーのプロファイルノードを選択します。

* 「accountnumber」という名前のプロパティを作成します。 プロパティタイプが文字列であることを確認します。

* このaccountnumberプロパティの値を&quot;3059827&quot;に設定します。 この値は任意の乱数に設定できます。

* [getad.htmlを開きます。](http://localhost:4502/content/getad.html)

* このURLに関連付けられているコードは、ログインしたユーザーのアカウント番号を取得します。 次に、このaccountnumberがrequestattributeとしてFDMに渡されます。 次に、FDMは、このアカウント番号に関連付けられたデータを取得し、Webチャネル・ドキュメントに入力します。

>[!NOTE]
>
>crxの&#x200B;**/apps/AEMForms/fetchad/GET.jsp**&#x200B;ファイルを見てください。 String変数webChannelDocumentが有効な通信ドキュメントパスを指していることを確認してください。
