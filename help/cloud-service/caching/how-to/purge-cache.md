---
title: CDN キャッシュをパージする方法
description: AEM as a Cloud Serviceの CDN からキャッシュされた HTTP 応答をパージまたは削除する方法について説明します。
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-08-13T00:00:00Z
jira: KT-15963
thumbnail: KT-15963.jpeg
exl-id: 5d81f6ee-a7df-470f-84b9-12374c878a1b
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '892'
ht-degree: 2%

---

# CDN キャッシュをパージする方法

AEM as a Cloud Serviceの CDN からキャッシュされた HTTP 応答をパージまたは削除する方法について説明します。 **Purge API Token** と呼ばれるセルフサービス機能を使用して、特定のリソース、リソースのグループ、キャッシュ全体のキャッシュをパージできます。

このチュートリアルでは、Purge API Token を設定および使用して、セルフサービス機能を使用してサンプル [AEM WKND](https://github.com/adobe/aem-guides-wknd) サイトの CDN キャッシュをパージする方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3432948?quality=12&learn=on)

## キャッシュの無効化と明示的なパージ

CDN からキャッシュされたリソースを削除する方法は 2 つあります。

1. **キャッシュの無効化：** これは、`Cache-Control`、`Surrogate-Control`、`Expires` などのキャッシュヘッダーに基づいて CDN からキャッシュされたリソースを削除するプロセスです。 キャッシュヘッダーの `max-age` 属性値は、リソースのキャッシュ有効期間（キャッシュ TTL （有効期間））を決定するために使用されます。 キャッシュの有効期限が切れると、キャッシュされたリソースは CDN キャッシュから自動的に削除されます。

1. **明示的パージ：** TTL の有効期限が切れる前に、キャッシュされたリソースを CDN キャッシュから手動で削除するプロセスです。 明示的なパージは、キャッシュされたリソースをすぐに削除する場合に便利です。 ただし、オリジンサーバーへのトラフィックが増加します。

キャッシュされたリソースが CDN キャッシュから削除されると、同じリソースに対する次のリクエストがオリジンサーバーから最新バージョンを取得します。

## パージ API トークンの設定

CDN キャッシュをパージするためのパージ API トークンを設定する方法を説明します。

### CDN ルールの設定

パージ API トークンは、AEM プロジェクトコードに CDN ルールを設定することで作成されます。

1. AEM プロジェクトのメイン `config` フォルダーから `cdn.yaml` ファイルを開きます。 例えば、[WKND プロジェクトの cdn.yaml](https://github.com/adobe/aem-guides-wknd/blob/main/config/cdn.yaml) ファイルです。

1. `cdn.yaml` ファイルに次の CDN ルールを追加します。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:  
  authentication: # The main authentication configuration
    authenticators: # The list of authenticators
       - name: purge-auth # The name of the authenticator
         type: purge  # The type of the authenticator, must be purge
         purgeKey1: ${{CDN_PURGEKEY_081324}} # The first purge key, must be referenced by the Cloud Manager secret-type environment variable name ${{CDN_EDGEKEY_073124}}
         purgeKey2: ${{CDN_PURGEKEY_111324}} # The second purge key, must be referenced by the Cloud Manager secret-type environment variable name ${{CDN_EDGEKEY_111324}}. It is used for the rotation of secrets without any interruptions.
    rules: # The list of authentication rules
       - name: purge-auth-rule # The name of the rule
         when: { reqProperty: tier, equals: "publish" } # The condition when the rule should be applied
         action: # The action to be taken when the rule is applied
           type: authenticate # The type of the action, must be authenticate
           authenticator: purge-auth # The name of the authenticator to be used, must match the name from the above authenticators list               
```

1. 変更内容を保存、コミットし、Adobeのアップストリームリポジトリーにプッシュします。

### Cloud Manager環境変数の作成

次に、Cloud Manager環境変数を作成して、Purge API Token の値を格納します。

1. [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) で Cloud Manager にログインし、組織とプログラムを選択します。

1. 「__環境__」セクションで、目的の環境の横にある **省略記号** （...）をクリックし、「**詳細を表示**」を選択します。

   ![詳細を表示](../assets/how-to/view-env-details.png)

1. 次に、「**設定**」タブを選択し、「**設定を追加**」ボタンをクリックします。

1. **環境設定** ダイアログで、次の詳細を入力します。
   - **名前**：環境変数の名前を入力します。 `cdn.yaml` ファイルの `purgeKey1` または `purgeKey2` の値と一致する必要があります。
   - **値**：パージ API トークンの値を入力します。
   - **適用されるサービス**:「**すべて**」オプションを選択します。
   - **タイプ**:「**秘密鍵**」オプションを選択します。
   - 「**追加**」ボタンをクリックします。

   ![変数を追加](../assets/how-to/add-cloud-manager-secrete-variable.png)

1. 上記の手順を繰り返して、`purgeKey2` 値の 2 番目の環境変数を作成します。

1. 「**保存**」をクリックして、変更を保存して適用します。

### CDN ルールのデプロイ

最後に、Cloud Manager パイプラインを使用して、設定済みの CDN ルールをAEM as a Cloud Service環境にデプロイします。

1. Cloud Managerで、「**パイプライン** セクションに移動します。

1. 新しいパイプラインを作成するか、**Config** ファイルのみをデプロイする既存のパイプラインを選択します。 詳しい手順については、[ 設定パイプラインの作成 ](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager) を参照してください。

1. 「**実行**」ボタンをクリックして、CDN ルールをデプロイします。

   ![ パイプラインを実行 ](../assets/how-to/run-config-pipeline.png)

## Purge API トークンの使用

CDN キャッシュをパージするには、Purge API トークンでAEM サービス固有のドメイン URL を呼び出します。 キャッシュをパージする構文を次に示します。

```
PURGE <URL> HTTP/1.1
Host: <AEM_SERVICE_SPECIFIC_DOMAIN>
X-AEM-Purge-Key: <PURGE_API_TOKEN>
X-AEM-Purge: <PURGE_TYPE>
Surrogate-Key: <SURROGATE_KEY>
```

条件：

- **PURGE`<URL>`**:`PURGE` メソッドの後に、パージするリソースの URL パスが続きます。
- **Host:`<AEM_SERVICE_SPECIFIC_DOMAIN>`**:AEM サービスのドメインを指定します。
- **X-AEM-Purge-Key:`<PURGE_API_TOKEN>`**：パージ API トークンの値を含むカスタムヘッダーです。
- **X-AEM-Purge:`<PURGE_TYPE>`**：消去操作のタイプを指定するカスタムヘッダー。 値は、`hard`、`soft`、`all` のいずれかです。 次の表に、各パージタイプを示します。

  | 消去タイプ | 説明 |
  |:------------:|:-------------:|
  | ハード（デフォルト） | キャッシュされたリソースを直ちに削除します。 オリジンサーバーへのトラフィックが増加するため、この方法は使用しないでください。 |
  | ソフト | キャッシュされたリソースを古いものとしてマークし、オリジンサーバーから最新バージョンを取得します。 |
  | すべて | キャッシュされたすべてのリソースを CDN キャッシュから削除します。 |

- **Surrogate-Key:`<SURROGATE_KEY>`**:（オプション）パージするリソースグループのサロゲートキー（スペース区切り）を指定するカスタムヘッダー。 サロゲートキーはリソースをグループ化するために使用され、リソースの応答ヘッダーで設定する必要があります。

>[!TIP]
>
>以下の例では、`X-AEM-Purge: hard` がデモ目的で使用されています。 必要に応じて、`soft` または `all` に置き換えることができます。 `hard` パージタイプを使用する場合は、オリジンサーバーへのトラフィックが増加するので注意が必要です。

### 特定のリソースのキャッシュをパージする

この例では、`curl` コマンドを使用して、AEM as a Cloud Service環境にデプロイされた WKND サイト上の `/us/en.html` リソースのキャッシュをパージします。

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com/us/en.html" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: hard"
```

パージが成功した場合、`200 OK` 応答が JSON コンテンツと共に返されます。

```json
{ "status": "ok", "id": "1000098-1722961031-13237063" }
```

### リソースのグループのキャッシュのパージ

この例では、`curl` コマンドを使用して、サロゲート キー `wknd-assets` を持つリソースのグループのキャッシュをパージします。 `Surrogate-Key` 応答ヘッダーは [`wknd.vhost`](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L176) に設定されます。次に例を示します。

```http
<VirtualHost *:80>
    ...

    # Core Component Image Component: long-term caching (30 days) for immutable URLs, background refresh to avoid MISS
    <LocationMatch "^/content/.*\.coreimg.*\.(?i:jpe?g|png|gif|svg)$">
        Header set Cache-Control "max-age=2592000,stale-while-revalidate=43200,stale-if-error=43200,public,immutable" "expr=%{REQUEST_STATUS} < 400"
        # Set Surrogate-Key header to group the cache of WKND assets, thus it can be flushed independtly
        Header set Surrogate-Key "wknd-assets"
        Header set Age 0
    </LocationMatch>

    ...
</VirtualHost>
```

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com" \
-H "Surrogate-Key: wknd-assets" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: hard"
```

パージが成功した場合、`200 OK` 応答が JSON コンテンツと共に返されます。

```json
{ "wknd-assets": "10027-1723478994-2597809-1" }
```

### キャッシュ全体をパージ

この例では、`curl` コマンドを使用して、AEM as a Cloud Service環境にデプロイされたサンプル WKND サイトからキャッシュ全体をパージします。

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com/" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: all"
```

パージが成功した場合、`200 OK` 応答が JSON コンテンツと共に返されます。

```json
{"status":"ok"}
```

### キャッシュパージの検証

キャッシュパージを確認するには、web ブラウザーでリソース URL にアクセスし、応答ヘッダーを確認します。 `X-Cache` ヘッダー値は `MISS` にする必要があります。

![X-Cache ヘッダー ](../assets/how-to/x-cache-miss.png)
