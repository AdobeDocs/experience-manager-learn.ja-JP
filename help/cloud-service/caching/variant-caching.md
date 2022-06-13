---
title: ページのバリアントのキャッシュ (AEMas a Cloud Service)
description: ページのバリアントのキャッシュをサポートするためにAEM as a cloud service を設定および使用する方法について説明します。
role: Architect, Developer
topic: Development
feature: CDN Cache, Dispatcher
source-git-commit: 7ca34c2fe14ffb944746e9ae1171bbf2309e3c9d
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 2%

---

# ページバリアントのキャッシュ

ページのバリアントのキャッシュをサポートするためにAEM as a cloud service を設定および使用する方法について説明します。

## 使用例

+ ユーザーの位置情報や動的コンテンツを含むページのキャッシュに基づいて、異なるサービスオファーのセットと対応する価格オプションを提供するサービスプロバイダーは、CDN と Dispatcher で管理する必要があります。

+ 小売の顧客は国内に店舗を持ち、各ストアの場所に応じて異なるオファーが提供されます。また、動的コンテンツを含むページのキャッシュは、CDN と Dispatcher で管理する必要があります。

## ソリューションの概要

+ バリアントキーと、そのキーの値の数を指定します。 この例では、米国の状態によって異なるので、最大数は 50 です。 これは、CDN でバリアント制限に関する問題を引き起こさないほど小さいです。 [バリアント制限の節を確認する](#variant-limitations).

+ AEMコードは Cookie を設定する必要があります __&quot;x-aem-variant&quot;__ を訪問者の優先状態 ( 例： `Set-Cookie: x-aem-variant=NY`) を返します。

+ 訪問者からの以降のリクエストでは、その Cookie を送信します ( 例： `“Cookie: x-aem-variant=NY”`) と Cookie が事前に定義されたヘッダー ( `x-aem-variant:NY`) が Dispatcher に渡されます。

+ Apache の書き換えルールによって要求パスが変更され、ページ URL のヘッダー値が Apache Sling セレクター ( 例： `/page.variant=NY.html`) をクリックします。 これにより、AEM パブリッシュは、セレクターと Dispatcher に基づいて異なるコンテンツを提供し、バリアントごとに 1 ページをキャッシュできます。

+ AEM Dispatcher から送信される応答には、HTTP 応答ヘッダーが含まれている必要があります `Vary: x-aem-variant`. これは、CDN に対し、ヘッダー値ごとに異なるキャッシュコピーを保存するように指示します。

## HTTP リクエストフロー

![バリアントキャッシュ要求フロー](./assets/variant-cache-request-flow.png)

>[!NOTE]
>
>上記の最初の HTTP リクエストフローは、バリアントを使用するコンテンツがリクエストされる前に実行する必要があります。

## 使用方法

1. この機能を実演するには、次を使用します。 [WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ja)の実装を例として示します。

1. の実装 [SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) AEMで `x-aem-variant` HTTP 応答の cookie に変数値を格納します。

1. AEM CDN による自動変換 `x-aem-variant` Cookie を同じ名前の HTTP ヘッダーに貼り付けます。

1. Apache Web サーバー mod_rewrite ルールを `dispatcher` プロジェクト内で、バリアントセレクターを含めるように要求パスを変更します。

1. Cloud Manager を使用してフィルターおよび書き換えルールをデプロイします。

1. リクエストフロー全体をテストします。

## コードサンプル

+ 設定する SlingServletFilter のサンプル `x-aem-variant` cookie の値がAEMに設定されていることを確認します。

   ```
   package com.adobe.aem.guides.wknd.core.servlets.filters;
   
   import javax.servlet.*;
   import java.io.IOException;
   
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.api.SlingHttpServletResponse;
   import org.apache.sling.servlets.annotations.SlingServletFilter;
   import org.apache.sling.servlets.annotations.SlingServletFilterScope;
   import org.osgi.service.component.annotations.Component;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   
   
   // Invoke filter on  HTTP GET /content/wknd.*.foo|bar.html|json requests.
   // This code and scope is for example purposes only, and will not interfere with other requests.
   @Component
   @SlingServletFilter(scope = {SlingServletFilterScope.REQUEST},
           resourceTypes = {"cq:Page"},
           pattern = "/content/wknd/.*",
           extensions = {"html", "json"},
           methods = {"GET"})
   public class PageVariantFilter implements Filter {
       private static final Logger log = LoggerFactory.getLogger(PageVariantFilter.class);
       private static final String VARIANT_COOKIE_NAME = "x-aem-variant";
   
       @Override
       public void init(FilterConfig filterConfig) throws ServletException { }
   
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           SlingHttpServletResponse slingResponse = (SlingHttpServletResponse) servletResponse;
           SlingHttpServletRequest slingRequest = (SlingHttpServletRequest) servletRequest;
   
           // Check is the variant was previously set
           final String existingVariant = slingRequest.getCookie(VARIANT_COOKIE_NAME).getValue();
   
           if (existingVariant == null) {
               // Variant has not been set, so set it now
               String newVariant = "NY"; // Hard coding as an example, but should be a calculated value
               slingResponse.setHeader("Set-Cookie", VARIANT_COOKIE_NAME + "=" + newVariant + "; Path=/; HttpOnly; Secure; SameSite=Strict");
               log.debug("x-aem-variant cookie is set with the value {}", newVariant);
           } else {
               log.debug("x-aem-variant previously set with value {}", existingVariant);
           }
   
           filterChain.doFilter(servletRequest, slingResponse);
       }
   
       @Override
       public void destroy() { }
   }
   ```

+ のサンプルの書き換えルール __dispatcher/src/conf.d/rewrite.rules__ Git でソースコードとして管理され、Cloud Manager を使用してデプロイされるファイル。

   ```
   ...
   
   RewriteCond %{REQUEST_URI} ^/us/.*  
   RewriteCond %{HTTP:x-aem-variant} ^.*$  
   RewriteRule ^([^?]+)\.(html.*)$ /content/wknd$1.variant=%{HTTP:x-aem-variant}.$2 [PT,L] 
   
   ...
   ```

## バリアントの制限

+ AEM CDN では、最大 200 個のバリエーションを管理できます。 これは、 `x-aem-variant` ヘッダーには、最大 200 個の一意の値を設定できます。 詳しくは、 [CDN 設定の制限](https://docs.fastly.com/en/guides/resource-limits).

+ 選択したバリアントキーがこの数を超えないように注意する必要があります。  例えば、ユーザー ID は、ほとんどの Web サイトで 200 個を超える可能性が高いので、適切なキーではありません。一方、国内の州/地域は、その国に 200 個未満の州がある場合に適しています。

>[!NOTE]
>
>バリエーションが 200 を超える場合、CDN は、ページコンテンツではなく「バリエーションが多すぎます」という応答を返します。
