---
title: AEM GraphQL の CORS 設定
description: AEM GraphQLで使用するためのクロスオリジンリソース共有（CORS）の設定方法について説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
last-substantial-update: 2024-03-22T00:00:00Z
duration: 215
source-git-commit: 1c967b450b8539c571799c293ff0e7b3d635cc34
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 100%

---

# クロスオリジンリソース共有（CORS）

Adobe Experience Manager as a Cloud Service のクロスオリジンリソース共有（CORS）は、AEM 以外の web プロパティが、AEM の GraphQL API およびその他の AEM ヘッドレスリソースに対してブラウザーベースのクライアントサイド呼び出しを容易に行えるようにします。

>[!TIP]
>
> 設定例は次のようになります。プロジェクトの要件に合わせて調整してください。

## CORS 要件

AEM に接続するクライアントが AEM と同じオリジン（ホストまたはドメインとも呼ばれる）から提供されない場合、AEM GraphQL API へのブラウザーベースの接続には、CORS が必要です。

| クライアントタイプ | [シングルページアプリ（SPA）](../spa.md) | [Web コンポーネント／JS](../web-component.md) | [モバイル](../mobile.md) | [サーバー間](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| CORS 設定が必要 | ✔ | ✔ | ✘ | ✘ |

## AEM オーサー

AEM オーサーサービスでの CORS の有効化は、AEM パブリッシュサービスおよび AEM プレビューサービスの場合とは異なります。AEM オーサーサービスでは、OSGi 設定を AEM オーサーサービスの実行モードフォルダーに追加する必要があり、Dispatcher 設定は使用しません。

### OSGi 設定

AEM CORS OSGi 設定ファクトリは、CORS HTTP リクエストを受け入れるための許可条件を定義します。

| クライアントの接続先 | AEM オーサー | AEM パブリッシュ | AEM プレビュー |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| CORS OSGi 設定が必要 | ✔ | ✘ | ✘ |


次の例は、AEM オーサーの OSGi 設定（`../config.author/..`）を定義し、AEM オーサーサービス上でのみアクティブです。

主な設定プロパティは次のとおりです。

+ `alloworigin` や `alloworiginregexp` は AEM web に接続するクライアントの実行元を指定します。
+ `allowedpaths` は指定したオリジンから許可される URL パスパターンを指定します。
   + AEM GraphQLで永続化クエリをサポートするには、パターン「`/graphql/execute.json.*`」を追加します。
   + エクスペリエンスフラグメントをサポートするには、パターン「`/content/experience-fragments/.*`」を追加します。
+ `supportedmethods` は、CORS リクエストで許可される HTTP メソッドを指定します。 AEM GraphQL 永続クエリ（およびエクスペリエンスフラグメント）をサポートするには、`GET` を追加します。
+ AEM オーサーへのリクエストとして `"Authorization"` を含む `supportedheaders` は、認証を受ける必要があります。
+ AEM オーサーへのリクエストとして `true` に設定されている `supportscredentials` は、認証を受ける必要があります。

[ CORS OSGi の設定について詳しくは、こちらを参照してください。](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=ja)

次の例では、 AEM オーサーでの AEM GraphQL 永続クエリの使用をサポートしています。クライアント定義の GraphQL クエリを使用するには、`allowedpaths` および `POST` からの GraphQL エンドポイント URL を `supportedmethods` に追加します。

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
  ],
  "allowedpaths": [
    "/graphql/execute.json.*",
    "/content/experience-fragments/.*"
  ],
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "POST"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": true,
  "exposedheaders":[ "" ]
}
```

#### OSGi 設定の例

+ [OSGi 設定の例は、WKND プロジェクトで確認できます。](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## AEM パブリッシュ

AEM パブリッシュ（およびプレビュー）サービスで CORS を有効にすることは、AEM オーサーサービスとは異なります。AEM パブリッシュサービスでは、AEM Dispatcher 設定を AEM パブリッシュの Dispatcher 設定に追加する必要があります。AEM パブリッシュは、[OSGi 設定](#osgi-configuration)を使用しません。

AEM パブリッシュで CORS を設定する際には、次の点を確認します。

+ `Origin` HTTP リクエストヘッダーは、AEM Dispatcher プロジェクトの `clientheaders.any` ファイルから `Origin` ヘッダー（以前に追加されていた場合）を削除すると、AEM パブリッシュサービスに送信できなくなります。`Access-Control-` ヘッダーは、`clientheaders.any` ファイルから削除する必要があり、AEM パブリッシュサービスではなく Dispatcher がこれらを管理します。
+ AEM パブリッシュサービスで [CORS OSGi 設定](#osgi-configuration)が有効になっている場合は、これらを削除し、その設定を以下で説明する [Dispatcher vhost 設定](#set-cors-headers-in-vhost)に移行する必要があります。

### Dispatcher 設定

AEM パブリッシュ（およびプレビュー）サービスの Dispatcher は、CORS をサポートするように設定する必要があります。

| クライアントの接続先 | AEM オーサー | AEM パブリッシュ | AEM プレビュー |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher の CORS 設定が必要 | ✘ | ✔ | ✔ |

#### vhost での CORS ヘッダーの設定

1. Dispatcher 設定プロジェクトで、AEM パブリッシュサービスの vhost 設定ファイルを開きます。通常は、`dispatcher/src/conf.d/available_vhosts/<example>.vhost` にあります。
2. 以下の `<IfDefine ENABLE_CORS>...</IfDefine>` ブロックの内容を、有効な vhost 設定ファイルにコピーします。

   ```{ highlight="17"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
         ################## Start of CORS configuration ##################
   
         SetEnvIfExpr "req_novary('Origin') == ''" CORSType=none CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') != ''" CORSType=cors CORSProcessing=true CORSTrusted=false
   
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') == '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=invalidpreflight CORSProcessing=false
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') != '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=preflight CORSProcessing=true CORSTrusted=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch 'http://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch 'https://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
   
         # For requests that require CORS processing, check if the Origin can be trusted
         SetEnvIfExpr "%{HTTP_HOST} =~ /(.*)/ " ParsedHost=$1
   
         ################## Adapt regex to match CORS origin(s) for your environment
         SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.your-domain\.tld(:\d+)?$)#" CORSTrusted=true
   
         # Extract the Origin header
         SetEnvIfNoCase ^Origin$ ^(.*)$ CORSTrustedOrigin=$1
   
         # Flush If already set
         Header unset Access-Control-Allow-Origin
         Header unset Access-Control-Allow-Credentials
   
         # Trusted
         Header always set Access-Control-Allow-Credentials "true" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Origin "%{CORSTrustedOrigin}e" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Methods "GET" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Max-Age 1800 "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Headers "Origin, Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers" "expr=reqenv('CORSTrusted') == 'true'"
   
         # Non-CORS or Not Trusted
         Header unset Access-Control-Allow-Credentials "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Origin "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Methods "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Max-Age "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
   
         # Always vary on origin, even if its not there.
         Header merge Vary Origin
   
         # CORS - send 204 for CORS requests which are not trusted
         RewriteCond expr "reqenv('CORSProcessing') == 'true' && reqenv('CORSTrusted') == 'false'"
         RewriteRule "^(.*)" - [R=204,L]
   
         # Remove Origin before sending to AEM Publish if this configuration handles the CORS
         RequestHeader unset Origin "expr=reqenv('CORSTrusted') == 'true'"
   
         ################## End of CORS configuration ##################
     </IfModule>
     ...
   </VirtualHost>
   ```

3. 以下の行の正規表現を更新して、AEM パブリッシュサービスにアクセスする目的の接触チャネルと一致させます。複数の接触チャネルが必要な場合は、この行を複製し、接触チャネル／接触チャネルパターンごとに更新します。

   ```
   SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*.your-domain.tld(:\d+)?$)#" CORSTrusted=true
   ```

   + 例えば、接触チャネルからの CORS アクセスを有効にするには：

      + `https://example.com` の任意のサブドメイン
      + `http://localhost` 上の任意のポート

     この行を次の 2 行に置き換えます。

     ```
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.example\.com$)#" CORSTrusted=true
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(http://localhost(:\d+)?$)#" CORSTrusted=true
     ```

#### Dispatcher 設定の例

+ [Dispatcher 設定の例は、WKND プロジェクトで確認できます。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost)
