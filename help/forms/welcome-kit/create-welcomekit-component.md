---
title: ウェルカムキットコンポーネントを作成する
description: 送信されたフォームデータに基づいてアセットをダウンロードするためのリンクを含むAEMサイトページを作成します。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 0e27907066c7d688549a980ccd17b3f17d74b60b
workflow-type: tm+mt
source-wordcount: '74'
ht-degree: 0%

---

# ウェルカムキットコンポーネント

エンドユーザーがダウンロードできるページ内のアセットのリストを表示するページコンポーネントが作成されました。 個々のアセットへのパスは、 **パス**. 送信されたフォームデータによって、含めるアセットが決まります。

次のコードは、ページ上のアセットをリストします。

```html
   <p class="cmp-press-kit__press-kit-size">
        Welcome kit contains ${pressKit.assets.size} assets.
    </p>
<ul class="cmp-press-kit__asset-list" data-sly-list.asset="${pressKit.assets}">
    <li class="cmp-press-kit__asset-item">
        <div class="cmp-press-kit__asset " >
            <div class="cmp-press-kit__asset-content">
                <img class="cmp-press-kit__asset-image" src="${asset.path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png" alt="${asset.name}"/>
                <p class="cmp-press-kit__asset-title">${asset.title}</p>
            </div>
            <div class="cmp-press-kit__asset-actions">
                <a class="cmp-press-kit__asset-download-button" href="${asset.path}">Download</a>
            </div>
        </div>
    </li>
</ul>
</sly>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!ready}"></sly>
```


