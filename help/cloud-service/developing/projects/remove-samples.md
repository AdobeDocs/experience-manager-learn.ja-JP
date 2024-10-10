---
title: AEM Maven プロジェクトからのサンプルの削除
description: AEM プロジェクトアーキタイプで生成された AEM プロジェクトからサンプルコードをクリーンアップし削除する方法を説明します。
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
jira: KT-9092
thumbnail: 337263.jpeg
exl-id: 4e10c2b7-41b6-41a0-b8d4-9207a9d3f9c8
duration: 341
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '85'
ht-degree: 100%

---

# AEM Maven プロジェクトからのサンプルの削除

AEM プロジェクトアーキタイプで生成された AEM プロジェクトから、生成されたサンプルコードをクリーンアップし削除する方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/337263?quality=12&learn=on)


## リソース

+ [AEM Maven プロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)

## コマンド

次のコマンドを実行すると、生成されたサンプルファイルを AEM Maven プロジェクトから削除できます。

```
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/filters \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/listeners \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/models \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/schedulers \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/servlets \
rm -rf core/src/test/java/com/adobe/aem/wknd/examples/core/* \
rm -rf ui.apps/src/main/content/jcr_root/apps/wknd-examples/components/helloworld \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.js \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.css
```

## 編集

`<div class="helloworld" ...></div>` を次から削除します。

```
ui.frontend/src/main/webpack/static/index.html
```

`<helloworld>` コンポーネントインスタンス定義を次から削除します。

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
