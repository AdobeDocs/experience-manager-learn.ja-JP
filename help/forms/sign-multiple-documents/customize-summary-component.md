---
title: 概要コンポーネントのカスタマイズ
description: 概要手順のコンポーネントを拡張して、パッケージ内の次のフォームに移動する機能を含めます。
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 0%

---


# 概要手順のカスタマイズ

概要手順のコンポーネントは、署名済みフォームをダウンロードするためのリンクと共にフォーム送信の概要を表示するために使用します。 概要手順は、通常、フォームの最後のパネルに配置します。
この使用例では、初期設定のSummaryコンポーネントに基づいて新しいコンポーネントを作成し、カスタムclientlibを含める機能を拡張しました。

このコンポーネントは、「Sign Multiple Form」というラベルで識別されます。

次のスクリーンショットは、署名式の完了時にメッセージを表示するために作成された新しいコンポーネントを示しています

![概要構成要素](assets/summary.PNG)

新しいコンポーネントは、既製の概要コンポーネントに基づいています。
![component-prop](assets/componentprop.PNG)

署名用に次のフォームに移動するためのボタンが追加されました
![テンプレートコード](assets/template-code.PNG)

summary.jspには次のコードがあります。 カテゴリID **getnextform**&#x200B;で識別されるクライアントライブラリへの参照が含まれます。

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

カスタム概要コンポーネントは、[ここから](assets/custom-summary-step.zip)ダウンロードできます


