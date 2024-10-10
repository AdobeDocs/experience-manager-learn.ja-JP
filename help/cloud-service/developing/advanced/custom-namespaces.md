---
title: カスタム名前空間
description: カスタム名前空間を定義して AEM as a Cloud Service にデプロイする方法を説明します。
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
jira: KT-11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
duration: 496
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '203'
ht-degree: 100%

---

# カスタム名前空間

カスタム[名前空間](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html)を定義して AEM as a Cloud Service にデプロイする方法を説明します。

カスタム名前空間は、JCR プロパティの「`:`」の前にあるオプション部分です。AEM では、次のようないくつかの名前空間を使用します。

+ `jcr`（JCR システムプロパティの場合）
+ `cq`（AEM（旧称 Adobe CQ）プロパティの場合）
+ `dam`（DAM アセット固有の AEM プロパティの場合）
+ `dc`（Dublin Core プロパティの場合）

その他多数

名前空間を使用すると、プロパティの範囲と目的を示すことができます。カスタム名前空間（多くの場合、会社名）を作成すると、AEM 実装に固有のノードやプロパティを明確に識別し、自社のビジネスに固有のデータを含めることができます。

カスタム名前空間は、[Sling リポジトリ初期化（repoinit）](https://sling.apache.org/documentation/bundles/repository-initialization.html)スクリプトで管理され、OSGi 設定として AEM as a Cloud Service にデプロイされて [AEM プロジェクトの](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ja) `ui.config` プロジェクトに追加されます。

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## リソース

+ [Sling リポジトリの初期化（repoinit）に関するドキュメント](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## コード

次のコードを使用して、`wknd` 名前空間を設定します。

### RepositoryInitializer OSGi 設定

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

これにより、`register namespace` 命令の後の最初のパラメーターとして示される `wknd` 名前空間を使用したカスタムプロパティを AEM で使用できるようになります。より詳細なスクリプト定義については、[Sling リポジトリ初期化（repoinit）のドキュメント](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)で取り上げている例を確認してください。
