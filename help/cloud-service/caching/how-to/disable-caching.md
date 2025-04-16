---
title: CDN キャッシュを無効にする方法
description: AEM as a Cloud Service の CDN で HTTP 応答のキャッシュを無効にする方法を学びます。
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
duration: 100
source-git-commit: cf006f24abbc5aa4b91277b91d68538c41d33e15
workflow-type: ht
source-wordcount: '432'
ht-degree: 100%

---

# CDN キャッシュを無効にする方法

AEM as a Cloud Serviceの CDN で HTTP 応答のキャッシュを無効にする方法を学びます。応答のキャッシュは、`Cache-Control`、`Surrogate-Control`、または `Expires` HTTP 応答キャッシュヘッダーによって制御されます。

これらのキャッシュヘッダーは、通常、`mod_headers` を使用して、AEM Dispatcher のホスト設定で行われますが、AEM パブリッシュ自体で実行されるカスタム Java™ コードで設定することもできます。

## 新しいデフォルトのキャッシュ動作

AEM as a Cloud Service CDN での HTTP 応答のキャッシュは、接触チャネルからの HTTP 応答ヘッダー `Cache-Control`、`Surrogate-Control` または `Expires` によって制御されます。`Cache-Control` に `private`、`no-cache` または `no-store` が含まれる接触チャネル応答はキャッシュされません。

AEM プロジェクトアーキタイプベースの AEM プロジェクトがデプロイされたときの、AEM パブリッシュおよびオーサーでの[デフォルトのキャッシュ動作](./enable-caching.md#default-caching-behavior)を確認します。


## キャッシュを無効にする

キャッシュをオフにすると、AEM as a Cloud Service インスタンスのパフォーマンスに悪影響を与える可能性があるので、デフォルトのキャッシュ動作を無効にする場合は注意が必要です。

ただし、次のようなシナリオで、キャッシュを無効にする場合があります。

- 新しい機能の開発中に、変更をすぐに確認したい場合。
- コンテンツが安全（認証済みユーザーのみを対象）または動的（買い物かご、注文の詳細）で、キャッシュされない場合。

キャッシュを無効にするには、2 つの方法でキャッシュヘッダーを更新します。

1. **Dispatcher vhost の設定：** AEM パブリッシュでのみ使用できます。
1. **カスタム Java™コード：** AEM パブリッシュとオーサーの両方で使用できます。

これらの各オプションを確認してみましょう。

### Dispatcher フィルター設定

キャッシュを無効にする場合は、このオプションを使用することをお勧めしますが、AEM パブリッシュでのみ使用できます。キャッシュヘッダーを更新するには、`mod_headers` モジュールと、Apache HTTP サーバーの vhost ファイル内の `<LocationMatch>` ディレクティブを使用します。一般的な構文は次のとおりです。

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surrogate-Control
    Header unset Expires

    # Instructs the Browser and the CDN to not cache the response.
    Header always set Cache-Control "private"

    # Instructs only the CDN to not cache the response.
    Header always set Surrogate-Control "private"
</LocationMatch>
```

#### 例

トラブルシューティングのために **CSS コンテンツタイプ**&#x200B;の CDN キャッシュを無効にするには、次の手順に従います。

既存の CSS キャッシュをバイパスするには、CSS ファイルに対して新しいキャッシュキーを生成するために、CSS ファイルでの変更が必要になることに注意してください。

1. AEM プロジェクト内で、`dispatcher/src/conf.d/available_vhosts` ディレクトリから目的の vhost ファイルを見つけます。
1. その vhost（例：`wknd.vhost`）ファイルを次のように更新します。

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the Browser and the CDN to not cache the response.
       Header always set Cache-Control "private"
   </LocationMatch>
   ```

   `dispatcher/src/conf.d/enabled_vhosts` ディレクトリ内の vhost ファイルは `dispatcher/src/conf.d/available_vhosts` ディレクトリ内のファイルへの&#x200B;**シンボリックリンク**&#x200B;なので、存在しない場合は、必ずシンボリックリンクを作成してください。
1. [Cloud Manager の web 層設定パイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=ja#web-tier-config-pipelines)または [RDE コマンド](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=ja#deploy-apache-or-dispatcher-configuration)を使用して、目的の AEM as a Cloud Service 環境に vhost の変更をデプロイします。

### カスタム Java™ コード

このオプションは、AEM パブリッシュと AEM オーサーの両方で使用できます。キャッシュヘッダーを更新するには、カスタム Java™ コード（Sling サーブレット、Sling サーブレットフィルター）で `SlingHttpServletResponse` オブジェクトを使用します。一般的な構文は次のとおりです。

```java
response.setHeader("Cache-Control", "private");
```
