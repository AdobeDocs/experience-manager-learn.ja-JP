---
title: Web チャネルドキュメントの配信のセットアップ
description: これは、最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの最後のパートです。ここでは、メールを使用した web チャネルドキュメントの配信について見てみます。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: Development
role: Developer
level: Beginner
exl-id: 510d1782-59b9-41a6-a071-a16170f2cd06
duration: 68
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 100%

---

# Web チャネルドキュメントの配信のセットアップ {#setting-up-the-delivery-of-web-channel-document}


ここでは、メールを使用した web チャネルドキュメントの配信について見てみます。

Web チャネルのインタラクティブ通信ドキュメントを定義しテストしたら、web チャネルドキュメントを受信者に配信するための配信メカニズムが必要になります。

メールを web チャネルドキュメントの配信メカニズムとして使用できるようにするには、フォームデータモデルに小さな変更を加える必要があります。

[メールを使用した web チャネル配信の詳細を確認するには：](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

AEM Forms にログインします。

* Forms／データ統合に移動します。

* RetiarementAccountStatement データモデルを編集モードで開きます。

* balances オブジェクトを選択し、「編集」ボタンをクリックします。

* 「鉛筆」アイコンを選択して、id 引数を編集モードで開きます。

* バインディングを「RequestAttribute」に変更します。

* バインディング値に accountnumber を設定します（下図を参照）。

* このようにして、request 属性を通じてフォームデータモデルに accountnumber を渡します。

* 必ず変更内容を保存してください。
  ![fdm](assets/requestattribute.gif)

## Web チャンネルドキュメントのメール配信のテスト {#test-email-delivery-of-web-channel-document}

* [パッケージマネージャーを使用してサンプルアセットをインストールします。](assets/webchanneldelivery.zip)
* [crx にログインします](http://localhost:4502/crx/de/index.jsp#)。

* /home/users に移動します。

* ユーザーのノード下で管理者ユーザーを検索します。

* 管理者ユーザーのプロファイルノードを選択します。

* 「accountnumber」という名前のプロパティを作成します。 プロパティタイプが文字列であることを確認します。

* この accountnumber プロパティの値を「3059827」に設定します。この値は任意の乱数に設定できます。

* [getad.html を開きます](http://localhost:4502/content/getad.html)。

* この URL に関連付けられているコードは、ログインしたユーザーの accountnumber を取得します。 次に、この accountnumber が requestattribute として FDM に渡されます。 次に、FDM は、この accountnumber に関連付けられているデータを取得し、web チャンネルドキュメントに入力します。

>[!NOTE]
>
>詳しくは、crx で **/apps/AEMForms/fetchad/GET.jsp** ファイルを確認してください。String 変数 webChannelDocument が有効なコミュニケーションドキュメントパスを指していることを確認してください。

## 次の手順

[メール配信のセットアップ](../interactive-communications/delivery-of-web-channel-document-tutorial-use.md)