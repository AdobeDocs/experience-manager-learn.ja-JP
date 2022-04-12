---
title: Email サービス[Email さーびす]
description: 出力ポートを使用して電子メールサービスに接続するようにAEM as a Cloud Serviceを設定する方法を説明します。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
source-git-commit: d00e47895d1b2b6fb629b8ee9bcf6b722c127fd3
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 1%

---

# Email サービス[Email さーびす]

AEMを設定してAEM as a Cloud Serviceから電子メールを送信 `DefaultMailService` 高度なネットワーク出力ポートを使用する場合。

（ほとんどの）メールサービスは HTTP/HTTPS 経由で実行されないので、AEM as a Cloud Serviceからのメールサービスへの接続をプロキシ化する必要があります。

+ `smtp.host` が OSGi 環境変数に設定されている `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` したがって、出力を通過します。
+ `smtp.port` が `portForward.portOrig` 宛先の電子メールサービスのホストおよびポートにマッピングするポート。 この例では、マッピングを使用します。 `AEM_PROXY_HOST:30002` → `smtp.sendgrid.com:465`.

秘密鍵はコードに格納できないので、電子メールサービスのユーザー名とパスワードは、 [シークレットの OSGi 設定変数](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values)、AIO CLI または Cloud Manager API を使用して設定します。

通常、 [フレキシブルポートエグレス](../flexible-port-egress.md) は、電子メールサービスとの統合を満たすために使用されます ( `allowlist` AdobeIP( この場合は [出力専用 ip アドレス](../dedicated-egress-ip-address.md) を使用できます。

## 高度なネットワークサポート

次のコード例は、次のアドバンスドネットワークオプションでサポートされています。

次を確認します。 [適切](../advanced-networking.md#advanced-networking) このチュートリアルに従う前に、高度なネットワーク設定が設定されています。

| 高度なネットワークがありません | [柔軟なポート出力](../flexible-port-egress.md) | [出力専用 IP アドレス](../dedicated-egress-ip-address.md) | [仮想プライベートネットワーク](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi 設定

次の OSGi 設定例では、次の Cloud Manager を使用して、外部のメールサービスを使用するようにAEM Mail OSGi Service を設定しています。 `portForwards` ルール [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 操作。

```json
...
"portForwards": [{
    "name": "smtp.mymail.com",
    "portDest": 465,
    "portOrig": 30002
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

AEMを設定 [DefaultMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) 電子メールプロバイダーの要求に応じて ( 例： `smtp.ssl`など )

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30002",
    "smtp.user": "$[env:EMAIL_USERNAME;default=emailapikey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

以下 `aio CLI` コマンドを使用して、環境ごとに OSGi シークレットを設定できます。

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "apikey" --secret EMAIL_PASSWORD "password123"
```
