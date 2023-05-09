---
title: 概要コンポーネントのカスタマイズ
description: 概要ステップコンポーネントを拡張して、パッケージ内の次のフォームに移動する機能を含めます。
feature: Adaptive Forms
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
exl-id: fb68579d-241c-414d-92f4-13194f4d1923
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 96%

---

# 概要ステップのカスタマイズ

概要ステップコンポーネントは、署名済みフォームをダウンロードするためのリンクと共に、フォーム送信の概要を表示するために使用します。通常、概要ステップはフォームの最後のパネルに配置されます。
このユースケースでは、標準の概要コンポーネントに基づいて新しいコンポーネントを作成し、カスタム clientlib を含める機能を拡張しました。

このコンポーネントは、複数のフォームに署名するラベルで識別されます

次のスクリーンショットは、署名行為の完了時にメッセージを表示するために作成された新しいコンポーネントを示しています

![概要コンポーネント](assets/summary.PNG)

新しいコンポーネントは、標準の概要コンポーネントに基づいています。
![component-prop](assets/componentprop.PNG)

署名用に次のフォームに移動するためのボタンが追加されました
![template-code](assets/template-code.PNG)

summary.jsp には次のコードが含まれます。 カテゴリ ID で識別されるクライアントライブラリへの参照を持っています&#x200B;**getnextform**

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

カスタム概要コンポーネントは、[ここからダウンロード](assets/custom-summary-step.zip)できます

## 次の手順

[署名用の次のフォームを取得します](./create-client-lib.md)