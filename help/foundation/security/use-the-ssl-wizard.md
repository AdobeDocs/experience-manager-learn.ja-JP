---
title: AEMでのSSLウィザードの使用
description: Adobe Experience ManagerのSSL設定ウィザードを使用して、AEMで実行するHTTPSインスタンスを簡単に設定できるようにします。
seo-description: Adobe Experience ManagerのSSL設定ウィザードを使用して、AEMで実行するHTTPSインスタンスを簡単に設定できるようにします。
version: 6.3, 6,4, 6.5
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: セキュリティ
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 1%

---


# AEMでのSSLウィザードの使用

Adobe Experience ManagerのSSL設定ウィザードを使用して、AEMで実行するHTTPSインスタンスを簡単に設定できるようにします。

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>管理環境の場合は、IT部門がCAに信頼された証明書とキーを提供することが最適です。
>
>自己署名証明書は、開発目的でのみ使用されます。

## 秘密鍵と自己署名入り証明書のダウンロード

次のzipファイルには、ローカルホストでAEM SSLを設定するために必要な[!DNL DER]および[!DNL CRT]ファイルが含まれており、ローカル開発目的でのみ使用されます。

[!DNL DER]ファイルと[!DNL CERT]ファイルは、便宜上提供され、以下の秘密鍵と自己署名証明書の生成の節で説明する手順を使用して生成されます。

必要に応じて、証明書のパスフレーズは&#x200B;**admin**&#x200B;になります。

localhost — 秘密鍵と自己署名証明書.zip （2028年7月に期限切れ）

[証明書ファイルのダウンロード](assets/use-the-ssl-wizard/certificate.zip)

## 秘密鍵と自己署名証明書の生成

上記のビデオでは、自己署名付き証明書を使用したAEMオーサーインスタンス上のSSLの設定を図式化しています。 [[!DNL OpenSSL]](https://www.openssl.org/)を使用する次のコマンドでは、ウィザードの手順2で使用する秘密鍵と証明書を生成できます。

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
