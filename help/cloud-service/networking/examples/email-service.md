---
title: メールサービス
description: エグレスポートを使用してメールサービスに接続するように AEM as a Cloud Service を設定する方法を説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
duration: 76
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '334'
ht-degree: 100%

---

# メールサービス

AEM の `DefaultMailService` を高度なネットワークエグレスポートを使用するように設定して、AEM as a Cloud Service からメールを送信します。

（ほとんどの）メールサービスは HTTP／HTTPS 経由で実行されないので、AEM as a Cloud Service からメールサービスへの接続はプロキシ化する必要があります。

+ `smtp.host` は OSGi 環境変数 `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` に設定されているので、エグレスを通過します。
   + `$[env:AEM_PROXY_HOST]` は、AEM as a Cloud Service が内部 `proxy.tunnel` ホストにマッピングする予約変数です。
   + `AEM_PROXY_HOST` を Cloud Manager 経由で設定しようとしないでください。
+ `smtp.port` は、宛先のメールサービスのホストおよびポートにマッピングする `portForward.portOrig` ポートに設定されています。この例では、次のマッピングを使用します。`AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`
   + `smpt.port` は `portForward.portOrig` ポートに設定されています。SMTP サーバーの実際のポートではありません。`smtp.port` と `portForward.portOrig` ポート間のマッピングは Cloud Manager の `portForwards` ルールによって確立されます（実際の例が後述されています）。

秘密鍵はコードに格納できないので、メールサービスのユーザー名とパスワードは[秘密鍵の OSGi 設定変数](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=ja#secret-configuration-values)を使用して指定するのが最善で、AIO CLI または Cloud Manager API を使用して設定されます。

通常、[フレキシブルポートエグレス](../flexible-port-egress.md)は、メールサービスとの統合を満たすために使用されます。ただし、Adobe IP を `allowlist` することが必要な場合は別で、この場合は[専用のエグレス IP アドレス](../dedicated-egress-ip-address.md)を使用できます。

さらに、[メールの送信](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html?lang=ja#sending-email)に関する AEM ドキュメントを確認します。

## 高度なネットワーク機能のサポート

次のコードサンプルは、以下の高度なネットワーク機能オプションでサポートされています。

このチュートリアルを開始する前に、高度なネットワーク機能の設定が[適切](../advanced-networking.md#advanced-networking)にセットアップされていることを確認してください。

| 高度なネットワーク機能なし | [柔軟なポートエグレス](../flexible-port-egress.md) | [専用エグレス IP アドレス](../dedicated-egress-ip-address.md) | [仮想プライベートネットワーク](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi 設定

この OSGi 設定の例では、以下の Cloud Manager の [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration?lang=ja) 操作の `portForwards` ルールを使用する方法によって、AEM の Mail OSGi Service を外部のメールサービスを使用するように設定しています。

```json
...
"portForwards": [{
    "name": "smtp.mymail.com",
    "portDest": 465,
    "portOrig": 30465
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

AEM の [DefaultMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html?lang=ja#sending-email) を、メールプロバイダーに要求されている通りに設定します（例：`smtp.ssl` など）。

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30465",
    "smtp.user": "$[env:EMAIL_USERNAME;default=myApiKey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

`EMAIL_USERNAME` および `EMAIL_PASSWORD` OSGi 変数と秘密鍵は、次のいずれかを使用して、環境ごとに設定できます。

+ [Cloud Manager の環境設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=ja)
+ または `aio CLI` コマンドの使用

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```
