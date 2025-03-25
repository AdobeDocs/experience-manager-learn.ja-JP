---
title: AEM Forms での計算済みフォームデータモデル要素の作成
description: 計算済みフォームデータモデル要素の作成
feature: Workflow
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 1f6f1bb6-3437-4fae-b5a1-698ab357ff23
last-substantial-update: 2020-09-10T00:00:00Z
duration: 374
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 100%

---

# AEM Forms での計算済みフォームデータモデル要素の作成{#creating-computed-form-data-model-elements-in-aem-forms}

計算済みフォームデータモデル要素を使用すると、操作結果を 1 つ以上のフォームデータモデル要素に保存できます。 例えば、年給フィールドに対して数学演算を実行して月給を計算し、保存する場合があります。これを行うには、年給を 12 で割り、結果を monthlySalary という計算済みフォームデータモデル要素に保存します。

計算済みフォームデータモデルを作成するもう 1 つの例は、2 つ以上のフォームデータモデル要素を連結する場合です。 例えば、state と zip のフォームデータモデル要素を、2 つの要素の間にハイフンを使用して連結できます。

次のスクリーンショットは、計算済み要素 StateandZip および monthlySalary を示しています

![computedfdmelement](assets/computedfdmelement.gif)

## 月給計算済み要素の作成

>[!VIDEO](https://video.tv.adobe.com/v/23855?quality=12&learn=on)

### StateandZip 計算済み要素の作成

>[!VIDEO](https://video.tv.adobe.com/v/23856?quality=12&learn=on)
