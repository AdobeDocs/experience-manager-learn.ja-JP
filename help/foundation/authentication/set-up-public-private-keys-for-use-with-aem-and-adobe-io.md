---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: AEMは、公開/秘密鍵のペアを使用して、Adobe I/Oや他の Web サービスと安全に通信します。 この短いチュートリアルでは、AEMとAdobe I/Oの両方で動作する openssl コマンドラインツールを使用して、互換性のあるキーとキーストアを生成する方法を説明します。
version: 6.4, 6.5
feature: User and Groups
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
topic: Development
role: Developer
level: Experienced
exl-id: 62ed9dcc-6b8a-48ff-8efe-57dabdf4aa66
last-substantial-update: 2022-07-17T00:00:00Z
thumbnail: KT-2450.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '763'
ht-degree: 0%

---

# Adobe I/Oで使用する公開鍵と秘密鍵を設定

AEMは、公開/秘密鍵のペアを使用して、Adobe I/Oや他の Web サービスと安全に通信します。 この短いチュートリアルでは、 [!DNL openssl] AEMとAdobe I/Oの両方で機能するコマンドラインツール。

>[!CAUTION]
>
>このガイドでは、開発や低レベルの環境での使用に役立つ自己署名付きキーを作成します。 通常、本番シナリオでは、キーは組織の IT セキュリティチームが生成し、管理します。

## 公開鍵と秘密鍵のペアを生成します {#generate-the-public-private-key-pair}

この [[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) コマンドラインツール [[!DNL req] command](https://www.openssl.org/docs/man1.0.2/man1/req.html) を使用して、Adobe I/OおよびAdobe Experience Managerと互換性のあるキーペアを生成できます。

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

次の手順で [!DNL openssl generate] コマンドを使用します。要求された場合は、証明書情報を入力します。 Adobe I/OとAEMは、これらの値が何であるかに関係ありませんが、値を一致させ、鍵を説明する必要があります。

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

## 新しいキーストアにキーペアを追加 {#add-key-pair-to-a-new-keystore}

新しい [!DNL PKCS12] キーストア。 の一部として [[!DNL openssl]'s [!DNL pcks12] コマンド](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) キーストアの名前 ( `-  caname`)、キーの名前 ( `-name`) とキーストアのパスワード ( `-  passout`) が定義されている場合にのみ使用できます。

これらの値は、キーストアとキーをAEMに読み込むために必要です。

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

このコマンドの出力は、 `keystore.p12` ファイル。

>[!NOTE]
>
>のパラメーター値 **[!DNL my-keystore]**, **[!DNL my-key]** および **[!DNL my-password]** を独自の値に置き換えます。

## キーストアの内容を確認します {#verify-the-keystore-contents}

Java™ [[!DNL keytool] コマンドラインツール](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) キーストアを表示して、キーストアファイル ([!DNL keystore.p12]) をクリックします。

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![キーストアをAdobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## AEMへのキーストアの追加 {#adding-the-keystore-to-aem}

AEMは生成された **秘密鍵** Adobe I/Oや他のウェブサービスと安全に通信する。 秘密鍵をAEMからアクセスできるようにするには、AEMユーザーのキーストアに秘密鍵をインストールする必要があります。

に移動します。 **AEM > [!UICONTROL ツール] > [!UICONTROL セキュリティ] > [!UICONTROL ユーザー]** および **ユーザーの編集** 秘密鍵は、に関連付けられます。

### AEMキーストアの作成 {#create-an-aem-keystore}

![AEMでキーストアを作成](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEM > [!UICONTROL ツール] > [!UICONTROL セキュリティ] > [!UICONTROL ユーザー] /ユーザーを編集*

キーストアを作成するように求められたら、作成します。 このキーストアはAEMにのみ存在し、openssl で作成されたキーストアではありません。 パスワードは任意のものにすることができ、 [!DNL openssl] コマンドを使用します。

### キーストアを使用した秘密鍵のインストール {#install-the-private-key-via-the-keystore}

![AEMに秘密鍵を追加](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL ユーザー] > [!UICONTROL キーストア] > [!UICONTROL キーストアから秘密鍵を追加]*

ユーザーのキーストアコンソールで、 **[!UICONTROL 秘密鍵をキーストアファイルから追加]** 次の情報を追加します。

* **[!UICONTROL 新規エイリアス]**:AEMでのキーのエイリアス。 これは何でもかまいません。また、openssl コマンドで作成したキーストアの名前に対応している必要はありません。
* **[!UICONTROL キーストアファイル]**:openssl pkcs12 コマンドの出力 (keystore.p12)
* **[!UICONTROL キーストアファイルのパスワード]**:を介して openssl pkcs12 コマンドに設定されたパスワード `-passout` 引数。
* **[!UICONTROL 秘密鍵のエイリアス]**:に指定された値 `-name` 上記の openssl pkcs12 コマンドの引数 ( `my-key`) をクリックします。
* **[!UICONTROL 秘密鍵のパスワード]**:を介して openssl pkcs12 コマンドに設定されたパスワード `-passout` 引数。

>[!CAUTION]
>
>キーストアファイルのパスワードと秘密鍵のパスワードは、両方の入力で同じです。 パスワードが一致しない場合は、キーがインポートされません。

### 秘密鍵がAEMキーストアに読み込まれていることを確認します。 {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![AEMで秘密鍵を検証](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL ユーザー] > [!UICONTROL キーストア]*

秘密鍵が指定されたキーストアからAEMキーストアに正常に読み込まれると、その秘密鍵のメタデータがユーザーのキーストアコンソールに表示されます。

## 公開鍵のAdobe I/Oへの追加 {#adding-the-public-key-to-adobe-i-o}

AEMサービスユーザーが、対応する秘密鍵を持ち、安全に通信できるように、一致する公開鍵をAdobe I/Oにアップロードする必要があります。

### 新しい統合のAdobe I/O作成 {#create-a-adobe-i-o-new-integration}

![新しい統合のAdobe I/O作成](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Adobe I/O統合を作成]](https://developer.adobe.com/console/) > [!UICONTROL 新しい統合]*

統合で統合を作成するには、Adobe I/Oの公開証明書をアップロードする必要があります。 をアップロードします。 **certificate.crt** 生成元 `openssl req` コマンドを使用します。

### 公開鍵がAdobe I/Oで読み込まれていることを確認する {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![「Verify Public Keys in」Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

インストールされた公開鍵とその有効期限は、 [!UICONTROL 統合] Adobe I/Oのコンソール複数の公開鍵を **[!UICONTROL 公開鍵を追加]** 」ボタンをクリックします。

AEMは秘密鍵を保持し、Adobe I/O統合は対応する公開鍵を保持するようになり、AEMはAdobe I/Oと安全に通信できます。
