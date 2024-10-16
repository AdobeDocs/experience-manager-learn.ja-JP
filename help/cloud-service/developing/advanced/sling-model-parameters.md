---
title: HTL から Sling Model のパラメーター化
description: AEM で HTL から Sling Model にパラメーターを渡す方法を説明します。
version: Cloud Service
topic: Development
feature: Sling Model
role: Developer
jira: KT-15923
level: Intermediate, Experienced
exl-id: 5d852617-720a-4a00-aecd-26d0ab77d9b3
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: ht
source-wordcount: '264'
ht-degree: 100%

---

# HTL から Sling Model のパラメーター化

Adobe Experience Manager（AEM）は、動的で柔軟な web アプリケーションを構築するための堅牢なフレームワークを提供します。その強力な機能の 1 つは、Sling Model をパラメーター化し、柔軟性と再利用性を向上させるというものです。このチュートリアルでは、パラメーター化された Sling Model を作成し、HTL（HTML テンプレート言語）で使用して動的コンテンツをレンダリングする手順を説明します。

## HTL スクリプト

この例では、HTL スクリプトは 2 つのパラメーターを `ParameterizedModel` Sling Model に送信します。モデルは `getValue()` メソッドでこれらのパラメーターを操作し、表示する結果を返します。

この例では 2 つの文字列パラメーターを渡しますが、`@RequestAttribute`](#sling-model-implementation) の注釈が付いた [Sling Model フィールドタイプが HTL から渡されたオブジェクトのタイプまたは値と一致する限り、任意のタイプの値またはオブジェクトを Sling Model に渡すことができます。

```html
<sly data-sly-use.myModel="${'com.example.core.models.ParameterizedModel' @ myParameterOne='Hello', myParameterTwo='World'}"
     data-sly-test.isReady="${myModel.isReady()}">

    <p>
        ${myModel.value}
    </p>

</sly>

<sly data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!isReady}">
</sly>
```

- **パラメーター化：**`data-sly-use` ステートメントは、`myParameterOne` と `myParameterTwo` を持つ `ParameterizedModel` のインスタンスを作成します。
- **条件付きレンダリング：**`data-sly-test` は、コンテンツを表示する前にモデルの準備が整っているかどうかを確認します。
- **プレースホルダー呼び出し：**`placeholderTemplate` では、モデルの準備が整っていない場合に対処します。

## Sling Model の実装

Sling Model を実装する方法を次に示します。

```java
package com.example.core.models.impl;

import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.SlingHttpServletRequest;
import org.osgi.service.component.annotations.Model;
import org.osgi.service.component.annotations.RequestAttribute;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Model(
    adaptables = { SlingHttpServletRequest.class },
    adapters = { ParameterizedModel.class }
)
public class ParameterizedModelImpl implements ParameterizedModel {
    private static final Logger log = LoggerFactory.getLogger(ParameterizedModelImpl.class);

    // Use the @RequestAttribute annotation to inject the parameter set in the HTL.
    // Note that the Sling Model field can be any type, but must match the type of object or value passed from HTL.
    @RequestAttribute
    private String myParameterOne;

    // If the HTL parameter name is different from the Sling Model field name, use the name attribute to specify the HTL parameter name
    @RequestAttribute(name = "myParameterTwo")
    private String mySecondParameter;

    // Do some work with the parameter values
    @Override
    public String getValue() {
        return myParameterOne + " " + mySecondParameter + ", from the parameterized Sling Model!";
    }

    @Override
    public boolean isReady() {
        return StringUtils.isNotBlank(myParameterOne) && StringUtils.isNotBlank(mySecondParameter);
    }
}
```

- **モデル注釈：**`@Model` 注釈は、このクラスを Sling モデルとして指定し、`SlingHttpServletRequest` から適応可能で、`ParameterizedModel` インターフェイスを実装します。
- **リクエスト属性：**`@RequestAttribute` 注釈は、モデルに HTL パラメーターを挿入します。
- **メソッド：**`getValue()` がパラメーターを連結し、`isReady()` はパラメーターが空白でないことを確認します。

`ParameterizedModel` インターフェイスは次のように定義します。

```java
package com.example.core.models;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface ParameterizedModel {
    /**
     * Get an example string value from the model. This value is the concatenation of the two parameters.
     * 
     * @return the value of the model
     */
    String getValue();

    /**
     * Check if the model is ready to be used.
     *
     * @return {@code true} if the model is ready, {@code false} otherwise
     */
    boolean isReady();
}
```

## 出力例

パラメーター `"Hello"` と `"World"` を使用すると、HTL スクリプトは次の出力を生成します。

```html
<p>
    Hello World, from the parameterized Sling Model!
</p>
```

これは、HTL 経由で提供される入力パラメーターに基づいて、AEM のパラメーター化された Sling モデルがどのように影響を受けるかを示しています。
