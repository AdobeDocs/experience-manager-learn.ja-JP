---
title: CDN キャッシュを無効にする方法
description: AEMas a Cloud Serviceの CDN で HTTP 応答のキャッシュを無効にする方法を説明します。
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
duration: 116
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 6%

---

# CDN キャッシュを無効にする方法

AEMas a Cloud Serviceの CDN で HTTP 応答のキャッシュを無効にする方法を説明します。 応答のキャッシュは、 `Cache-Control`, `Surrogate-Control`または `Expires` HTTP 応答キャッシュヘッダー。

これらのキャッシュヘッダーは、通常、`mod_headers` を使用して、AEM Dispatcher のホスト設定で行われますが、AEM パブリッシュ自体で実行されるカスタム Java™ コードで設定することもできます。

## デフォルトのキャッシュ動作

AEM Publish およびオーサー環境で、 [AEM Project Archetype](./enable-caching.md#default-caching-behavior) ベースのAEMプロジェクトがデプロイされます。

## キャッシュを無効にする

キャッシュをオフにすると、AEMas a Cloud Serviceインスタンスのパフォーマンスに悪影響を与える可能性があるので、デフォルトのキャッシュ動作をオフにする場合は注意が必要です。

ただし、キャッシュを無効にする場合は、次のようなシナリオが考えられます。

- 新しい機能を開発する際に、変更をすぐに確認したい場合。
- コンテンツは安全（認証済みユーザー向けのみ）または動的（買い物かご、注文の詳細）です。キャッシュしないでください。

キャッシュを無効にするには、2 つの方法でキャッシュヘッダーを更新します。

1. **Dispatcher vhost の設定：** AEM Publish でのみ使用できます。
1. **カスタム Java™コード：** AEM Publish と Author の両方で使用できます。

これらの各オプションを確認してみましょう。

### Dispatcher vhost の設定

キャッシュを無効にする場合は、このオプションを使用することをお勧めしますが、AEM Publish でのみ使用できます。 キャッシュヘッダーを更新するには、 `mod_headers` モジュールと `<LocationMatch>` Apache HTTP Server の vhost ファイル内のディレクティブ。 一般的な構文は次のとおりです。

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Expires

    # Instructs the CDN to not cache the response.
    Header set Cache-Control "private"
</LocationMatch>
```

#### 例

の CDN キャッシュを無効にするには、以下を実行します。 **CSS コンテンツタイプ** トラブルシューティングの目的に応じて、次の手順に従います。

既存の CSS キャッシュをバイパスするには、CSS ファイルに対して新しいキャッシュキーを生成するために、CSS ファイルに対する変更が必要になることに注意してください。

1. AEMプロジェクト内で、目的のホストファイルを次の場所から見つけます。 `dispatcher/src/conf.d/available_vhosts` ディレクトリ。
1. vhost を更新します ( 例： `wknd.vhost`) ファイルの内容を次に示します。

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the CDN to not cache the response.
       Header set Cache-Control "private"
   </LocationMatch>
   ```

   の vhost ファイル `dispatcher/src/conf.d/enabled_vhosts` ディレクトリは **symlinks** を `dispatcher/src/conf.d/available_vhosts` ディレクトリに含まれていない場合は、必ず symlinks を作成してください。
1. を使用して、目的のAEMas a Cloud Service環境に vhost の変更をデプロイします。 [Cloud Manager - Web 層設定パイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) または [RDE コマンド](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### カスタム Java™コード

このオプションは、AEM Publish と Author の両方で使用できます。 キャッシュヘッダーを更新するには、 `SlingHttpServletResponse` オブジェクトがカスタム Java™コード（Sling サーブレット、Sling サーブレットフィルター）に含まれている。 一般的な構文は次のとおりです。

```java
response.setHeader("Cache-Control", "private");
```
