---
title: AEM コンテンツフラグメントの翻訳サポート
description: コンテンツフラグメントを Adobe Experience Manager でローカライズおよび翻訳する方法を説明します。 コンテンツフラグメントに関連付けられている混在メディアアセットも、抽出および翻訳の対象となります。
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: 6.4, 6.5, Cloud Service
kt: 201
thumbnail: 18131.jpg
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '250'
ht-degree: 100%

---

# AEM コンテンツフラグメントの翻訳サポート {#translation-support-content-fragments}

コンテンツフラグメントを Adobe Experience Manager でローカライズおよび翻訳する方法を説明します。 コンテンツフラグメントに関連付けられている混在メディアアセットも、抽出および翻訳の対象となります。

>[!VIDEO](https://video.tv.adobe.com/v/18131?quality=12&learn=on)

## コンテンツフラグメントの翻訳のユースケース {#content-fragment-translation-use-cases}

コンテンツフラグメントとは、外部翻訳サービスに送信するために AEM で抽出される認識済みのコンテンツタイプです。 次のユースケースが標準でサポートされています。

1. コンテンツフラグメントは、[言語コピーと翻訳用にアセットコンソールで直接選択](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html?lang=ja)することができます。
2. Sites ページで参照されるコンテンツフラグメントは適切な言語フォルダーにコピーされ、言語コピー用に Sites ページが選択されると翻訳用に抽出されます。
3. コンテンツフラグメント内に埋め込まれたインラインメディアアセットは、抽出および翻訳の対象となります。
4. コンテンツフラグメントに関連付けられたアセットコレクションは、抽出および翻訳の対象となります。

## 翻訳ルールエディター {#translation-rules-editor}

Experience Managerの翻訳動作は、**翻訳ルールエディター**&#x200B;で更新できます。翻訳を更新するには、[http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html) で&#x200B;**ツール**／**一般**／**翻訳の設定**&#x200B;に移動します。

標準設定では、リソースタイプが `core/wcm/components/contentfragment/v1/contentfragment` の `fragmentPath` でコンテンツフラグメントを参照します。  `v1/contentfragment` から継承されるすべてのコンポーネントは、デフォルトの設定で認識されます。

![翻訳ルールエディター](assets/translation-configuration.png)
