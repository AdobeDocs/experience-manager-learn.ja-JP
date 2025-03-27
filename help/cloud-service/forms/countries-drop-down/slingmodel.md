---
title: コンポーネントの Sling モデルの作成
description: コンポーネントの Sling モデルの作成
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: f4a18f02-61a2-4fa3-bfbb-41bf696cd2a8
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '384'
ht-degree: 100%

---

# コンポーネントの Sling モデルの作成

AEM の Sling モデルは、コンポーネントのバックエンドロジックの開発を簡素化するために使用される Java ベースのフレームワークです。これにより、開発者は注釈を使用して AEM リソース（JCR ノード）のデータを Java オブジェクトにマッピングでき、コンポーネントの動的データを処理するためのクリーンかつ効率的な方法が提供されます。
このクラス CountriesDropDownImpl は、AEM（Adobe Experience Manager）プロジェクトの CountriesDropDown インターフェイスの実装です。これにより、ユーザーが選択した大陸に基づいて国を選択できるドロップダウンコンポーネントが強化されます。ドロップダウンデータは、AEM DAM（Digital Asset Manager）に保存されている JSON ファイルから動的に読み込まれます。

**クラスのフィールド**

* **multiSelect**：ドロップダウンで複数選択を許可するかどうかを示します。
デフォルト値が false の @ValueMapValue を使用してコンポーネントプロパティから挿入されます。
* **request**：現在の HTTP リクエストを表します。コンテキスト固有の情報へのアクセスに役立ちます。
* **continent**：ドロップダウンで選択した大陸を保存します（例：「アジア」、「ヨーロッパ」）。
コンポーネントのプロパティダイアログから挿入されます。指定していない場合は、デフォルト値が「アジア」になります。
* **resourceResolver**：AEM リポジトリ内のリソースにアクセスして操作するために使用されます。
* **jsonData**：選択した大陸に対応する JSON ファイルから解析されたデータを保存する JSONObject。

**クラスの更新**

* **getContinent()**：大陸フィールドの値を返す簡単なメソッド。
デバッグの目的で返された値をログに記録します。
* **init()**：クラスが構築され、依存関係が挿入された後に実行される、@PostConstruct で注釈が付けられたライフサイクルメソッド。大陸の値に基づいて JSON ファイルへのパスを動的に構築します。
resourceResolver を使用して AEM DAM から JSON ファイルを取得します。
ファイルをアセットに適合させ、そのコンテンツを読み取り、JSONObject に解析します。
この処理中のエラーや警告をログに記録します。
* **getEnums()**：解析された JSON データからすべてのキー（国コード）を取得します。
キーをアルファベット順に並べ替え、配列として返します。
返される国コードの数をログに記録します。
* **getEnumNames()**：解析された JSON データからすべての国名を抽出します。
名前をアルファベット順に並べ替え、配列として返します。
国の合計数と取得した各国名をログに記録します。
* **isMultiSelect()**：multiSelect フィールドの値を返し、ドロップダウンで複数選択を許可するかどうかを示します。



```java
package com.aem.customcorecomponent.core.models.impl;
import com.adobe.cq.export.json.ComponentExporter;
import com.adobe.cq.export.json.ExporterConstants;
import com.adobe.cq.forms.core.components.internal.form.ReservedProperties;
import com.adobe.cq.forms.core.components.util.AbstractOptionsFieldImpl;
import com.aem.customcorecomponent.core.models.CountriesDropDown;
import com.day.cq.dam.api.Asset;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.models.annotations.Default;
import org.apache.sling.models.annotations.Exporter;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.InjectionStrategy;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
import org.json.JSONObject;
import javax.annotation.PostConstruct;
import javax.inject.Inject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import java.util.*;
import java.util.stream.Collectors;

//@Model(adaptables = SlingHttpServletRequest.class,adapters = CountriesDropDown.class,defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)

@Model(
        adaptables = { SlingHttpServletRequest.class, Resource.class },
        adapters = { CountriesDropDown.class,
                ComponentExporter.class },
        resourceType = { "corecomponent/components/adaptiveForm/countries" })
@Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)

public class CountriesDropDownImpl extends AbstractOptionsFieldImpl implements CountriesDropDown {
    @ValueMapValue(injectionStrategy = InjectionStrategy.OPTIONAL, name = ReservedProperties.PN_MULTISELECT)
    @Default(booleanValues = false)
    protected boolean multiSelect;

    private static final Logger logger = LoggerFactory.getLogger(CountriesDropDownImpl.class);
    @Inject
    SlingHttpServletRequest request;

    @Inject
    @Default(values = "asia")
    public String continent;
    @Inject
    private ResourceResolver resourceResolver;
    private JSONObject jsonData;
    public String getContinent()
    {
        logger.info("Returning continent");
        return continent;
    }
    @PostConstruct
    public void init() {

        String jsonPath = "/content/dam/corecomponent/" + getContinent() + ".json"; // Update path as needed
        logger.info("Initializing JSON data for continent: {}", getContinent());

        try {
            // Fetch the JSON resource
            Resource jsonResource = resourceResolver.getResource(jsonPath);
            if (jsonResource != null) {
                // Adapt the resource to an Asset
                Asset asset = jsonResource.adaptTo(Asset.class);
                if (asset != null) {
                    // Get the original rendition and parse it as JSON
                    try (InputStream inputStream = asset.getOriginal().adaptTo(InputStream.class);
                         BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream, StandardCharsets.UTF_8))) {

                        String jsonString = reader.lines().collect(Collectors.joining());
                        jsonData = new JSONObject(jsonString);
                        logger.info("Successfully loaded JSON data for path: {}", jsonPath);
                    }
                } else {
                    logger.warn("Failed to adapt resource to Asset at path: {}", jsonPath);
                }
            } else {
                logger.warn("Resource not found at path: {}", jsonPath);
            }
        } catch (Exception e) {
            logger.error("An error occurred while initializing JSON data for path: {}", jsonPath, e);
        }
    }

    @Override
    public Object[] getEnums() {
        Set<String> keySet = jsonData.keySet();


// Convert the set of keys to a sorted array
        String[] countryCodes = keySet.toArray(new String[0]);
        Arrays.sort(countryCodes);

        logger.debug("Returning sorted country codes: " + countryCodes.length);

        return countryCodes;


    }

    @Override
    public String[] getEnumNames() {
        String[] countries = new String[jsonData.keySet().size()];
        logger.info("Fetching countries - Total number: " + countries.length);

// Populate the array with country names
        int index = 0;
        for (String code : jsonData.keySet()) {
            String country = jsonData.getString(code);
            logger.debug("Retrieved country: " + country);
            countries[index++] = country;
        }

// Sort the array alphabetically
        Arrays.sort(countries);
        logger.debug("Returning " + countries.length + " sorted countries");

        return countries;

    }

    @Override
    public Boolean isMultiSelect() {
        return multiSelect;
    }
}
```

## 次の手順

[作成、デプロイ、テスト](./build.md)
