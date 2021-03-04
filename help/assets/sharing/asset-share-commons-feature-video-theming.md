---
title: アセット共有コモンズのテーマ設定の概要
description: 資産共有コモンズに関する機能的および技術的な理解のための資料
version: 6.3, 6.4, 6.5
topic: コンテンツ管理
role: デベロッパー
level: 中間
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 4%

---


# アセット共有コモンズでのテーマ設定の概要{#asset-share-commons-theme}

アセット共有コモンズでのテーマ設定について簡単に説明します。 このビデオでは、カスタムカラースキームを使用して新しいテーマを作成するプロセスについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

このビデオでは、アセット共有コモンズのダークテーマに基づいて新しいテーマが作成されます。 カラースキームは、サイトのルック&amp;フィールを一貫させるためにカスタムロゴと一致します。

## テーマ変数

```java
/*******************************
         Theme Variables
*******************************/
 
/* Below is a condensed list of variables for easy updating of the theme */
 
/* Color Palette */
@primaryColor      : #560a63;
@secondaryColor    : #fc3096;
 
/* Text */
@fontName          : 'LatoWeb';
@fontSmoothing     : antialiased;
@headerFont        : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@pageFont          : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@textColor         : #D0D0D0;
 
 
/* Backgrounds */
@pageBackground      : #3C3C3C;
@sideBarBackground   : #363636;
 
/* Links */
@linkColor           : #FFFFFF;
@linkHoverColor      : @secondaryColor;
 
/* Buttons */
@buttonPrimaryColor      : #560a63; /* must be a value to prevent variable recursion*/
@buttonPrimaryColorHover : saturate(darken(@buttonPrimaryColor, 5), 10, relative);
 
/* Filters (Checkboxes,radio buttons, toggle, slider, dropdown, accordion colors)*/
@filterPrimaryColor      : @secondaryColor;
@filterPrimaryColorFocus : saturate(darken(@filterPrimaryColor, 5), 10, relative);
 
/* Label */
@labelPrimaryColor       : @primaryColor;
@labelPrimaryColorHover  : saturate(darken(@labelPrimaryColor, 5), 10, relative);
 
/* Menu */
@menuPrimaryColor        : @primaryColor;
 
/* Message */
@msgPositiveBackgroundColor : @secondaryColor;
@msgNegativeBackgroundColor : @red;
@msgInfoBackgroundColor: @midBlack;
@msgWarningBackgroundColor: @yellow;
```

[カスタムクライアントライブラリテーマ](assets/asc-theme-demo.zip)のダウンロード

## その他のリソース{#additional-resources}

* [アセット共有コモンズリリースのダウンロード](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0以降のリリースのダウンロード](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)