---
title: AEM GraphQL の画像パスの解決
description: null
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: null
thumbnail: KT-.jpg
mini-toc-levels: 2
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '44'
ht-degree: 2%

---


# AEM GraphQL の画像パス

https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/how-to/images.html

## パスの要件


| クライアント | ドメインをAEMと共有 | AEMとドメインを分離 | ドメインがありません ( 例： モバイル（サーバー側） |
|----------------------------:|:-----------------------:|:------------------------:|:-----------------------------------:|
| リソースパスが必要です | ✘ | ✔ | ✔ |

## AEM Host を使用

```graphql
query ($path: String!) {
  adventureByPath(_path: $path) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _path
        }
      }
    }
  }
}
```

```graphql
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg",
        }
      }
    }
  }
}
```

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_ORIGIN = env.process.REACT_APP_AEM_ORIGIN; // https://publish-p123-e456.aemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

return (
    {/* <img src="https://publish-p123-e456.aemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg"/>  */}
    <img src={AEM_ORIGIN + data.adventureByPath.item.primaryImage._path }>
)
```

## AEM Host を使用

```graphql
query ($path: String!) {
  adventureByPath(_path: $path) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _publishUrl
        }
      }
    }
  }
}
```

```graphql
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_publishUrl": "https://publish-p123-e789.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg"
        }
      }
    }
  }
}
```

```javascript
...
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

return (
    {/* <img src="https://publish-p123-e789.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg"/>  */}
    <img src={AEM_ORIGIN + data.adventureByPath.item.primaryImage._publishUrl }>
)
```
