---
title: ページコンポーネントと新しいアダプティブフォームテンプレートの関連付け
description: 新しいページコンポーネントの作成
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
exl-id: 7b2b1e1c-820f-4387-a78b-5d889c31eec0
duration: 35
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '150'
ht-degree: 100%

---

# ページコンポーネントとテンプレートの関連付け

次に、ページコンポーネントと新しいアダプティブフォームテンプレートの関連付けを行います。これにより、新しいテンプレートに基づくアダプティブフォームがレンダリングされるたびに、ページコンポーネント内のコードが確実に実行されます。このチュートリアルのために、**StoreAndRestoreFromAzure** という新しいアダプティブフォームテンプレートを **AzurePortalStorage** フォルダーに作成しました。
/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr:content ノードに移動し、次のプロパティを追加して変更内容を保存します。

| **プロパティ名** | **プロパティタイプ** | **プロパティ値** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | 文字列 | azureportalpagecomponent/component/page/storeandfetch |

/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/structure/jcr:content ノードに移動し、次のプロパティを追加して変更内容を保存します。
| **プロパティ名** | **プロパティタイプ** | **プロパティ値** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | 文字列 | azureportalpagecomponent/component/page/storeandfetch |


## 次の手順

[Azure ストレージとの統合の作成](./create-fdm.md)
