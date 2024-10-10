---
title: AEM Forms OSGi での Reader 拡張機能の設定
description: AEM Forms OSGi の Trust ストアに Reader 拡張機能の資格情報を追加します。
feature: Reader Extensions
type: Tutorial
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
duration: 308
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '209'
ht-degree: 100%

---

# Reader 拡張機能の資格情報の追加{#configuring-reader-extension-osgi}

DocAssurance サービスは PDF ドキュメントに使用権限を適用できます。PDF ドキュメントに使用権限を適用するには、証明書を設定します。

## fd-service ユーザーのキーストアを作成する

Reader 拡張機能の資格情報は fd-service ユーザーに関連付けられます。fd-service ユーザーに資格情報を追加するには、次の手順に従います。fd-service ユーザーのキーストアを既に作成している場合は、このセクションをスキップします。

* AEM オーサーインスタンスに管理者としてログインします。
* Tools-Security-Users に移動します。
* fd-service ユーザーアカウントが見つかるまで、ユーザーのリストを下にスクロールします。
* fd-service ユーザーをクリックします。
* 「キーストア」タブをクリックします。
* 「キーストアを作成」をクリックします。
* キーストアのアクセスパスワードを設定し、設定を保存してキーストアのパスワードを作成してください。

### fd-service ユーザーのキーストアに資格情報を追加する

ビデオに従って、fd-service ユーザーに資格情報を追加してください。

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=12&learn=on)


pfx ファイルの詳細を一覧表示するコマンドです。次のコマンドは、pfx ファイルと同じディレクトリにあることを前提としています。

**keytool -v -list -storetype pkcs12 -keystore &lt;pfx ファイル名>**

例えば、keytool -v -list -storetype pkcs12 -keystore 1005566.pfx の場合、1005566.pfx はご使用の pfx ファイルの名前です。
