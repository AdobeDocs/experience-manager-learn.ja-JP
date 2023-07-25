---
title: ページコンポーネントを新しいアダプティブフォームテンプレートに関連付ける
description: 新しいページコンポーネントの作成
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 7%

---

# ページコンポーネントとテンプレートの関連付け

次の手順では、ページコンポーネントを新しいアダプティブフォームテンプレートに関連付けます。 これにより、新しいテンプレートに基づくアダプティブフォームがレンダリングされるたびに、ページコンポーネント内のコードが実行されます。 このチュートリアルの目的では、新しいアダプティブフォームテンプレート「 **StoreAndRestoreFromAzure** が **AzurePortalStorage** フォルダー。
/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr:content ノードに移動し、次のプロパティを追加して変更を保存します。

| **プロパティ名** | **プロパティタイプ** | **プロパティの値** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | 文字列 | azureportalpagecomponent/component/page/storeandfetch |

/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/structure/jcr:content ノードに移動し、次のプロパティを追加して変更を保存します。
| **プロパティ名**  | **プロパティタイプ** | **プロパティ値**                                    | |—|—|—| | sling:resourceType |文字列 | azureportalpagecomponent/component/page/storeandfetch |


## 次の手順

[Azure ストレージとの統合の作成](./create-fdm.md)
