---
title: AEMでの SSL ウィザードの使用
description: Adobe Experience Managerの SSL セットアップウィザードを使用して、AEMインスタンスを HTTPS 経由で簡単に実行できるようにします。
seo-description: Adobe Experience Manager's SSL setup wizard to make it easier to set up an AEM instance to run over HTTPS.
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
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
source-git-commit: 835c01cb2ad1d154437087c51c70a2daf90493dd
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 1%

---

# AEMでの SSL ウィザードの使用

Adobe Experience Managerの SSL セットアップウィザードを使用して、AEMインスタンスを HTTPS 経由で簡単に実行できるようにします。

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>管理環境の場合、IT 部門が CA に信頼された証明書と鍵を提供することが最適です。
>
>自己署名証明書は、開発目的でのみ使用されます。

## 秘密鍵と自己署名証明書のダウンロード

次の zip ファイルには、ローカルホストでAEM SSL を設定するために必要な [!DNL DER] および [!DNL CRT] ファイルが含まれており、ローカル開発目的でのみ使用されます。

[!DNL DER] ファイルと [!DNL CERT] ファイルは便宜上提供され、以下の「秘密鍵と自己署名証明書の生成」の節で説明する手順を使用して生成されます。

必要に応じて、証明書のパスフレーズは **admin** です。

localhost — 秘密鍵および自己署名証明書.zip （2028 年 7 月に期限切れ）

[証明書ファイルのダウンロード](assets/use-the-ssl-wizard/certificate.zip)

## 秘密鍵と自己署名証明書の生成

上記のビデオでは、自己署名付き証明書を使用したAEMオーサーインスタンス上の SSL の設定と設定を示しています。 [[!DNL OpenSSL]](https://www.openssl.org/) を使用する次のコマンドは、ウィザードの手順 2 で使用する秘密鍵と証明書を生成できます。

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
