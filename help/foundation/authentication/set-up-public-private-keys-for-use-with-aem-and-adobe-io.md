---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEMは、公開鍵と秘密鍵のペアを使用して、Adobe I/Oや他のWebサービスと安全に通信します。 この短いチュートリアルでは、AEMとAdobe I/Oの両方で動作するopensslコマンドラインツールを使用して、互換性のあるキーとキーストアを生成する方法を説明します。 '
version: 6.4, 6.5
feature: 'ユーザーとグループ '
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 0%

---


# Adobe I/Oで使用する公開鍵と秘密鍵の設定

AEMは、公開鍵と秘密鍵のペアを使用して、Adobe I/Oや他のWebサービスと安全に通信します。 この短いチュートリアルでは、AEMとAdobe I/Oの両方で動作する[!DNL openssl]コマンドラインツールを使用して、互換性のあるキーとキーストアを生成する方法を説明します。

>[!CAUTION]
>
>このガイドでは、開発や低レベルの環境での使用に役立つ自己署名付きキーを作成します。 通常、実稼動シナリオでは、キーは組織のITセキュリティチームによって生成および管理されます。

## 公開鍵と秘密鍵のペア{#generate-the-public-private-key-pair}を生成します。

[[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html)コマンドラインツールの[[!DNL req] コマンド](https://www.openssl.org/docs/man1.0.2/man1/req.html)を使用して、Adobe I/OとAdobe Experience Managerに対応したキーペアを生成できます。

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

[!DNL openssl generate]コマンドを完了するには、要求されたときに証明書情報を指定します。 Adobe I/OとAEMは、これらの値が何であるかは気にしませんが、キーに合わせてキーを記述する必要があります。

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

## 新しいキーストアにキーペアを追加します。 {#add-key-pair-to-a-new-keystore}

キーペアを新しい[!DNL PKCS12]キーストアに追加できます。 [[!DNL openssl]'s [!DNL pcks12] コマンドの一部として、](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html)はキーストアの名前（`-  caname`経由）、キーの名前（`-name`経由）、キーストアのパスワード（`-  passout`経由）を定義します。

これらの値は、キーストアとキーをAEMに読み込むために必要です。

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

このコマンドの出力は`keystore.p12`ファイルです。

>[!NOTE]
>
>**[!DNL my-keystore]**、**[!DNL my-key]**&#x200B;および&#x200B;**[!DNL my-password]**&#x200B;のパラメーター値は、独自の値に置き換えられます。

## キーストアの内容を確認します。 {#verify-the-keystore-contents}

Javaの[[!DNL keytool] コマンドラインツール](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818)は、キーストアを表示し、キーストアファイル([!DNL keystore.p12])にキーが正常に読み込まれたことを確認します。

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![Adobe I/Oでのキーストアの検証](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## AEM {#adding-the-keystore-to-aem}へのキーストアの追加

AEMは、生成された&#x200B;**秘密鍵**&#x200B;を使用して、Adobe I/Oや他のWebサービスと安全に通信します。 秘密鍵をAEMにアクセスできるようにするには、AEMユーザーのキーストアに秘密鍵をインストールする必要があります。

**AEM > [!UICONTROL ツール] > [!UICONTROL セキュリティ] > [!UICONTROL ユーザー]**&#x200B;と&#x200B;**ユーザーを編集**&#x200B;に移動します。

### AEMキーストア{#create-an-aem-keystore}の作成

![AEMでキーストアを](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*作成/ツ [!UICONTROL ール] / [!UICONTROL セキュリティ] / [!UICONTROL ユーザー] /ユーザーを編集*

キーストアの作成を求めるプロンプトが表示されたら、作成します。 このキーストアはAEMにのみ存在し、opensslで作成されたキーストアではありません。 パスワードは任意のものにすることができ、[!DNL openssl]コマンドで使用するパスワードと同じである必要はありません。

### キーストア{#install-the-private-key-via-the-keystore}を使用して秘密鍵をインストールします。

![AEM Userで秘密鍵を追](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL 加] / [!UICONTROL キーストア] /キースト [!UICONTROL アから秘密鍵を追加]*

ユーザーのキーストアコンソールで、「 **[!UICONTROL 秘密鍵をキーストアファイル]**&#x200B;から追加 」をクリックし、次の情報を追加します。

* **[!UICONTROL 新しいエイリアス]**:AEMでのキーのエイリアス。これは何でもかまいません。opensslコマンドで作成したキーストアの名前と対応している必要はありません。
* **[!UICONTROL キーストアファイル]**:openssl pkcs12コマンドの出力(keystore.p12)
* **[!UICONTROL キーストアファイルのパスワード]**:引数を介してopenssl pkcs12コマンドに設定されるパスワ `-passout` ード。
* **[!UICONTROL 秘密鍵のエイリアス]**:上記のopenssl pkcs12コ `-name` マンドで引数に指定した値( `my-key`)をクリックします。
* **[!UICONTROL 秘密鍵のパスワード]**:引数を介してopenssl pkcs12コマンドに設定されるパスワ `-passout` ード。

>[!CAUTION]
>
>キーストアファイルのパスワードと秘密鍵のパスワードは、両方の入力で同じです。 パスワードが一致しない場合は、キーがインポートされません。

### 秘密鍵がAEMキーストア{#verify-the-private-key-is-loaded-into-the-aem-keystore}に読み込まれていることを確認します。

![AEM Userでの秘密鍵の](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL 確認] /キース [!UICONTROL トア]*

秘密鍵が指定されたキーストアからAEMキーストアに正常に読み込まれると、その秘密鍵のメタデータがユーザーのキーストアコンソールに表示されます。

## Adobe I/O{#adding-the-public-key-to-adobe-i-o}への公開鍵の追加

対応する公開鍵を持つAEMサービスユーザーが安全に通信できるように、一致する公開鍵をAdobe I/Oにアップロードする必要があります。

### Adobe I/Oの新しい統合{#create-a-adobe-i-o-new-integration}を作成します。

![新しい統合のAdobe I/Oの作成](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Adobe I/O統合の作成]](https://console.adobe.io/) /新 [!UICONTROL しい統合]*

新しい統合をAdobe I/Oで作成するには、公開証明書をアップロードする必要があります。 `openssl req`コマンドで生成された&#x200B;**certificate.crt**&#x200B;をアップロードします。

### 公開鍵がAdobe I/O{#verify-the-public-keys-are-loaded-in-adobe-i-o}に読み込まれていることを確認します。

![Adobe I/O内の公開鍵の検証](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

インストールされた公開鍵とその有効期限は、Adobe I/Oの[!UICONTROL 統合]コンソールに表示されます。「**[!UICONTROL 公開鍵を追加]**」ボタンを使用して、複数の公開鍵を追加できます。

AEMは秘密鍵を保持し、Adobe I/O統合は対応する公開鍵を保持するようになり、AEMはAdobe I/Oと安全に通信できます。
