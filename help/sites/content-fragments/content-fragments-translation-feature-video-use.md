---
title: AEMコンテンツフラグメントの翻訳のサポート
description: コンテンツフラグメントをAdobe Experience Managerでローカライズおよび翻訳する方法を説明します。 コンテンツフラグメントに関連付けられている混在メディアアセットも、抽出および翻訳の対象となります。
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: 6.3, 6.4, 6.5, Cloud Service
kt: 201
thumbnail: 18131.jpg
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
source-git-commit: 1c2ee81c0d262f9e3f92f4907aba8e8787ce729f
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 2%

---

# AEMコンテンツフラグメントの翻訳のサポート {#translation-support-content-fragments}

コンテンツフラグメントをAdobe Experience Managerでローカライズおよび翻訳する方法を説明します。 コンテンツフラグメントに関連付けられている混在メディアアセットも、抽出および翻訳の対象となります。

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## コンテンツフラグメント翻訳の使用例 {#content-fragment-translation-use-cases}

コンテンツフラグメントは、外部翻訳サービスに送信するためにAEMで抽出される、認識されるコンテンツタイプです。 次の使用例が標準でサポートされています。

1. コンテンツフラグメントは、 [言語コピーと翻訳用にアセットコンソールで直接選択された状態](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).
2. Sites ページで参照されるコンテンツフラグメントは、適切な言語フォルダーにコピーされ、言語コピー用に Sites ページが選択されると、翻訳用に抽出されます。
3. コンテンツフラグメント内に埋め込まれたインラインメディアアセットは、抽出および翻訳の対象となります。
4. コンテンツフラグメントに関連付けられたアセットコレクションは、抽出および翻訳の対象となります。

## 翻訳ルールエディター {#translation-rules-editor}

Experience Managerの翻訳動作は、 **翻訳ルールエディター**. 翻訳を更新するには、に移動します。 **ツール** > **一般** > **翻訳設定** 時刻 [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

標準設定では、次の場所にあるコンテンツフラグメントを参照します。 `fragmentPath` リソースタイプが `core/wcm/components/contentfragment/v1/contentfragment`. から継承されるすべてのコンポーネント `v1/contentfragment` はデフォルトの設定で認識されます。

![翻訳ルールエディター](assets/translation-configuration.png)
