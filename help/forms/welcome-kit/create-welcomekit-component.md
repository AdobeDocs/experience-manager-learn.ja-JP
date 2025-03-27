---
title: ウェルカムキットコンポーネントを作成する
description: 送信されたフォームデータに基づいてアセットをダウンロードするためのリンクを含む、AEM Sites のページを作成します。
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 66496f0e-c121-4b6d-b371-084393ece3ca
duration: 20
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '74'
ht-degree: 100%

---

# ウェルカムキットコンポーネント

エンドユーザーがダウンロードできるページ内のアセットを一覧表示するページコンポーネントを作成しました。個々のアセットへのパスは、**パス**&#x200B;というプロパティに保存されます。送信されたフォームデータによって、含めるアセットが決まります。

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
