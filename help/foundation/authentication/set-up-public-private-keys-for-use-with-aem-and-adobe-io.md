---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEMは、公開/秘密キーのペアを使用して、Adobe I/Oや他のWebサービスと安全に通信します。 この短いチュートリアルでは、AEMとAdobe I/Oの両方で動作するopensslコマンドラインツールを使用して、キーとキーストアの互換性をどのように生成できるかを説明します。 '
version: 6.4, 6.5
feature: authentication
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 0%

---


# Adobe I/Oで使用する公開鍵と秘密鍵を設定する

AEMは、公開/秘密キーのペアを使用して、Adobe I/Oや他のWebサービスと安全に通信します。 この短いチュートリアルは、AEMとAdobe I/Oの両方で動作する[!DNL openssl]コマンドラインツールを使って、キーとキーストアの互換性をどのように生成できるかを説明しています。

>[!CAUTION]
>
>このガイドは、開発および低環境での使用に役立つ自己署名付きのキーを作成します。 実稼働シナリオでは、鍵は通常、組織のITセキュリティチームによって生成および管理されます。

## 公開鍵と秘密鍵のペア{#generate-the-public-private-key-pair}の生成

[[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html)コマンドラインツールの[[!DNL req] コマンド](https://www.openssl.org/docs/man1.0.2/man1/req.html)は、Adobe I/OやAdobe Experience Managerと互換性のあるキーペアを生成するのに使用できます。

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

[!DNL openssl generate]コマンドを完了するには、要求されたときに証明書情報を指定します。 Adobe I/OとAEMは、これらの値が何であるかは気にしませんが、これらの値を調整し、キーを記述する必要があります。

```
Generating a 2048 bit RSA private key
...........................................................+++
...+++
writing new private key to 'private.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:US
State or Province Name (full name) []:CA
Locality Name (eg, city) []:San Jose
Organization Name (eg, company) []:Example Co
Organizational Unit Name (eg, section) []:Digital Marketing
Common Name (eg, fully qualified host name) []:com.example
Email Address []:me@example.com
```

## 新しいキ追加ーストア{#add-key-pair-to-a-new-keystore}へのキーペア

キーペアは、新しい[!DNL PKCS12]キーストアに追加できます。 [[!DNL openssl]'s [!DNL pcks12] コマンドの一部として、](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html)キーストアの名前（`-  caname`経由）、キーの名前（`-name`経由）、キーストアのパスワード（`-  passout`経由）が定義されます。

これらの値は、キーストアとキーをAEMに読み込むために必要です。

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

このコマンドの出力は`keystore.p12`ファイルです。

>[!NOTE]
>
>**[!DNL my-keystore]**、**[!DNL my-key]**、**[!DNL my-password]**&#x200B;のパラメーター値は、独自の値に置き換えられます。

## キーストアの内容を確認{#verify-the-keystore-contents}

Java [[!DNL keytool] コマンドラインツール](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818)は、キーストアを表示し、キーストアファイル([!DNL keystore.p12])にキーが正常に読み込まれたことを確認します。

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![Adobe I/Oでのキーストアの確認](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## AEM {#adding-the-keystore-to-aem}へのキーストアの追加

AEMは、生成された&#x200B;**秘密鍵**&#x200B;を使用して、Adobe I/Oや他のWebサービスと安全に通信します。 秘密鍵をAEMにアクセスできるようにするには、AEMユーザーのキーストアに秘密鍵をインストールする必要があります。

**AEM/[!UICONTROL ツール]/[!UICONTROL セキュリティ]/[!UICONTROL ユーザー]**&#x200B;と&#x200B;**ユーザーを編集**&#x200B;に移動して、秘密鍵を関連付けます。

### AEMキーストアの作成{#create-an-aem-keystore}

![キーストアを](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEMで作成し、 [!UICONTROL ツール] / [!UICONTROL セキュリティ] /ユーザー  /ユーザーを編集*

キーストアの作成を求めるプロンプトが表示されたら、作成します。 このキーストアはAEMにのみ存在し、opensslを介して作成されたキーストアではありません。 パスワードは任意のものにすることができ、[!DNL openssl]コマンドで使用されるパスワードと同じでなくてもかまいません。

### キーストア{#install-the-private-key-via-the-keystore}を介して秘密鍵をインストールします

![AEMUser追加追加のPrivate Key in ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL AEMUser] / [!UICONTROL Keystore] /キーストアの [!UICONTROL 秘密鍵]*

ユーザーのキーストアコンソールで、「**[!UICONTROL Private Key form KeyStore file]**」をクリックし、追加次の情報を追加します。

* **[!UICONTROL 新しいエイリアス]**:aemのキーのエイリアス。これは任意の値を指定でき、opensslコマンドで作成されたキーストアの名前と対応する必要はありません。
* **[!UICONTROL キーストアファイル]**:openssl pkcs12コマンドの出力(keystore.p12)
* **[!UICONTROL Private Key Alias]**:openssl pkcs12コマンドで `-  passout` 引数を介して設定されたパスワード。

* **[!UICONTROL Private Key Password]**:openssl pkcs12コマンドで `-  passout` 引数を介して設定されたパスワード。

### 秘密鍵がAEMキーストアに読み込まれていることを確認します{#verify-the-private-key-is-loaded-into-the-aem-keystore}

![AEMUserでの秘密鍵の確認](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL /]  [!UICONTROL キーストア]*

秘密鍵が指定されたキーストアからAEMキーストアに正常に読み込まれると、秘密鍵のメタデータがユーザーのキーストアコンソールに表示されます。

## 公開鍵をAdobe I/Oに追加する{#adding-the-public-key-to-adobe-i-o}

一致する公開鍵をAdobe I/Oにアップロードし、AEMサービスユーザが公開鍵に対応する秘密鍵を持っていて、安全に通信できるようにする必要があります。

### Adobe I/Oの新しい統合を作成{#create-a-adobe-i-o-new-integration}

![Adobe I/Oの新しい統合の作成](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Adobe I/O統合の作成]](https://console.adobe.io/) / [!UICONTROL 新しい統合]*

Adobe I/Oで新しい統合を作成するには、公開証明書をアップロードする必要があります。 `openssl req`コマンドで生成された&#x200B;**certificate.crt**&#x200B;をアップロードします。

### 公開鍵がAdobe I/O{#verify-the-public-keys-are-loaded-in-adobe-i-o}に読み込まれていることを確認します

![Adobe I/Oで公開鍵を確認](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

インストールされた公開鍵とその有効期限は、Adobe I/Oの[!UICONTROL 統合]コンソールに一覧表示されます。**[!UICONTROL 公開鍵]**&#x200B;ボタンを使用して、複数の公開鍵追加を追加できます。

AEMは秘密鍵を保持し、Adobe I/O統合は対応する公開鍵を保持するようになり、AEMはAdobe I/Oと安全に通信できます。
