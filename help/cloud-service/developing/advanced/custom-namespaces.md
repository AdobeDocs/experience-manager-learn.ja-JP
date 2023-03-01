---
title: カスタム名前空間
description: カスタム名前空間を定義し、AEM as a Cloud Serviceにデプロイする方法を説明します。
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
kt: 11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 1a4ee470a650aacc5412fbd27062ca14ccdb1967
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 6%

---

# カスタム名前空間

カスタムの定義とデプロイの方法を説明します [名前空間](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) をAEMas a Cloud Serviceに

カスタム名前空間は、JCR プロパティのオプションの部分で、 `:`. AEMは、次のようないくつかの名前空間を使用します。

+ `jcr` （JCR システムプロパティの場合）
+ `cq` (AEM( 旧称Adobe CQ) プロパティの場合 )
+ `dam` DAM アセット固有のAEMプロパティの場合
+ `dc` （Dublin Core プロパティ用）

その他多数

名前空間は、プロパティの範囲と目的を示すために使用できます。 カスタム名前空間（多くの場合、会社名）を作成すると、AEM実装に固有のノードやプロパティを明確に識別し、自社のビジネスに固有のデータを含めるのに役立ちます。

カスタム名前空間は、で管理されます。 [Sling リポジトリの初期化 (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) スクリプトが作成され、OSGi 設定としてAEMにデプロイされます。また、OSGi 設定としてas a Cloud Serviceに追加されます。 [AEMプロジェクトの](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ja) `ui.config` プロジェクト。

>[!VIDEO](https://video.tv.adobe.com/v/3412319/?quality=12&learn=on)

## リソース

+ [Sling リポジトリの初期化 (repoinit) ドキュメント](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## コード

次のコードは、 `wknd` 名前空間。

### RepositoryInitializer OSGi 設定

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

これにより、 `wknd` 名前空間。の後の最初のパラメーターとして示されます。 `register namespace` 命令。AEMで使用します。 より高度なスクリプト定義については、 [Sling リポジトリの初期化 (repoinit) ドキュメント](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
