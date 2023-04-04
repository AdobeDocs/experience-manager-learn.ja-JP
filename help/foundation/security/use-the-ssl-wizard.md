---
title: AEMでの SSL ウィザードの使用
description: Adobe Experience Manager SSL セットアップウィザードを使用して、AEM経由で実行するインスタンスを簡単に設定できるようにします。
seo-description: Adobe Experience Manager's SSL setup wizard to make it easier to set up an AEM instance to run over HTTPS.
version: 6.4, 6.5
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# AEMでの SSL ウィザードの使用

Adobe Experience Manager SSL セットアップウィザードを使用して、AEM経由で実行するインスタンスを簡単に設定できるようにします。

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)

を開きます。 __SSL 設定ウィザード__ は、次の場所に移動して直接開くことができます。 __AEM オーサー/ツール/セキュリティ/SSL 設定__.

>[!NOTE]
>
>管理環境の場合、IT 部門が CA に信頼された証明書と鍵を提供するのが最適です。
>
>自己署名証明書は、開発目的でのみ使用されます。

## 秘密鍵と自己署名証明書のダウンロード

次の zip ファイルにはが含まれます。 [!DNL DER] および [!DNL CRT] ローカルホストでAEM SSL を設定するために必要なファイルで、ローカル開発目的でのみ使用されます。

この [!DNL DER] および [!DNL CERT] ファイルは、便宜上提供され、以下の「秘密鍵と自己署名証明書を生成する」の節で説明されている手順を使用して生成されます。

必要に応じて、証明書のパスフレーズはになります。 **admin**.

localhost — 秘密鍵と自己署名証明書.zip （有効期限：2028 年 7 月）

[証明書ファイルをダウンロードします](assets/use-the-ssl-wizard/certificate.zip)

## 秘密鍵と自己署名証明書の生成

上記のビデオでは、自己署名付き証明書を使用したAEMオーサーインスタンス上の SSL の設定と設定を示しています。 以下のコマンドは、 [[!DNL OpenSSL]](https://www.openssl.org/) では、ウィザードの手順 2 で使用する秘密鍵と証明書を生成できます。

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
