---
title: AEM Forms OSGiでのReader拡張機能の設定
description: AEM Forms OSGiのTrust StoreにReader拡張資格情報を追加します。
feature: Reader Extensions
feature-set: Reader Extensions
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: 管理
role: Admin
level: Beginner
source-git-commit: 55a6ff5d01898b994aee60f214126c5c18a06a5e
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 8%

---


# Reader拡張機能秘密鍵証明書の追加{#configuring-reader-extension-osgi}

DocAssurance サービスは PDF ドキュメントに使用権限を適用できます。PDFドキュメントに使用権限を適用するには、証明書を設定します。

## fd-serviceユーザーのキーストアの作成

Reader Extensionsの秘密鍵証明書はfd-serviceユーザーに関連付けられます。 fd-serviceユーザーに資格情報を追加するには、次の手順に従います。 fd-serviceユーザーのキーストアを作成済みの場合は、このセクションをスキップします。

* AEMオーサーインスタンスに管理者としてログインします。
* Tools-Security-Usersに移動します。
* fd-serviceユーザーアカウントが見つかるまで、ユーザーのリストを下にスクロールします。
* fd-serviceユーザーをクリックします。
* 「キーストア」タブをクリックします。
* 「キーストアを作成」をクリックします。
* キーストアアクセスパスワードを設定し、設定を保存してキーストアパスワードを作成します。

### fd-serviceユーザーキーストアに秘密鍵証明書を追加する

ビデオに従って、fd-serviceユーザーに資格情報を追加してください

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=9&learn=on)











