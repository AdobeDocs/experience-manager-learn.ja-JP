---
title: Asset Share Commonsのテーマの概要
description: Assets Share Commonsの機能と技術の両方に関する資料
version: 6.3, 6.4, 6.5
topic: コンテンツ管理
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 3%

---


# Asset Share Commonsのテーマの概要 {#asset-share-commons-theme}

Asset Share Commonsでのテーマの概要です。 このビデオでは、カスタムカラースキームを使用して新しいテーマを作成するプロセスを順を追って説明します。

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

このビデオでは、 Asset Share Commons Darkテーマに基づいて新しいテーマが作成されます。 カラースキームは、カスタムロゴと一致し、サイトのルックアンドフィールを統一します。

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

* [Asset Share Commonsリリースのダウンロード](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0+リリースのダウンロード](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)