---
title: 概要コンポーネントのカスタマイズ
description: 概要ステップコンポーネントを拡張して、パッケージ内の次のフォームに移動する機能を含めます。
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 2%

---


# 概要ステップのカスタマイズ

概要ステップコンポーネントは、署名済みフォームをダウンロードするためのリンクと共にフォーム送信の概要を表示するために使用します。 概要手順は、通常、フォームの最後のパネルに配置されます。
この使用例では、標準の概要コンポーネントに基づいて新しいコンポーネントを作成し、カスタムclientlibを含める機能を拡張しました。

このコンポーネントは、「複数のフォームに署名」というラベルで識別されます

次のスクリーンショットは、署名式の完了時にメッセージを表示するために作成された新しいコンポーネントを示しています

![概要コンポーネント](assets/summary.PNG)

新しいコンポーネントは、標準の概要コンポーネントに基づいています。
![component-prop](assets/componentprop.PNG)

署名用に次のフォームに移動するボタンが追加されました
![template-code](assets/template-code.PNG)

summary.jspには次のコードが含まれます。 カテゴリID **getnextform**&#x200B;で識別されるクライアントライブラリへの参照を持ちます。

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

カスタム概要コンポーネントは、[ここから](assets/custom-summary-step.zip)ダウンロードできます。


