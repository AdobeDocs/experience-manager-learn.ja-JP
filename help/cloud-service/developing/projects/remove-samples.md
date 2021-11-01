---
title: AEM Maven プロジェクトからのサンプルの削除
description: AEMプロジェクトアーキタイプで生成されたAEMプロジェクトからサンプルコードをクリーンアップし、削除する方法を説明します。
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
kt: 9092
thumbnail: 337263.jpeg
source-git-commit: e8b3bcaeee40b4bfd4f967f929ad664e8d168cb0
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 6%

---


# AEM Maven プロジェクトからのサンプルの削除

AEMプロジェクトアーキタイプで生成されたAEMプロジェクトから、生成されたサンプルコードをクリーンアップし、削除する方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/337263/?quality=12&learn=on)


## リソース

+ [AEM Maven プロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)

## コマンド

次のコマンドを実行して、生成されたサンプルファイルをAEM Maven プロジェクトから削除できます。

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

を削除します。 `<div class="helloworld" ...></div>` 送信元：

```
ui.frontend/src/main/webpack/static/index.html
```

を削除します。 `<helloworld>` コンポーネントインスタンス定義元：

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
