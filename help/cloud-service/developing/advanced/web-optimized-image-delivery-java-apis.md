---
title: Web に最適化された画像配信 Java&trade;API
description: AEM as a Cloud Serviceの Web に最適化された画像配信 Java&trade の使用方法を説明します。高パフォーマンスの Web エクスペリエンスを開発する API。
version: Cloud Service
feature: APIs, Sling Model, OSGI, HTL or HTML Template Language
topic: Performance, Development
role: Architect, Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-03-30T00:00:00Z
jira: KT-13014
thumbnail: KT-13014.jpeg
source-git-commit: 14d89d1a3c424de044df4f6d74546788256fa383
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 1%

---


# Web に最適化された画像配信 Java™ API

AEM as a Cloud Serviceの Web に最適化された画像配信 Java™ API を使用して、高パフォーマンスの Web エクスペリエンスを開発する方法を説明します。

AEMas a Cloud Serviceサポート [web に最適化された画像配信](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html) は、アセットの最適化された画像 Web レンディションを自動的に生成します。 Web に最適化された画像配信は、次の 3 つの主な方法で使用できます。

1. [AEM Core WCM コンポーネントの使用](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)
2. 次のカスタムコンポーネントを作成 [AEM Core WCM コンポーネントの画像コンポーネントを拡張](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html#tackling-the-image-problem)
3. AssetDelivery Java™ API を使用して Web に最適化された画像 URL を生成するカスタムコンポーネントを作成します。

この記事では、AEM as a Cloud ServiceとAEM SDK の両方でコードベースが機能するように、カスタムコンポーネントで Web 最適化画像 Java™ API の使用について説明します。

## Java™ API

この [アセット配信 API](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/cq/wcm/spi/AssetDelivery.html) は、画像アセット用に Web 最適化された配信 URL を生成する OSGi サービスです。 `AssetDelivery.getDeliveryURL(...)` 使用できるオプションは次のとおりです。 [ここに記載されています](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html#can-i-use-web-optimized-image-delivery-with-my-own-component%3F).

この `AssetDelivery` OSGi サービスは、AEM as a Cloud Serviceで実行している場合にのみ満たされます。 AEM SDK では、 `AssetDelivery` OSGi サービスの戻り値 `null`. AEM as a Cloud Serviceで実行する場合は Web に最適化された URL を条件付きで使用し、AEM SDK ではフォールバック画像 URL を使用することをお勧めします。 通常、アセットの Web レンディションは十分なフォールバックです。


### OSGi サービスでの API の使用

を`AssetDelivery` カスタム OSGi サービスをAEM SDK で引き続き使用できるように、カスタム OSGi サービスではをオプションとして参照します。

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@Reference(cardinality = ReferenceCardinality.OPTIONAL)
private volatile AssetDelivery assetDelivery;
```

### Sling モデルで使用する API

を`AssetDelivery` カスタム Sling モデルではをオプションとして参照するので、カスタム Sling モデルはAEM SDK で引き続き使用できます。

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@OSGiService(injectionStrategy = InjectionStrategy.OPTIONAL)
private AssetDelivery assetDelivery;
```

### API の条件付き使用

Web に最適化された画像 URL または `AssetDelivery` OSGi サービスの可用性。 条件付き使用では、AEM SDK でコードを実行する際にコードが機能します。

```java
if (assetDelivery != null ) {
    // When running on AEM as a Cloud Service use the real web-optimized image URL.
    return assetDelivery.getDeliveryURL(...);
} else {
    // When running on AEM SDK, use some fallback image so the experience does not break.
    // What the fallback is up to you! 
    return getFallbackURL(...);
}
```

## サンプルコード

次のコードは、Web に最適化された画像 URL を使用して画像アセットのリストを表示するサンプルコンポーネントを作成します。

コードをAEM as a Cloud Service上で実行する場合、Web に最適化された Web 画像レンディションがカスタムコンポーネントで使用されます。

![AEM as a Cloud Service上の Web に最適化された画像](./assets/web-optimized-image-delivery-java-apis/cloud-service.png)

_AEM as a Cloud Serviceは AssetDelivery API をサポートしているので、Web に最適化された WebP レンディションが使用されます_

コードをAEM SDK で実行する場合は、最適でない静的 Web レンディションが使用され、ローカル開発中にコンポーネントを機能させます。

![AEM SDK の Web に最適化されたフォールバック画像](./assets/web-optimized-image-delivery-java-apis/aem-sdk.png)

_AEM SDK は AssetDelivery API をサポートしていないので、フォールバックの静的 Web レンディション (PNG またはJPEG) が使用されます_

実装は、次の 3 つの論理的な部分に分類されます。

1. この `WebOptimizedImage` OSGi サービスは、AEMが提供するの「スマートプロキシ」として機能します `AssetDelivery` AEM as a Cloud Service SDK とAEM SDK の両方で実行を処理できる OSGi サービス。
2. この `ExampleWebOptimizedImages` Sling モデルは、表示する画像アセットとその Web に最適化された URL のリストを収集するビジネスロジックを提供します。
3. この `example-web-optimized-images` AEMコンポーネントは、HTL を実装して Web に最適化された画像のリストを表示します。

以下のコード例は、コードベースにコピーし、必要に応じて更新できます。

### OSGi サービス

この `WebOptimizedImage` OSGi サービスは、アドレス可能なパブリックインターフェイス (`WebOptimizedImage`) および内部実装 (`WebOptimizedImageImpl`) をクリックします。 この `WebOptimizedImageImpl` は、AEM as a Cloud Serviceで実行している場合に web に最適化された画像 URL と、AEM SDK で静的な web レンディション URL を返し、コンポーネントをAEM SDK で引き続き機能させます。

#### インターフェイス

インターフェイスは、Sling モデルなどの他のコードとやり取りできる OSGi サービス契約を定義します。

```java
package com.adobe.aem.guides.wknd.core.images;

import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.annotation.versioning.ProviderType;

import java.util.Map;

/**
 * OSGi Service that acts as a facade for the AssetDelivery API, such that a fallback can be automatically served on the AEM SDK.
 *
 * This service can be extended to provide additional functionality, such as srcsets, etc.
 */
@ProviderType
public interface WebOptimizedImage {
    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options);
}
```

#### 実装

OSGi サービスの実装には、AEMへのオプションの参照が含まれています `AssetDelivery` OSGi サービス、および適切な画像 URL を選択するためのフォールバックロジック ( `AssetDelivery` が `null` AEM SDK の フォールバックロジックは、要件に基づいて更新できます。

```java
package com.adobe.aem.guides.wknd.core.images.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.cq.wcm.spi.AssetDelivery;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.api.Rendition;
import com.day.cq.dam.api.RenditionPicker;
import com.day.cq.dam.commons.util.DamUtil;
import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;

import java.util.Map;
@Component
public class WebOptimizedImageImpl implements WebOptimizedImage {
    private static final String DEFAULT_FORMAT = "webp";
    @Reference(cardinality = ReferenceCardinality.OPTIONAL)
    private volatile AssetDelivery assetDelivery;

    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    @Override
    public String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        if (assetDelivery != null) {
            return getWebOptimizedUrl(resourceResolver, path, options);
        } else {
            return getFallbackUrl(resourceResolver, path);
        }
    }
    /**
     * Uses the AssetDelivery API to get the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    private String getWebOptimizedUrl(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        // These 3 options are required for the AssetDelivery API to work, else it will return null
        options.put("path", asset.getPath());
        options.put("format", StringUtils.defaultString((String) options.get("format"), DEFAULT_FORMAT));
        options.put("seoname", StringUtils.defaultString((String) options.get("seoname"), asset.getName()));

        // The resource only provides the security context into AEM so the asset's UUID can be looked up for the Web Optimized Image URL
        return assetDelivery.getDeliveryURL(resource, options);
    }

    /**
     * Fallback to the static web rendition if the AssetDelivery API is not available, meaning the code is running on the AEM SDK.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @return the path to the web rendition
     */
    private String getFallbackUrl(ResourceResolver resourceResolver, String path) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        return asset.getRendition(WebRenditionPicker).getPath();
    }

    /**
     * Picks the web rendition of the asset.
     */
    private static final RenditionPicker WebRenditionPicker = new RenditionPicker() {
        @Override
        public Rendition getRendition(Asset asset) {
            return asset.getRenditions().stream().filter(rendition -> StringUtils.startsWith(rendition.getName(), "cq5dam.web.")).findFirst().orElse(asset.getOriginal());
        }
    };
}
```

### Sling モデル

この `ExampleWebOptimizedImages` Sling モデルは、アドレス可能なパブリックインターフェイス (`ExampleWebOptimizedImages`) および内部実装 (`ExampleWebOptimizedImagesImpl`);

この `ExampleWebOptimizedImagesImpl` Sling Model は、表示する画像アセットのリストを収集し、カスタムを呼び出します `WebOptimizedImage` Web に最適化された画像 URL を取得する OSGi サービス。 この Sling モデルはAEMコンポーネントを表すので、次のような通常のメソッドがあります。 `isEmpty()`, `getId()`、および `getData()` ただし、これらの方法は、web 最適化された画像の使用に直接関連するものではありません。

#### インターフェイス

インターフェイスは、HTL などの他のコードがやり取りできる Sling Model コントラクトを定義します。

```java
package com.adobe.aem.guides.wknd.core.models;

import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.fasterxml.jackson.annotation.JsonProperty;

import java.util.List;

public interface ExampleWebOptimizedImages {

    /**
     * @return a list of web optimized images for the component to display. Each item in the list has necessary information to render the image.
     */
    List<Img> getImages();

    /**
     * @return true if this component has no images to display.
     */
    boolean isEmpty();

    /**
     * @return String representing the unique identifier of the ExampleWebOptimizedImages component on a page
     */
    String getId();

    /**
     * @return JSON data to populate the data layer
     */
    @JsonProperty("dataLayer")
    default ComponentData getData() {
        return null;
    }

    /**
     * Describes a web optimized image.
     */
    interface Img {
        /**
         * @return the URL to the web optimized rendition of the image.
         */
        String getSrc();

        /**
         * @return the alt text of the web optimized image.
         */
        String getAlt();

        /**
         * @return the height of the web optimized image.
         */
        String getHeight();
        /**
         * @return the width of the web optimized image.
         */
        String getWidth();
    }
}
```

#### 実装

Sling モデルは、カスタム `WebOptimizeImage` コンポーネントに表示される画像アセットの Web 最適化画像 URL を収集する OSGi サービス。

この例では、画像アセットの収集に単純なクエリを使用します。

```java
package com.adobe.aem.guides.wknd.core.models.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages;
import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
import com.adobe.cq.wcm.core.components.util.ComponentUtils;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.commons.util.DamUtil;
import com.day.cq.wcm.api.Page;
import com.day.cq.wcm.api.components.ComponentContext;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.models.annotations.DefaultInjectionStrategy;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.Required;
import org.apache.sling.models.annotations.injectorspecific.OSGiService;
import org.apache.sling.models.annotations.injectorspecific.ScriptVariable;
import org.apache.sling.models.annotations.injectorspecific.Self;

import java.util.*;

@Model(
        adaptables = {SlingHttpServletRequest.class},
        adapters = {ExampleWebOptimizedImages.class},
        resourceType = {ExampleWebOptimizedImagesImpl.RESOURCE_TYPE},
        defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
)
public class ExampleWebOptimizedImagesImpl implements ExampleWebOptimizedImages {

    protected static final String RESOURCE_TYPE = "wknd/components/example-web-optimized-images";

    private static final int MAX_RESULTS = 10;

    @Self
    @Required
    private SlingHttpServletRequest request;

    @OSGiService
    private WebOptimizedImage webOptimizedImage;

    @ScriptVariable
    private Page currentPage;

    @ScriptVariable
    protected ComponentContext componentContext;

    private List<Img> images;

    // XPath query to find image assets to display
    private static final String XPATH_QUERY = "/jcr:root/content/dam/wknd-shared/en/adventures//element(*, dam:Asset) [ (jcr:contains(jcr:content/metadata/@dc:format, 'image/')) ]";
    @Override
    public List<Img> getImages() {

        if (images == null) {
            images = new ArrayList<>();

            // Set the AssetDelivery options to request a web-optimized rendition.
            // These options can be set as required by the implementation (Dialog, pass in from HTL via @RequestAttribute)
            final Map<String, Object> options = new HashMap<>();
            options.put("format", "webp");
            options.put("preferwebp", "true");
            options.put("width", "350");
            options.put("height", "350");

            final Iterator<Resource> results = request.getResourceResolver().findResources(XPATH_QUERY, "xpath");

            while (results.hasNext() && images.size() < MAX_RESULTS) {
                Resource resource = results.next();
                Asset asset = DamUtil.resolveToAsset(resource);

                // Get the image URL; the web-optimized rendition on AEM as a Cloud Service, or the static web rendition fallback on AEM SDK
                final String url = webOptimizedImage.getDeliveryURL(request.getResourceResolver(), resource.getPath(), options);

                // Add the image to the list that is passed to the HTL component to display
                // We'll add some extra attributes so that the HTL can display the image in a performant, SEO-friendly, and accessible way
                // ImgImpl can be extended to add additional attributes, such as srcset, etc.
                images.add(new ImgImpl(url, asset.getName(), (String) options.get("height"), (String) options.get("width")));
            }
        }

        return this.images;
    }

    @Override
    public boolean isEmpty() {
        return getImages().isEmpty();
    }

    @Override
    public String getId() {
        return ComponentUtils.getId(request.getResource(), currentPage, componentContext);
    }

    @Override
    public ComponentData getData() {
        if (ComponentUtils.isDataLayerEnabled(request.getResource())) {
            return DataLayerBuilder.forComponent()
                    .withId(() -> getId())
                    .withType(() -> RESOURCE_TYPE)
                    .build();
        }
        return null;
    }

    class ImgImpl implements Img {
        private final String url;
        private final String alt;
        private final int height;
        private final int width;

        public ImgImpl(String url, String alt, String height, String width) {
            this.url = url;
            this.alt = alt;
            this.height = Integer.parseInt(height);
            this.width = Integer.parseInt(width);
        }

        @Override
        public String getSrc() {
            return url;
        }

        @Override
        public String getAlt() {
            return alt;
        }

        @Override
        public String getHeight() {
            return height + "px";
        }

        @Override
        public String getWidth() {
            return width + "px";
        }
    }
}
```

### AEM Component

AEMコンポーネントは、 `WebOptimizedImagesImpl` Sling モデルの実装。画像のリストを表示します。



このコンポーネントは、 `Img` オブジェクト経由 `getImages()` AEM as a Cloud Service上で実行する際に、web に最適化された WEBP 画像を含める このコンポーネントは、 `Img` オブジェクト経由 `getImages()` AEM SDK で実行している場合に、静的 PNG/JPEGWeb 画像を含める

#### HTL

HTL では `WebOptimizedImages` Sling Model を作成し、  `Img` オブジェクトが返す `getImages()`.

```html
<style>
    .cmp-example-web-optimized-images__list {
        width: 100%;
        list-style: none;
        padding: 0;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        gap: 2rem;
    }

    .cmp-example-web-optimized-images-list__item {
        margin: 0;
        padding: 0;
    }
</style>

<div data-sly-use.exampleImages="com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages"
     data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-test.hasContent="${!exampleImages.empty}"
     data-cmp-data-layer="${exampleImages.data.json}">

    <h3>Example web-optimized images</h3>

    <ul class="cmp-example-web-optimized-images__list"
        data-sly-list.item="${exampleImages.images}">
        <li class="cmp-example-web-optimized-images-list__item">
            <img class="cmp-example-web-optimized-images__image"
                 src="${item.src}"
                 alt="${item.alt}"
                 width="${item.width}"/>
        </li>
    </ul>
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent, classAppend='cmp-example-web-optimized-images'}"></sly>
```
