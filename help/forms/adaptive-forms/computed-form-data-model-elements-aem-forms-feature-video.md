---
title: AEM Formsでの計算済みフォームデータモデル要素の作成
description: 計算済みフォームデータモデル要素の作成
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 1f6f1bb6-3437-4fae-b5a1-698ab357ff23
last-substantial-update: 2020-09-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# AEM Formsでの計算済みフォームデータモデル要素の作成{#creating-computed-form-data-model-elements-in-aem-forms}

計算済みフォームデータモデル要素を使用すると、操作結果を 1 つ以上のフォームデータモデル要素に保存できます。 たとえば、月給フィールドに対して数学演算を実行して月給を計算し、保存する場合があります。 これをおこなうには、給与を 12 で割り、結果を monthlySalary という計算済みフォームデータモデル要素に保存します。

計算済みフォームデータモデルを作成するもう 1 つの例は、2 つ以上のフォームデータモデル要素を連結することです。 例えば、state と zip のフォームデータモデル要素を、2 つの要素の間にハイフンを持つ連結できます。

次のスクリーンショットは、計算済み要素 StateandZip および monthlySalary を示しています

![computedfdmelement](assets/computedfdmelement.gif)

## 月給計算要素の作成

>[!VIDEO](https://video.tv.adobe.com/v/23855?quality=9&learn=on)

### StateandZip 計算要素の作成

>[!VIDEO](https://video.tv.adobe.com/v/23856/?quality=9&learn=on)
