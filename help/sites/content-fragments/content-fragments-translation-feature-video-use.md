---
title: AEMコンテンツフラグメントの翻訳のサポート
description: コンテンツフラグメントをAdobe Experience Managerでローカライズおよび翻訳する方法を説明します。 コンテンツフラグメントに関連付けられた混在メディアアセットも、抽出および翻訳の対象となります。
feature: コンテンツフラグメント、マルチサイトマネージャー
topic: ローカリゼーション
role: Business Practitioner
level: Intermediate
version: 6.3, 6.4, 6.5, cloud-service
kt: 201
thumbnail: 18131.jpg
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 2%

---


# AEMコンテンツフラグメントの翻訳サポート{#translation-support-content-fragments}

コンテンツフラグメントをAdobe Experience Managerでローカライズおよび翻訳する方法を説明します。 コンテンツフラグメントに関連付けられた混在メディアアセットも、抽出および翻訳の対象となります。

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## コンテンツフラグメント翻訳の使用例{#content-fragment-translation-use-cases}

コンテンツフラグメントは、外部翻訳サービスに送信するAEM抽出のコンテンツタイプとして認識されます。 次のようないくつかの使用例が標準でサポートされています。

1. コンテンツフラグメントは、アセットコンソールで直接選択して、言語コピーと翻訳をおこなうことができます
2. サイトページで参照されるコンテンツフラグメントは、適切な言語フォルダーにコピーされ、翻訳用に抽出されます（言語コピー用にサイトページが選択されている場合）。
3. コンテンツフラグメント内に埋め込まれたインラインメディアアセットは、抽出および翻訳の対象となります。
4. コンテンツフラグメントに関連付けられたアセットコレクションは、抽出および翻訳の対象となります

## 翻訳ルールエディター {#translation-rules-editor}

Experience Managerの翻訳動作は、**翻訳ルールエディター**&#x200B;を使用して更新できます。 翻訳を更新するには、**ツール** / **一般** / **翻訳設定**([http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html))に移動します。

標準設定では、リソースタイプ`core/wcm/components/contentfragment/v1/contentfragment`の`fragmentPath`にあるコンテンツフラグメントを参照します。 `v1/contentfragment`から継承されるすべてのコンポーネントは、デフォルトの設定で認識されます。

![翻訳ルールエディター](assets/translation-configuration.png)
