---
title: AEM Forms OSGi でのReader拡張機能の設定
description: AEM Forms OSGi の Trust Store にReader拡張資格情報を追加します。
feature: Reader Extensions
audience: developer
type: Tutorial
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 18%

---

# Reader拡張資格情報の追加{#configuring-reader-extension-osgi}

DocAssurance サービスは PDF ドキュメントに使用権限を適用できます。PDF ドキュメントに使用権限を適用するには、証明書を設定します。

## fd-service ユーザーのキーストアを作成する

Reader Extensions 証明書は fd-service ユーザーに関連付けられます。 fd-service ユーザーに秘密鍵証明書を追加するには、次の手順に従います。 fd-service ユーザーのキーストアを既に作成している場合は、このセクションをスキップします。

* AEM オーサーインスタンスに管理者としてログインします。
* Tools-Security-Users に移動します。
* fd-service ユーザーアカウントが見つかるまで、ユーザーのリストを下にスクロールします。
* fd-service ユーザーをクリックします。
* 「キーストア」タブをクリックします。
* 「キーストアを作成」をクリックします。
* キーストアアクセスパスワードを設定し、設定を保存してキーストアパスワードを作成します。

### fd-service ユーザーキーストアに秘密鍵証明書を追加する

ビデオに従って、fd-service ユーザーに認証情報を追加してください

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=9&learn=on)


pfx ファイルの詳細をリストするコマンドはです。 次のコマンドは、 pfx ファイルと同じディレクトリにあることを前提としています。

**keytool -v -list -storetype pkcs12 -keystore &lt;name of=&quot;&quot; your=&quot;&quot; pfx=&quot;&quot; file=&quot;&quot;>**

例えば、keytool -v -list -storetype pkcs12 -keystore 1005566.pfx の場合、1005566.pfx はご使用の pfx ファイルの名前です。
