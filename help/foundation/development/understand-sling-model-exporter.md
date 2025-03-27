---
title: AEM の Sling モデルエクスポーターについて
description: Apache Sling Models 1.3.0 では、Sling モデルオブジェクトをカスタムの抽象概念に書き出しまたはシリアル化する優れた方法である Sling モデルエクスポーターが導入されました。 この記事では、Sling モデルを使用して HTL スクリプトを入力する従来のユースケースと、Sling モデルエクスポーターフレームワークを利用した Sling モデルの JSON へのシリアル化を並置します。
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
duration: 133
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '568'
ht-degree: 100%

---

# [!DNL Sling Model Exporter] について

Apache [!DNL Sling Models] 1.3.0 では [!DNL Sling Model Exporter] が導入されています。これは、[!DNL Sling Model] オブジェクトをカスタム抽象化にエクスポートまたはシリアル化するエレガントな方法です。この記事では、[!DNL Sling Models] を使用して HTL スクリプトを入力する従来のユースケースと、[!DNL Sling Model Exporter] フレームワークを利用した [!DNL Sling Model] の JSON へのシリアル化を並置します。

## 従来の Sling モデル HTTP リクエストフロー

[!DNL Sling Models] の従来のユースケースは、リソースまたはリクエストのビジネス抽象化を提供することです。これにより、ビジネス機能にアクセスするためのインターフェイスが HTL スクリプト（以前の JSP）に提供されます。

一般的なパターンは、AEM コンポーネントまたはページを表す [!DNL Sling Models] を開発し、[!DNL Sling Model] オブジェクトを使用して HTL スクリプトにデータをフィードし、HTML の最終結果をブラウザーに表示することです。

### Sling モデル HTTP リクエストフロー

![Slingモデルリクエストフロー](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. AEM のリソースに対して [!DNL HTTP GET] リクエストが行われます。

   例：`HTTP GET /content/my-resource.html`

1. リクエストリソースの `sling:resourceType` に基づいて、適切なスクリプトが解決されます。

1. スクリプトは、リクエストまたはリソースを目的の [!DNL Sling Model] に適合させます。

1. スクリプトは [!DNL Sling Model] オブジェクトを使用して HTML レンディションを生成します。

1. スクリプトで生成された HTML は、HTTP 応答で返されます。

[!DNL Sling Model] は HTL を介して簡単に利用できるため、この従来のパターンは HTML を生成するコンテキストでうまく機能します。JSON や XML など、より構造化されたデータを作成する方が、HTL が自然にこれらの形式の定義に適用されないので、はるかに退屈な作業です。

## [!DNL Sling Model Exporter] HTTP リクエストフロー

Apache [!DNL Sling Model Exporter] には、「通常の」[!DNL Sling Model] オブジェクトを JSON に自動的にシリアル化する Sling 提供の Jackson エクスポーターが付属しています。Jackson エクスポーターは、設定が可能な状態で、その中核では [!DNL Sling Model] オブジェクトを生成し、JSON キーとして「getter」メソッドを使用し、JSON 値として getter 戻り値を使用して JSON を生成します。

[!DNL Sling Models] の直接シリアル化により、従来の [!DNL Sling Model] リクエストフロー（上記を参照）を使用して作成された HTML レスポンスを使用して通常の Web リクエストを処理できるだけでなく、Web サービスまたは JavaScript アプリケーションで使用できる JSON レンディションを公開することもできます。

![Sling モデルエクスポーター HTTP リクエストフロー](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*このフローは、提供された Jackson エクスポーターを使用した JSON 出力の生成に関するフローを記述します。 カスタムエクスポーターの使用は、同じフローに従いますが、その出力形式で行います。*

1. [!DNL Sling Model] のエクスポーターに登録されたセレクターと拡張機能を使用して、AEM のリソースに対して HTTP GET リクエストが行われます。

   例：`HTTP GET /content/my-resource.model.json`

1. Sling は、リクエストされたリソースの `sling:resourceType`、セレクター、および拡張機能を、動的に生成された Sling エクスポーターサーブレットに解決します。このサーブレットは、エクスポーターで [!DNL Sling Model] にマッピングされます。
1. 解決された Sling エクスポーターサーブレットは、リクエストまたはリソースから適応された [!DNL Sling Model] オブジェクトに対して [!DNL Sling Model Exporter] を呼び出します（Sling Models adaptables によって決定される）。
1. エクスポーターは、エクスポーターオプションとエクスポーター固有の Sling モデルアノテーションに基づいて [!DNL Sling Model] をシリアル化し、結果を Sling エクスポーターサーブレットに返します。
1. Sling エクスポーターサーブレットは、HTTP レスポンスで [!DNL Sling Model] の JSON レンディションを返します。

>[!NOTE]
>
>Apache Sling プロジェクトは [!DNL Sling Models] を JSON にシリアル化する Jackson エクスポーターを提供しますが、エクスポーターフレームワークはカスタムエクスポーターもサポートします。例えば、プロジェクトは、[!DNL Sling Model] を XML にシリアル化するカスタムエクスポーターを実装できます。

>[!NOTE]
>
>[!DNL Sling Model Exporter] は [!DNL Sling Models] を *シリアル化*&#x200B;するだけでなく、それらを Java オブジェクトとして書き出しすることもできます。他の Java オブジェクトへの書き出しは、HTTP リクエストフローで役割を果たさないので、上の図には表示されません。

## サポート資料

* [Apache [!DNL Sling Model Exporter]  フレームワークのドキュメント](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
