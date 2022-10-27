---
title: ネストされたパネルの操作
description: ネストされたパネルの操作
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9335
exl-id: c60d019e-da26-4f67-8579-ef707e2348bb
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 0%

---

# 複数のパネルを含むナビゲーションタブ

フォームがナビゲーションタブから離れ、いずれかのタブに複数のパネルがある場合、子パネルのタイトルを非表示にし、引き続きこれらのタブのタブと子パネル間を移動できるようにする必要があります

## アダプティブフォームを作成

次の構造を持つアダプティブフォームを作成します。 ルートパネルには子パネルがあり、左側にタブとして表示されます。 これらの「**タブ**」には、追加の子パネルがあります。 例えば、[ ファミリ ] タブには、[ 配偶者 ] と [ 子 ] という 2 つの子パネルがあります。

FormContainer の下には、「前へ」ボタンと「次へ」ボタンを含むツールバーも追加されます

![toolbar-spacing](assets/multiple-panels.png)



このフォームのデフォルトの動作では、左側にすべてのパネルを表示し、次のボタンをクリックしてタブ間を移動します。

このデフォルトの動作を変更するには、次の操作を行う必要があります

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=9&learn=on)


次のコードを **次へ** ボタンを使用して、コードエディターで

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

次のコードを **前へ** ボタンを使用して、コードエディターで

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

上記のコードは、各タブのタブと子パネル間を移動する際に役立ちます。

## 子パネルのタイトルを非表示にする

スタイルエディターを使用して、タブの子パネルのタイトルを非表示にします。

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=9&learn=on)

>[!NOTE]
>
>この記事で説明されている機能は、最後のタブでは機能しません。 例えば、「アドレス」タブに子パネルがある場合、この機能は機能しません。
