---
title: AEM での SSL ウィザードの使用
description: Adobe Experience Manager SSL セットアップウィザードを使用すると、AEM インスタンスを HTTPS 経由で実行するように簡単に設定できます。
seo-description: Adobe Experience Manager's SSL setup wizard to make it easier to set up an AEM instance to run over HTTPS.
version: 6.5, Cloud Service
jira: KT-13465
topics: security, operations
feature: Security
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
source-git-commit: f6e9d1f1991abf34765b28e6e05382a58a6203e3
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 31%

---

# AEM での SSL ウィザードの使用

組み込みの SSL ウィザードを使用して、Adobe Experience Managerで SSL を設定し、HTTPS 経由で実行する方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)


>[!NOTE]
>
>管理環境の場合、IT 部門が CA に信頼された証明書と鍵を提供するのが最適です。
>
>自己署名証明書は、開発目的でのみ使用されます。

## SSL 設定ウィザードの使用

に移動します。 __AEM オーサー/ツール/セキュリティ/SSL 設定__&#x200B;をクリックし、 __SSL 設定ウィザード__.

![SSL 設定ウィザード](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### ストアの資格情報を作成

を作成するには、以下を実行します。 _キーストア_ ～と関連している `ssl-service` システムユーザーとグローバル _トラストストア_&#x200B;を使用する場合は、 __ストア資格情報__ ウィザードの手順です。

1. パスワードを入力し、 __キーストア__ ～と関連している `ssl-service` システムユーザー。
1. グローバルのパスワードを入力し、パスワードを確認します __トラストストア__. これはシステム全体の Trust Store で、既に作成されている場合、入力されたパスワードは無視されます。

   ![SSL 設定 — ストア資格情報](assets/use-the-ssl-wizard/store-credentials.png)

### 秘密鍵と証明書をアップロード

次の手順で _秘密鍵_ および _SSL 証明書_&#x200B;を使用する場合は、 __鍵と証明書__ ウィザードの手順です。

通常、IT 部門が CA 信頼済みの証明書と鍵を提供しますが、自己署名済みの証明書はに使用できます __開発__ および __テスト__ 目的。

自己署名証明書を作成またはダウンロードするには、 [自己署名型秘密鍵および証明書](#self-signed-private-key-and-certificate).

1. をアップロードします。 __秘密鍵__ DER(Distinguished Encoding Rules) 形式の PEM とは異なり、DER でエンコードされたファイルには、次のようなプレーンテキストステートメントは含まれません。 `-----BEGIN CERTIFICATE-----`
1. 関連する __SSL 証明書__ （内） `.crt` 形式を使用します。

   ![SSL の設定 — 秘密鍵と証明書](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### SSL コネクタの詳細の更新

次の手順で _hostname_ および _ポート_ を使用します。 __SSL コネクタ__ ウィザードの手順です。

1. を更新または検証します。 __HTTPS ホスト名__ 値の場合は、 `Common Name (CN)` 証明書から。
1. を更新または検証します。 __HTTPS ポート__ の値です。

   ![SSL の設定 — SSL コネクタの詳細](assets/use-the-ssl-wizard/ssl-connector-details.png)

### SSL 設定の確認

1. SSL を検証するには、 __HTTPS URL に移動__ 」ボタンをクリックします。
1. セル署名済み証明書を使用している場合は、 `Your connection is not private` エラー。

   ![SSL 設定 — HTTPS 経由でのAEMの検証](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## 自己署名型秘密鍵および証明書

次の zip ファイルにはが含まれます。 [!DNL DER] および [!DNL CRT] AEM SSL をローカルで設定するために必要なファイルで、ローカル開発の目的でのみ使用します。

この [!DNL DER] および [!DNL CERT] ファイルは、便宜上提供され、次の「秘密鍵と自己署名証明書を生成する」のセッションで説明されている手順を使用して生成されます。

必要な場合、証明書のパスフレーズは **admin** です。

この localhost — 秘密鍵と自己署名証明書.zip （有効期限は 2028 年 7 月）

[証明書ファイルをダウンロードする](assets/use-the-ssl-wizard/certificate.zip)

### 秘密鍵と自己署名証明書の生成

上記のビデオでは、自己署名付き証明書を使用した AEM オーサーインスタンス上の SSL の設定と構成を示しています。[[!DNL OpenSSL] ](https://www.openssl.org/) を使用して次のコマンドを実行すると、ウィザードの手順 2 で使用する秘密鍵と証明書を生成することができます。

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -extfile <(printf "subjectAltName=DNS:localhost") -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
