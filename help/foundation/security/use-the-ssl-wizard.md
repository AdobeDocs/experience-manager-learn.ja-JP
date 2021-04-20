---
title: AEMでのSSLウィザードの使用
description: Adobe Experience ManagerのSSLセットアップウィザードを使用して、HTTPS経由で実行するAEMインスタンスを簡単にセットアップできます。
seo-description: Adobe Experience ManagerのSSLセットアップウィザードを使用して、HTTPS経由で実行するAEMインスタンスを簡単にセットアップできます。
version: 6.3, 6,4, 6.5
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Security
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 1%

---


# AEMでのSSLウィザードの使用

Adobe Experience ManagerのSSLセットアップウィザードを使用して、HTTPS経由で実行するAEMインスタンスを簡単にセットアップできます。

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>管理対象環境の場合は、IT部門がCAの信頼できる証明書とキーを提供することが最適です。
>
>自己署名入り証明書は、開発目的でのみ使用します。

## 秘密鍵と自己署名証明書のダウンロード

次のzipファイルには、AEM SSLをlocalhostに設定するために必要な[!DNL DER]ファイルと[!DNL CRT]ファイルが含まれています。これらのファイルはローカル開発目的でのみ使用されます。

[!DNL DER]ファイルと[!DNL CERT]ファイルは便宜上提供され、下の「秘密鍵と自己署名証明書の生成」の手順に従って生成されます。

必要に応じて、証明書のパスフレーズは&#x200B;**admin**&#x200B;です。

localhost — 秘密鍵と自己署名証明書.zip（2028年7月に期限切れ）

[証明書ファイルのダウンロード](assets/use-the-ssl-wizard/certificate.zip)

## 秘密鍵と自己署名証明書の生成

上記のビデオでは、自己署名入り証明書を使用したAEM作成者インスタンスでのSSLの設定と設定を説明しています。 [[!DNL OpenSSL]](https://www.openssl.org/)を使用する次のコマンドは、ウィザードの手順2で使用する秘密鍵と証明書を生成できます。

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
